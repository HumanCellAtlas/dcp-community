### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*
`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# DCP Media Types

Please note that this RFC has been imported its original Google Doc and went through a design review process prior to the RFC Process implementation. The original document is [here](https://docs.google.com/document/d/1TqihrgXjct9aDmTJO52_gE2WlpFysB1OkG9C8exmWTw/edit#heading=h.87ix45a71erf). Please be aware that the contents below may potentially be out-of-date as the last-modified date is October 20th, 2017. 

## Summary

DCP processing involves the transfer and storage of a number of different types of file.  To indicate the type of each file to the DCP, we will use Internet media types [RFC 2046](https://tools.ietf.org/html/rfc2046) during data movement and file storage.

## Author(s)

Original RFC Author:

[Sam Pierson](mailto:spierson@chanzuckerberg.com)

Transcription Authors:

[Arathi Mani](mailto:arathi.mani@chanzuckerberg.com)

[Andrey Kislyuk](mailto:akislyuk@chanzuckerberg.com)

## Shepherd

[Arathi Mani](mailto:arathi.mani@chanzuckerberg.com)

## Motivation

The DCP should not invent new media types; we should append `dcp-type=metadata` or `dcp-type=data` to our Content-Types using the media type "parameter" syntax, e.g. `Content-Type: application/json; dcp-type=”metadata/sample”`. Instead the DCP should strive to use "in-band" media type communication mechanisms where possible.

## Detailed Design

Some DCP subsystems need to know the type of files.  For example differentiating between data files and metadata files is important for the Ingestion and Secondary Analysis services.

There are several different strategies we could use to identify DCP file types:

1) Store a DCP media type in headers/metadata separate from the normal Content-Type
2) Define new media types / subtypes
    - and register them with IANA
    - and use them without registering them
3) Use the personal ("_per._") media type subtype tree.
4) Use the vendor ("_vnd._") media type subtype tree.
5) Use the unregistered ("_x._") media type subtype tree.
6) Use media type parameters.

There are different arguments against each of options 1 through 5, and one overriding argument that they all share, which is:

**There really isn’t such a thing as a DCP specific type of file.  In reality we are using standard types of files while are interpreted in DCP specific ways.  E.g. a `.fastq.gz` data file isn’t a DCP-specific format, it is a gzipped text file.**

This leads us to option 6, using Parameters.  Parameters are additional information about a file type.  They are not highly controlled and there may be an arbitrary number of them.

### Proposal

We add a media type parameter "`dcp-type`" to the end of the media type.  This parameter will indicate the type of the file from the DCP’s perspective.  When used with quotes, the parameter can contain slashes ("/") so we can subtype, just like regular media types).

#### Example Media Types

    application/json; dcp-type=metadata
    application/json; dcp-type="metadata/sample"
    application/json; dcp-type="metadata/assay"

    application/octet-stream; dcp-type=data
    application/gzip; dcp-type=data

#### dcp-type Parameter Values

To communicate possible values for the dcp-type parameter to parties who will use it, let us enumerate all the valid values in this table:

| **dcp-type=** | **Description** |
| ------------- | --------------- |
| data          | A data file     |
| "metadata/assay" | A JSON object describing an assay |
| "metadata/sample" | A JSON object describing a sample|
| "metadata/protocol" | A JSON object describing a protocol |
| "metadata/project" | A JSON object describing a project |
| "metadata/analysis" | A JSON object describing an analysis |

#### Compressed Data

The use of media type suffix `+zip` is acceptable if the data is zipped, e.g. `application/octet-stream+zip`.  However note that there is no suffix for GZip, and therefore `application/gzip` must be used.  HTTP also provides for a `Content-Encoding` header that may indicate that data is compressed, but that is inappropriate in this case as files will remain compressed after transit and Content-Encoding is only used for data in flight and is not supported by storage services such as S3.

### Communicating Media Type to, and between DCP Components

Currently, files enter the DCP in 2 places:

1) Metadata files are deposited by Ingest using the [Upload Service API](https://upload.staging.data.humancellatlas.org/).
2) Data files are uploaded by submitters using the Upload Service commands in the DCP CLI. 

Metadata files may be deposited by Ingest using the Upload Service API `PUT /area/<uuid>/<filename>`.  This an HTTP interface and therefore supports the Content-Type header.  The Content-Type of the files transmitted is stored with the file in the Upload Area using standard file metadata. It is the responsibility of the file uploader to provide a Content-Type with a `dcp-type=` parameter.

Data files are uploaded to the Upload Service using the DCP CLI or (in future when available) the Upload Service Python library, e.g. `hca upload file <filename>`. These utilities will attempt to automatically determine (“sniff”) the media type of the file using code built on top of the _libmagic_ library.  The parameter `dcp-type=data` will be appended to the media type.  The media type may be overridden using command line options or API arguments (TODO).

The DCP Upload Service is implemented using S3, which supports Content-Type metadata for files stored there.  The media types provided by the above routes are copied to the S3 Content-Type metadata field.

#### Ingestion Service

Ingest does not currently directly access data files in the Upload Service.  It uses the Upload Service API `GET /area/<uuid>` to obtain a listing of the files present in the Upload Area.  This listing contains a `content_type` entry for each file.

#### Data Store Service (DSS)

Files are stored in the DSS by copying them from an Upload Service Upload Area.  During this copy process the DSS has access to the metadata, and hence Content-Type, of the file in the Upload Area.

**Should we, in the future, support storage of files from a location that does not support  Content-Type metadata, it will be the responsibility of the file provider to tag the file with a `dcp-content-type` tag containing the media type of the file.**

It is the responsibility of the DSS to store the media type of each of the files it contains and return in when requested.  This may be implemented using standard service metadata, if such metadata supports a Content-Type concept, or “out of band” e.g. using tags, if necessary.

## Prior Art

### Primer on Media Types

As specified in [RFC 7231](https://tools.ietf.org/html/rfc7231#section-3.1.1.1) the structure of a media type used in HTTP `Content-Type` or `Accept` headers is:

    media-type = type "/" subtype *( OWS ";" OWS parameter )
    
        type       = token
        subtype    = token


        OWS        = *( SP / HTAB )
                   ; optional whitespace

    The type/subtype MAY be followed by parameters in the form of name=value pairs.

        parameter  = token "=" ( token / quoted-string )

Additionally:

- [RFC 6838 section 3](https://tools.ietf.org/html/rfc6838#section-3) defines subtype prefixes known as "trees", such as "`per.`" (personal/vanity), "`vnd.`" (vendor) and "`x.`" (unregistered).

- [RFC 6839](https://tools.ietf.org/html/rfc6839) and others specify subtype suffixes such as "`+json`" and "`+zip`", which indicate the base format of the type.  IANA keeps the [Structured Syntax Suffix Registry](https://www.iana.org/assignments/media-type-structured-suffix/media-type-structured-suffix.xhtml).

- [RFC 2045](https://tools.ietf.org/html/rfc2045) points out that comments "(comment)" are allowed in RFC822 structured headers (they use Content-Type as their example), however this doesn’t appear to have carried over to HTTP.

[Wikipedia](https://en.wikipedia.org/wiki/Media_type) sums up the syntax more succinctly:

    top-level-type-name / [ tree. ] subtype-name [ +suffix ] [ ; parameters ]

Top level types, trees, subtypes and suffixes should all be registered with IANA.

Top level types are: application, _audio_, _example_, _font_, _image_, _message_, _model_, _multipart_, _text_, _video_.

Trees are "" (the standard tree), per. (personal), vnd. (vendor) and x. (unregistered).

Suffixes are _+xml_, _+json_, _+json-seq_, _+ber_, _+der_, _+fastinfoset_, _+wbxml_ and _+zip_.

Parameters are optional and, with a few exceptions, are not controlled.
