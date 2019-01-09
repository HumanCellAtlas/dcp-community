### DCP PR:
# Metadata Caching
## Summary
Speed up the checkout process for metadata file by caching them in the checkout bucket.
## Author

[Trent Smith](mailto:tsmith12@ucsc.edu)

[Amar Jandu](mailto:ajandu@ucsc.edu)

## Motivation

Checking out small metadata files is slow, cost inefficient, and performed frequently. 
This becomes more obvious during reindexing events from Azul. The goal is to speed up the process of 
checking out metadata files and reduce excessive costs related to transferring small files from a 
replica to a checkout bucket.

## User Stories

As the Azul Indexer, I would like to retrieving all metadata files from the dss, so I may rebuild my index.

## Design Details
 During the checkout of metadata, additional tags will be added to the files to denote them as metadata (`metadata=True`).
 
 [AWS S3 object lifecycle rules](https://docs.aws.amazon.com/AmazonS3/latest/dev/intro-lifecycle-rules.html) 
 allows you deleted object based on age, and tags.
 
 [Google Cloud Storage](https://cloud.google.com/storage/docs/lifecycle) does not allow lifecyle rules based on tags.
  
 ### Option 1: Tags
  In the DCP managed checkout bucket, tagged metadata files will not be subjected to the same time to live (TTL) rules as data 
  files and are never removed by TTL rules.
  
  #### Pros
  This leverages the existing lifecyle rules management of AWS.
  
  #### Cons
  This option is only supported in AWS, 
 
 ### Option 2: AWS Lambda
 Create an AWS lambda that will run on CRON job to iterate over the contents of the checkout buckets and determine 
 what should be deleted. A simpler version could refresh the TTL of objects with the metadata tag and allow The bucket
 object lifecyle rules to handle the removal.
 
 #### Pros:
 * Using our own code removed the dependency on AWS and GCP object lifecycle rules. We can then take into account file 
   size, age, type when inspecting the object in the bucket.
 * We can control when the checkout bucket gets cleaned out.
 
 
 #### Cons:
 * Requires iterating over the contents of checkout.
 * Requires writing and maintaining more code.
 
 ### Option 4: Special Metadata Storage
 When metadata files are checkout to a separate directory in the checkout bucket. That directory is not subjected to
 a TTL.
 
 #### Pros:
 * Allows the cloud provider to handle the objects lifecyle

 #### Cons:
 * The checkout logic will need to change to store metadate files in a different location.
 * Downstream users who use checkout will be affected.


 ### Option 5:
 Do not copy metadata files to the checkout bucket. Metadata files are served directly from the replica bucket.

## Unresolved Questions


## Drawbacks and Limitations

* Expectation for the speed for larger files could leak to uses expectation of performance for larger files

## Prior Art 

* all files are treated the same during checkout

## Alternatives
* serve the file directly from replica if indexable