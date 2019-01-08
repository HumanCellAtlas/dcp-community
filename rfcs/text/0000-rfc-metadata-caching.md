### DCP PR:
# Metadata Caching
## Summary
Speedup the checkout process for metadata file by caching them in the checkout bucket.
## Author

[Trent Smith](mailto:tsmith12@ucsc.edu)

[Amar Jandu](mailto:ajandu@ucsc.edu)

## Motivation

Checking out small metadata files is slow, cost inefficient, and performed frequently . This becomes more obvious during reindexing events from Azul. The goal is to speed up the process of checking out metadata files and reduce excessive costs related to transferring small files from a replica to a checkout bucket.

## User Stories

As the Azul Indexer, I would like to retrieving all metadata files from the dss, so I may rebuild my index.

## Design Details

 * Solution: Configure the ttl for small files so they never leave checkout.
    * Deletion will need to remove these flagged files from checkout.
    * Either flag files by size or file type.
    * Enable request timeout on boto3 to avoid timing out lambdas and s3

## Unresolved Questions

* DataStore: Should the DSS manage it's own index?
* Cache hit path must be fast enough

## Drawbacks and Limitations

* Expectation for the speed for larger files could leak to uses expectation of performance for larger files

## Prior Art 

* all files are treated the same during checkout

## Alternatives
* serve the file directly from replica if indexable