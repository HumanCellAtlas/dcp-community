### DCP PR:

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# RFC Deletion of data in the DCP

## Summary
Establish a secure and controlled process for deleting data from the Data Store.


## Author(s)

 [Trent Smith](mailto:tsmith12@ucsc.edu)
 
 [Hannes Schmidt](mailto:hannes@ucsc.edu)

## Shepherd
 `[Name](mailto:username@example.com)`

## Motivation

One of the DCP's primary objectives is to ensure the durability of the
(meta)data it contains and to maintain a complete record of the changes made to
it. The DCP achieves this by never overwriting a (meta)data file but instead
treating an update to such a file as the addition of a new version of that
file. Instead of physically removing a file version, the DCP hides that
file version behind a deletion marker.

Under certain circumstances however, the *physical deletion* of a file version
is required. The first section of this specification describes the
circumstances and mechanisms for logical and physical deletion of files and the
bundles that contain them. The second section details the process of deleting data from the DSS.

### User Stories

* As a user of the DSS, I would like to know what bundles have been deleted, so I can keep my personal index up-to-date. 
* As a data wrangler of the DSS, I need to remove data from the DSS, because there was no consent from the donor to share the data.

## Detailed Design

### Reasons

Possible reasons for deletion of (meta)data are

* consent to share (meta)data derived from a donor's specimens is withdrawn by
  that donor after the (meta)data was submitted to DCP
  
* that consent was never given but this fact is revealed only after the
  (meta)data was submitted to the DCP
  
* storing or processing the (meta)data causes a serious disruption of service
  within the DCP

* the (meta)data was submitted to the DCP by a malicious party in violation of
  the DCP's data sharing agreement. This includes (meta)data that violates the
  laws or regulations in a jurisdiction applicable to the DCP or the (meta)data
  it stores

This is not an exhaustive list, and more reason maybe discovered later.
 
Whether any of the reasons require a physical or logical deletion is at the
discretion of the administrator performing the deletion. That discretion is
guided by a standard operating procedure (SOP).

### File Deletion API

Only **physical deletes** exist for the file deletion API. The file deletion API is a two part process. The first 
request will return the bundles that will be **logically deleted**
as a side effect and a confirmation code. The second request must include the confirmation code in order to begin the
deletion process. The request follows this swagger:

```yml
      /file/{uuid}:
        delete:
          security:
            - dcpAuth: []
          summary: Delete a file version
          description: >
            Delete the file with the given UUID and version. This deletion is applied across all replicas.
          parameters:
            - name: uuid
              in: path
              description: A RFC4122-compliant ID for the bundle.
              required: true
              type: string
              pattern: "[A-Za-z0-9]{8}-[A-Za-z0-9]{4}-[A-Za-z0-9]{4}-[A-Za-z0-9]{4}-[A-Za-z0-9]{12}"
            - name: replica
              in: query
              description: Replica to write to first.
              required: false
              type: string
              enum: [aws, gcp]
              default: aws
            - name: version
              in: query
              description: Timestamp of bundle creation in DSS_VERSION format.
              required: true
              type: string
            - name: json_request_body
              in: body
              required: true
              schema:
                type: object
                properties:
                  reason:
                    description: the reason for the deletion.
                    type: string
                    enum: [consent_withdrawn, consent_absent, service_disruption, legal]
                  details
                    description: User-friendly reason for the bundle or timestamp-specfic bundle deletion.
                    type: string
                required:
                  - reason
      responses:
        200:
          description: Deletion pending
          schema:
            type: object
              properties:
                bundles:
                  type: array
                  description: list of bundles effected by deletion
                  items:
                  type: string
                confirmation:
                  type: string
                  description: a key used to confirm the deletion operation
        201:
          description: Deletion confirmed
          schema:
            type: object
              properties:
                files:
                  type: array
                  description: list of files effected by deletion
                  items:
                    type: string
                bundles:
                  type: array
                  description: list of bundles effected by deletion
                  items:
                  type: string
        401:
          description: Unauthorized user it attempting this action.
            ...
```

A user must have explicit permission to perform the file deletion operation.

### Bundle Deletion API

The bundle deletion API is a two part process. The first request will return the bundle and files that will be affect 
as a consequence of this opertion, and a confirmation code. Both logical and physical deletions can be performed through
this API. For **logical deletions** only a list bundles of bundles is returned in the response since files are unaffected
by the logical deletion of a bundle. For a **physical deletion** a list of affected files and bundles is returned.
The second request must include the confirmation code in order to begin the deletion process. The request follows this 
swagger:

