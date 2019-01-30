### DCP PR:

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# RFC Deletion of data in the DCP

## Summary
Establish a secure and controlled process for deleting data from the Data Store.


## Author(s)

 `[Trent Smith](mailto:tsmith12@ucsc.edu)`
 
 `[Hannes Schmidt](mailto:hannes@ucsc.edu)`

## Shepherd
 `[Name](mailto:username@example.com)`

## Motivation

One of the DCP's primary objectives is to ensure the durability of the
(meta)data it contains and to maintain a complete record of the changes made to
it. The DCP achieves this by never overwriting a (meta)data file but instead
treating an update to such a file as the addition of a new version of that
file. And instead of physically removing a file version, the DCP hides that
file version behind a deletion marker.

Under certain circumstances however, the *physical deletion* of a file version
is required. The first section of this specification describes the
circumstances and mechanisms for logical and physical deletion of files and the
bundles that contain them.

### User Stories

* As a user of the DSS, I would like to know what bundles have been deleted, so I can keep my personal index up-to-date. 

* As a data wrangler of the DSS, I need to remove data from the DSS, because there was no consent from the donor to share the data.

## Detailed Design

### Reasons

Reasons for deletion of (meta)data are

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

Whether any of the reasons require a physical or logical deletion is at the
discretion of the administrator performing the deletion. That discretion is
guided by a standard operating procedure (SOP).

### Process

Once it is determined that a (meta)data file needs to be physically deleted –
the details of this determination are the scope of an SOP – the following
procedure takes place:

1) An administrator identifies the Data Store file versions to be deleted. The
   administrator then identifies the Data Store bundle versions referencing
   those file versions.

2) The administrator runs the admin CLI tool to queue the bundle for deletion using the bundles UUID and version.
   The bundle UUID and version is required for deletion. The admin CLI will return a list of files that will be 
   deleted and bundles that are effected by this deletion. Bundles may be affected if they share files or are derived 
   from the deleted bundle.
   
   ```bash
   delete_bundle.py <uuid>.<version> --reason [consent_withdrawn, consent_absent, service_disruption, legal] 
    --type [logical, physical] --details "<additional info>"
   
   Files to be deleted:
   <fileblobs>
   .
   .
   .

   Effected Bundles:
   <uuid>.<version>
   .
   .
   .

   confirm (yes or no)?

   ```

3) **Logical Deletion:** The DSS places a **bundle tombstone** in the
   underlying storage (S3 or GCP bucket). The tombstone's content is as follows:
   ```JSON
   { "reason": "<reason>",
     "details": "<additional info>",
     "admin_email": "<admin>"
   }
   ```
   The tombstone causes `GET /bundle/{uuid}` to return a 404. If `{version}` 
   signifies the latest version of the bundle
   identified by `{uuid}`, requests for `GET /bundle/{uuid}` without a version
   also return a 404. The same applies to HEAD requests, both versioned and
   unversioned. The file versions referenced by the deleted bundle version are
   not immediately affected by the deletion and remain accessible. The bundle and associated files are added to 
   a deletion queue for regularly schedule **Physical Deletion**.
   
   A notification is sent out to subscribers of datastore deletions when a new bundle tombstone is created, so they can 
   update their index appropriately.

4) On a regular schedule the deletion queue is processed by code running within a secure boundary. An administrator 
   may manually invoke the deletion process or wait for it run at it's scheduled time once daily.
   The deletion daemon is responsible for performing **Physical Deletes** of data from the deletion queue. 
   A physical delete makes all files associated with a bundle or bundle version inaccessible. This will make HEAD and 
   GET requests against those file versions return a 404.
   
   A log entry is produced when ever the deletion daemon is run. It contains information on what bundles and files
   have been logically deleted, the reason, the admin who executed the deletion, and info about the deletion markers. 
   The same information is stored in the **Deletion Table**. The deletion table tracks all bundles and files currently
   pending permanent deletion. The Deletion Table contains enough information to reverse Physical Deletes before 
   the data is Permanently deleted.

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

When a bundle is deleted it is removed from all replicas.

### Consistency

It is the administrator's responsibility to enumerate all bundles referring to
a file that is to be deleted and issue a delete requests for each of
them. If the administrator misses such a bundle (a *dangling bundle*), users
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

A bundle can be restored using `restore_bundle.py <uuid.<version>` if has not been physically deleted. This process
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

TODO: remove support for `all-versions` tombstone in Data Store

TODO: implement consistency check for dangling bundles

TODO: implement object deletion and restoration for AWS anf GCP

TODO: implement delete_bundle.py

TODO: implement restore_bundle.py

TODO: enable bucket versioning and lifecycle rules for replicas.

TODO: Remove `Bundle/Delete` API from DSS.

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