```yml
      /bundle/{uuid}:
        delete:
          security:
            - dcpAuth: []
          summary: Delete a bundle version
          description: >
            Delete the bundle with the given UUID and version. This deletion is applied across all replicas.
          parameters:
            - name: uuid
              in: path
              description: A RFC4122-compliant ID for the bundle.
              required: true
              type: string
              pattern: "[A-Za-z0-9]{8}-[A-Za-z0-9]{4}-[A-Za-z0-9]{4}-[A-Za-z0-9]{4}-[A-Za-z0-9]{12}"
            - name: replica
              in: query
              description: Replica to write to first.
              required: false
              type: string
              enum: [aws, gcp]
              default: aws
            - name: version
              in: query
              description: Timestamp of bundle creation in DSS_VERSION format.
              required: true
              type: string
            - name: physical
              in: query
              require: true
              description: True for a physical deletion and false for a logical deletion
              type: bool
            - cc:
              in: query
              description: the confirmation code that must be return to confirm the deletion of a file.
              required: false
              type: string
            - name: json_request_body
              in: body
              required: true
              schema:
                type: object
                properties:
                  reason:
                    description: the reason for the deletion.
                    type: string
                    enum: [consent_withdrawn, consent_absent, service_disruption, legal]
                  details
                    description: User-friendly reason for the bundle or timestamp-specfic bundle deletion.
                    type: string
                required:
                  - reason
      responses:
        200:
          description: Deletion pending
          schema:
            type: object
              properties:
                files:
                  type: array
                  description: list of files effected by deletion
                  items:
                    type: string
                bundles:
                  type: array
                  description: list of bundles effected by deletion
                  items:
                  type: string
                confirmation:
                  type: string
                  description: a key used to confirm the deletion operation
        201:
          description: Deletion confirmed
          schema:
            type: object
              properties:
                files:
                  type: array
                  description: list of files effected by deletion
                  items:
                    type: string
                bundles:
                  type: array
                  description: list of bundles effected by deletion
                  items:
                  type: string
        401:
          description: Unauthorized user it attempting this action.
            ...
```
    
A user must have explicit permission to perform a **logical deletion** of a bundle. For a **physcial deletion** of a
bundle the user must have explicit permission to **physically delete** files and bundles.

(!) The deletion of a bundle does not handle the deletion of secondary analysis bundles referencing this bundle via the 
links metadata field.

### Process

Once it is determined that a (meta)data file needs to be deleted –
the details of this determination are the scope of an SOP – the following
procedures takes place: 

1) A user identifies the Data Store file or bundle bundle to be deleted.

2) The user makes a request to the appropriate API to retrieve the confirmation code. A list of bundles and files
   affected is also returned in the request.

3) The users makes a second request that included the confirmation code. This starts the deletion process.

4) **Logical Deletion**: After the deletion request has been confirmed the DSS places a **bundle tombstone** in the
   underlying storage system(S3 or GCP bucket) for all bundles listed in the orignal request. The tombstone's content is
    as follows:
   ```JSON
   { "reason": "<reason>",
     "details": "<additional info>",
     "email": "<requester's email>"
   }
   ```
   A bundle version tombstone will cause `GET /bundle/{uuid}` to return a 404 for that bundle version. If the tombstoned
   bundle is the latest version then a request for `GET /bundle/{uuid}` without a version will
   also returns a 404. If bundle tombstone does not specify a bundle then all request for `GET /bundle/{uuid}` will
   return 404. The same applies to HEAD requests, both versioned and
   unversioned. The file versions referenced by the deleted bundle version are
   not immediately affected by the deletion and remain accessible.
   
   A notification is sent out to subscribers of datastore deletions when a new bundle tombstone is created, so they can 
   update their index appropriately. If the the bundle was logically deleted and later physically deleted a second
   identical notification will be sent to subscribers. The subscribers must be able to handle these notifications 
   idempotently. 
   
   If the bundle is marked for physical deletion then the bundle and associated files are also added to 
   a deletion queue for regularly schedule **Physical Deletion**. 
   
   If a file is marked for **physical deletion** then it is placed in the deletion queue and all associated bundles are 
   **logically deleted**.

4) **Physical Deletion**: On a regular schedule the deletion queue is processed by code running within a secure boundary. An administrator 
   may manually invoke the deletion process or wait for it run at it's scheduled time once daily.
   The deletion daemon is responsible for performing **Physical Deletes** of data from the deletion queue. 
   A physical delete makes all files associated with a bundle or bundle version inaccessible. This will make HEAD and 
   GET requests against those file versions return a 404.
   
   A log entry is produced when ever the deletion daemon is run. It contains information on what bundles and files
   have been logically deleted, the reason, the administrator who executed the deletion, and info about the deletion markers. 
   The same information is stored in the **Deletion Table**. The deletion table tracks all bundles and files currently
   pending permanent deletion. The Deletion Table contains enough information to reverse Physical Deletes before 
   the data is Permanently deleted.
   
   A daily digest of bundles and files that are scheduled for deletion are sent to DCP administrator 24 hours prior to the data
   being permanently deleted.

5) Data that has been physical deleted remain in the bucket for 7 days before being permanently deleted from the bucket.
   This grace period allows the deletion to be reversed within that time period. After the grace period the data will 
   be permanently deleted.
 
### Primary indexes

Bundle tombstones are indexed verbatim by the Data Store. The JSON schema
for deletion markers is defined such that there are no conflicts with the
top-level keys in the index documents for regular bundles (`files`, `manifest`,
`uuid`, `version` etc). Bundle tombstones are therefore compatible with
regular bundle documents with respect to in the same index even if
Elasticsearch's dynamic mapping is enabled.

### Replication

When a bundle or file is deleted it is removed from all replicas.

### **WIP** Consistency

It is the users's responsibility to enumerate all bundles referring to
a file that is to be deleted and issue a delete requests for each of
them. If the users misses such a bundle (a *dangling bundle*), users
requesting `GET /bundle` for a dangling bundle will be able to retrieve its
manifest. Likewise, `POST /search` requests will return the metadata for
dangling bundles. Secondary indexes like the Data Browser (Orange Box) will
continue to hand out (meta)data for dangling bundles. If users request `GET
/files` for a file contained in a dangling bundle and the requested file was
referenced by another already logically (physically) deleted bundle, the Data
Store will respond with a 404 (500) status code.

The administrator can initiate a consistency check against a storage replica to
list dangling bundles. The administrator can then issue more `DELETE /bundles`
requests for those dangling bundles.

### Secondary indexes

The Data Store can optionally notify subscribers about the creation of a bundle
tombstone. Secondary index services need to register a subscription using
a query that matches deletion markers. When they are notified about a deletion
marker, they need to take the necessary actions to remove the data from their
database or index.

### Derived data

The metadata in bundles containing derived data must reference the input bundle
versions. Administrators are responsible for tracking down data bundles with
data derived from data in deleted bundles and to issue deletion requests for
them.

### Tiered storage

Physical deletion of blobs is effective across all tiered storage classes in
the underlying blob store. Additional cost may be incurred for deleting data recently moved to a
slower access storage tier. Deletion occur There is no additional delays cause when deleting from a 
tiered storage system such as AWS glacier.

### Undoing logical deletions

A bundle can be restored using `restore_bundle.py <uuid.<version>`**WIP** if has not been physically deleted. This process
uses the info from the deletion table to removes tombstones, and prevent permanent deletion. Subscribers of new 
bundles will receive a notification that a new bundle has arrived.

Deletion Table Entry Example:

|Bundle.Version|admin|reason|AWS Deletion Markers (key,ID)|GCP Previous Generations (key, previous generation)|
|--------------|-----|------|--------------------|------------------------|
|1234-2134-3145|admin@email.com|consent|(file.obj, 1234), ...| (file.obj, 4321), ...|

A bundle is restored in AWS by deleting the deletion marker and the tombstone for each files in the bundle.
In GCS a bundle can be restored by by storing a copy of the orginal files in the bucket with the same name, and deleting 
the tombstones.

### Resources

* [AWS S3 Versioning](https://docs.aws.amazon.com/AmazonS3/latest/dev/Versioning.html),
* [AWS S3 Deleting Object Versions](https://docs.aws.amazon.com/AmazonS3/latest/dev/DeletingObjectVersions.html),
* [AWS S3 Restoring Previous Versions](https://docs.aws.amazon.com/AmazonS3/latest/dev/RestoringPreviousVersions.html),
* [AWS Object Lifecycle Management](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html),
* [Amazon DynamoDB TTL](https://aws.amazon.com/about-aws/whats-new/2017/02/amazon-dynamodb-now-supports-automatic-item-expiration-with-time-to-live-ttl/)
* [GCS Object Versioning](https://cloud.google.com/storage/docs/object-versioning),
* [GCS Object Lifecycle Management](https://cloud.google.com/storage/docs/lifecycle)
* [HumanCellAtlas/data-store#1662 Explore Bucket Versioning for Deletion](https://github.com/HumanCellAtlas/data-store/issues/1662)
* [HumanCellAtlas/data-store#1663 Deletion Daemon Design](https://github.com/HumanCellAtlas/data-store/issues/1663)

### Open Issues

TODO: remove support for `all-versions` tombstone in Data Store.

TODO: implement consistency check for dangling bundles

TODO: implement object deletion and restoration for AWS anf GCP

TODO: implement delete_bundle.py

TODO: implement restore_bundle.py

TODO: enable bucket versioning and lifecycle rules for replicas.


### Acceptance Criteria [optional]

* When a bundle is tombstoned it is made immediately unavailable to users using search.
* Maintainers of external indices shall removed deleted data from its index when a bundle tombstone notification is received.
* Bundle files are still available until the deletion process has run.
* Bundles are not completely removed from the DSS until after the grace period has elapsed.

### Unresolved Questions

- *What aspects of the design do you expect to clarify further through the RFC review process?*
- *What aspects of the design do you expect to clarify later during iterative development of this RFC?*

### Drawbacks and Limitations [optional]

* This introduces the ability to permanently destroy data from the DSS. 
* There is no limit on the amount of data that can be rapidly permanently deleted.
* Permanently deletion does not occur until after the grace period has elapsed. This could
  be problematic if files need to be removed sooner. 
* If two bundles share files and only one bundle is deleted, this causes the remaining bundle to be incomplete.
* the ability to search for bundles that contain a files is depended on maintaining a current index of bundles and files.
  Presently Elasticsearch provides this indexing.
