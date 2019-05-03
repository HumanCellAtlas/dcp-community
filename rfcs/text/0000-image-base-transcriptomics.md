### DCP PR:
Leave this blank until the RFC is approved then the Author(s) must create a link between the assigned RFC number and this pull request in the format:

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# **Adding DCP support for image based-transcriptomics**

## **Summary**

The Human Cell Atlas (HCA) Governance Group (GG) has requested that the
Data Coordination Platform (DCP) prioritize the incorporation of an
image-based transcriptomics assay into the DCP. This RFC is intended to 
serve as an informational RFC, as suggested by 
https://github.com/HumanCellAtlas/dcp-community/issues/30, and describes the
minimum set of deliverables that would need to be prioritized and completed for
the DCP to be able to (1) store and (2) process image-based single-cell
transcriptomics experiments. The following assumptions are made:

1.  Support for an image-based transcriptomics assay in the early
    iteration phase should be added to the DCP

2.  The current state of the starfish ecosystem is sufficient to
    process image-based transcriptomics experiments.

3.  This instantiation will require limited scale (low hundreds of
    datasets over several years)

## **Author(s)**

[Ambrose Carr](mailto:acarr@chanzuckerberg.com)

[Josh Moore](mailto:j.a.moore@dundee.ac.uk)

## **Shepherd**
[Brian Raymor](mailto:braymor@chanzuckerberg.com)

## **Motivation**

Cellular imaging assays typically either (1) categorize cells based on
the presence/absence of a particular set of markers, or (2) count
structures within cells. These assays exist at variable levels of
magnifications, including the counting of
[<span class="underline">individual
atoms</span>](https://www.sciencemag.org/news/2018/07/tour-de-force-researchers-image-entire-fly-brain-minute-detail)
(Electron Microscopy (EM))
[<span class="underline">synapses</span>](https://www.hhmi.org/news/how-to-rapidly-image-entire-brains-at-nanoscale-resolution)
(lattice light sheet), or [<span class="underline">counting molecules
within cells</span>](http://linnarssonlab.org/osmFISH/) (image-based
single-cell transcriptomics and image-based proteomics).

Image-based transcriptomics assays are a good candidate for DCP imaging
support. They are relatively simple imaging assays that transform image
features into a series of detected transcripts, which are aggregated by
cell. Because these assays require thin-sections of tissue, they produce
datasets that are relatively small by imaging standards (1-10s TBs). In
addition, image-based transcriptomics pipelines generate the same cell x
gene expression matrix outputted by scRNA-seq assays with additional
cellular annotations of the location of the cells. Because these data
structures are already present in the DCP, there are fewer components
that need to be generalized than for other imaging assays. Finally,
engineers from
[*<span class="underline">starfish</span>*](https://spacetx-starfish.readthedocs.io/en/latest/)
and
[<span class="underline">OME</span>](https://www.openmicroscopy.org/)
are working together to craft data formats, validation tooling, and
processing pipelines that comply with DCP requirements where possible.
These characteristics make this assay class a good choice for
prototyping image support in the DCP as it presents the smallest scale
challenge, the majority of the major deliverables can be provided by
DCP-extrinsic developers, and it requires the fewest modifications of
DCP components. Implementation of support for these assays is likely to
generalize to additional areas of interest to the HCA community such as
multiplexed antibody imaging and thick-section volumetric imaging for
both RNA and protein.

The remainder of this document provides additional details on the
characteristics of image-based transcriptomics assays, why they are the
right assay class to prototype imaging support in the DCP, their current
scientific state, the standardization of imaging file formats, and a
longer-term view of support for single-cell imaging assays. In brief:

  - Support for image-based transcriptomics can be added without need
    to construct any additional DCP services, and should not require
    significant generalization of existing ones.

  - Image-based transcriptomics is the right class of assay to
    initiate imaging support in the DCP because of their similarity to
    existing DCP sequencing assays and the abundance of labs using
    single-molecule FISH assays to clarify spatial distributions of
    cell types identified in RNA-seq experiments.

  - If desired, *all* key validation, specification, and analysis
    deliverables can be provided by *starfish* and OME engineers,
    minimizing load on DCP engineering teams.

  - A draft metadata schema has already been built by EBI via
    collaboration with the SpaceTx consortium, who have delivered
    image-based transcriptomics datasets which the ingestion service
    is beginning to test.

  - Based on [<span class="underline">delivery timelines for
    prerequisite
    work</span>](https://docs.google.com/document/d/1RQcj450v0WHGUvSm7YwwFQ7sRFoPAOCjRbo63UzyQd0/edit?ts=5c40d3f1#heading=h.4qbf4q96id16),
    implementation of support for image-based transcriptomics could
    begin as early as Q2-Q3 2019.

  - While image-based transcriptomics data can be ingested and stored
    without a visualization capability, significant user demand can be
    expected similar to that for count matrices.

### **User Stories**

  - As a submitter, I want to upload directories of files in SpaceTx Format representing
    an imaging experiment for storage in the DCP.

  - As a user with a keyboard, I want to identify images with a given
    gene for download so that I can visualize the gene's spatial patterning.

  - As a user with a keyboard or pipette, I want to see spots overlayed on an
    image for QC to convince myself that the data have been properly processed.

  - As a user with a keyboard, I want to see cells overlayed on an
    image for QC to confirm that cells have been properly segmented.

  - As a user with a keyboard, I want to generate a gene x cell matrix
    for biological analysis of my data and combination with sequencing
    assays so that I can use these data to answer biological questions.

## **Scientific "guardrails"**

Image-based transcriptomics assays are still in active development, and
benchmarking and characterization of these data types is in process. As
such starfish and the Data Processing Pipelines team expect to utilize community feedback to iterate on scientific
quality of the pipelines, and will seek governance approval for the
inclusion of data from these pipelines in release only when the
technologies are more mature.

Before large-scale ingress of these complex datasets is begun, the
proposed integration should be reviewed against the HCA Science
governance’s understanding of both the communities likely use of imaging
data as well as the long-term costs of storage and transfer. Based on
the intended investment for imaging, additional effort at this initial
stage of the architecture may be called for to increase value or
decrease cost.

## **Detailed Design**

### Data Specification

A data specification consists of format specifications for any files
(inputs and outputs) used by imaging assays, and HCA metadata that sits
on top of these data and describes information about the samples and how
the data were acquired. Formats and specifications listed below are
being developed by *starfish*, OME, and HCA Metadata teams. Each have
working prototypes supported by active development teams.

#### Input Data Formats

##### SpaceTx Format

To prototype the use of imaging data in the DCP, the *starfish* and OME
teams propose that the DCP use [<span class="underline">SpaceTx
Format</span>](https://spacetx-starfish.readthedocs.io/en/latest/help_and_reference/spacetx-format/index.html),
a format that groups and indexes a potentially very large number of two
dimensional images, forming 5-dimensional tensors over (time, fluorescence channel,
z, y, x) -- dimensions which are fairly consistent across imaging domains
and will likely generalize to other use cases.

A SpaceTx-formatted fileset represents the output from one continuous acquisition of a specimen
and as a unit is called an "experiment". An experiment typically consists of one primary image
as well as auxillary images, such as dots or nuclei. These images are acquired as multiple
possibly overlapping fields-of-view, the 5-dimensional tensors described above.

SpaceTx created this format because they were unable to find an existing format
that satisfied our cloud compute and data visualization use cases. *starfish*
and OME have since become aware of contemporaneous development of
projects that aim to satisfy similar use cases and are working towards
unification, where possible across scientific disciplines and languages.

To avoid writing a large amount of microscope-specific code at the
outset, *starfish* requests that users carry out basic pre-processing of
their data to fix registration or chromatic errors. More details on the
specific requests made of users can be found
[<span class="underline">here</span>](https://spacetx-starfish.readthedocs.io/en/latest/usage/index.html).
The exact specification of the image data format will depend upon the
delivery date that the DCP requests for this prototype.

##### Pipeline Recipes

The parameterization of an imaging pipeline depends on the assay in
question, the microscope used to capture the data, the density of spots
in the tissue, and the tissue being analyzed. For example, if the same
assay is run a second time at twice the magnification, spots would be
expected to have twice the radius, which would need to be communicated
to the spot-finding algorithm.

Human analysts are very good at selecting these parameters, but
processes to fit parameters to all datasets across assays, tissues, and
microscopes have not yet been developed. To solve this problem, starfish
exposes the ability for users to pilot starfish on small subsets of
their data in a local environment. These analyses generate parameter
sets ("pipeline recipes") which are stored in json and validated by
*starfish*. These parameters are then used to run *starfish* across the
complete dataset, in a cloud-based environment.

#### Output Data Formats

Image-based transcriptomics experiments emit three output formats that
contain identified features, all of which can be emitted in either
netcdf or zarr file formats, the latter being the format consumed by the
matrix service. In addition, it has the opportunity to emit image files
that have been processed to align images, remove autofluorescence and
enhance spots. All output formats (including SpaceTx image format)
contain full provenance records of the analysis environment, versions of
starfish and key dependencies, algorithms that were run, and their
parameters.

##### [IntensityTable]

Image-based transcriptomics experiments image the same tissue using
multiple color channels, and after subjecting the tissue to different
reagents. The physical locations of fluorescent molecules manifest as
spots in the images, and these spots are detected and aggregated across
fluorescence channels and time to build codes. In practice, the process
of detecting and decoding spots contains significant possibilities for
error, and the IntensityTable supports analysis and QC for this
process.

##### [SegmentationMask]

In order to assign the spots identified above to individual cells, the
locations of cells within the images must be identified. Compressed
segmentation masks are a compact way to represent these data, which
store, for each cell, the portion of the image that is attributed to
that cell.

##### [ExpressionMatrix]

*starfish* combines spot calls from the IntensityTable with cell areas
in the SegmentationMask to produce expression matrices. These are saved
in zarr format and look identical to the matrices emitted by the HCA data 
processing pipelines, with the exception that each cell is
additionally annotated with metadata that specify the x, y, and z
location of the data. Feature names will be consistent with DCP standards.

##### Processed Images

Image-based transcriptomics experiments suffer from several sources of
error and variation which are removed via image processing. Microscope
stages or tissue slices may shift as reagents are added and removed
across imaging rounds, or which spectral filters are swapped between
channels. In addition, reagents can non-specifically bind to cellular
material, causing autofluorescence. Prior to spot-calling, images can be
aligned, background removed, and spots computationally enhanced.
*starfish* can output these processed images, annotated with information
sufficient to re-do the processing, so that the images input to spot
calling can be evaluated by users. If emitted, processed images will be
generated in SpaceTx Format.

#### HCA Sample Metadata

The HCA metadata team has built a metadata specification draft for
imaging data that references data in SpaceTx-Format. The issue that
formed the draft can be found
[<span class="underline">here</span>](https://github.com/HumanCellAtlas/metadata-schema/issues/368).

### Data and Metadata validation

#### Input Data validation

SpaceTx Format consists of a set of json files which describe a
further set of two-dimensional image files. As a result, it is
possible to rapidly validate that a fileset is complete and internally
consistent. In addition, SpaceTx Format stores per-chunk checksum
information, enabling the data to be checked for integrity. It is
possible to carry out additional, more data-specific checks, such as
that images are not blank, contain spots, or contain cells, however
these checks take increasing amounts of time.

OME and SpaceTx can provide a docker container that validates image
datasets with the highest level of stringency possible in a DCP-provided
"maximum validation time".

#### Output Data validation

Each of the output formats produced by SpaceTx can be stored in Zarr and
read (on-demand) by *starfish* and xarray. SpaceTx provides a docker
container that loads and validates each output file using the xarray
library. Validation can be tuned to occur within a DCP-provided target
validation time.

### Data Processing Workflows

To process data in the DCP, the *starfish* team can deliver WDL-wrapped
data processing workflows for any of the existing image-based
transcriptomics pipelines. These workflows are based on the *starfish*
python library, which in turn leverages standard open-source python
projects. *starfish* provides a docker container that exposes both a
python API and a CLI, and supports composable, modular, pipelines. These
pipelines take as inputs images in SpaceTx Format and a pipeline recipe.
Each assay emits output in the formats specified above. More detail on
the state of image-based transcriptomics analyses can be found
[<span class="underline">here</span>](https://docs.google.com/document/d/18Ve9C_cEn5lI2paivfgTAyjsM62qMyUF_s4mj82l1L0/edit#).

### Data Processing Infrastructure

All *starfish* pipelines share the same inputs and outputs. It should
therefore be possible to deliver a single adapter WDL that satisfies any
SpaceTx assay, and a family of subscriptions with minimal inter-class
changes that specify the particular assay in question.

### Delivery of Output Data to Users

#### Matrices

As mentioned above, image-based transcriptomics output data
include a gene expression matrix and related metadata that are
compatible with matrix service requirements and should be no
more difficult to convert than the output of a sequencing assay.

However, other imaging experiments will likely require extending how
features (genes) are named to support a broader set of features
(eg. proteins and cellular components). Currently, the DCP standardizes
around GENCODE gene IDs and HGNC gene symbols. Image based proteomics
will require protein identifiers such as UNIPROT, and other use cases
that measure sub-cellular components will likely require more general
cellular component information (e.g. "endoplasmic reticulum").

### **Acceptance Criteria**

Except where listed ("responsible team"), the starfish and OME teams
will do the majority of project management and execution for the
following deliverables. They will coordinate and collaborate with DCP
partners ("contact person") to validate design correctness and minimize
code changes and their impact.

| Component/Deliverable                                 	| Responsible Team/Service 	| Contact Person	|
|-------------------------------------------------------	|--------------------------	|---------------	|
| HCA Metadata Draft                                    	| EBI                      	| Z. Perova     	|
| Test Datasets Available                               	| starfish                 	|  A. Carr       	|
| Submission Datasets Available                         	| starfish                 	|  A. Carr       	|
| Analysis & Adapter Pipelines                          	| Broad                    	|  A. Carr       	|
| Lira subscription                                     	| Broad                    	|  S. Ehsan      	|
| Data Processing Pipelines Tests                        	| Broad                    	|  G. Grant      	|
| Specification for input format                        	| starfish                 	|  J. Moore      	|
| Specification for output format                       	| starfish                 	|  A. Carr       	|
| Input validation tool                                 	| starfish, OME            	|  J. Moore      	|
| Output validation tool                                	| starfish, OME            	|  A. Carr       	|
| Ingest support for the nested arrays of channels      	| EBI                      	|  Z. Perova        |
| [Upload and download of subdirectories][b]             	| ?                        	|  H. Schmidt       |
| DSS spatial data bundle (or bundless) specification   	| UCSC                     	|  B. Hannafious 	|
| DSS scale testing                                     	| UCSC / DCP               	|  B. Hannafious 	|
| Data Browser facets for spatial data                  	| UCSC                     	|  H. Schmidt    	|
| Matrix service integration                            	| starfish                 	|  M. Kinsella   	|
| Upload Bundle for 1M+ files                           	| ?                        	|  P. Shah       	|
| Image file download latency testing                   	| ?                        	|  B. Hannafious  	|
| Image wrangling process                                   | EBI/UCSC                  |  Z. Perova        |

### **Drawbacks and Limitations**

Imaging formats and pipelines have not yet reached a level of
standardization across the community. As such, flexibility around
updating and upgrading formats and pipelines will be necessary.
Specifically:

1.  Currently, little to no imaging data is _acquired_ in the SpaceTx
    Format and the conversion of datasets into SpaceTx Format is outside
    the scope of this RFC. Submitters with the help of wranglers must
    convert their data before upload. The *starfish* library, however,
    provides a scripting mechanism for converting simple formats into
    SpaceTx Format. An example of this is provided in the [ISS Vignette].
    For more complicated and opaque data, use of the [Bio-Formats] library
    will work for any of the formats on the [Supported Formats] list.

2.  The development of a widel-supported chunked image file format\[1\] that supports
    cloud workflows for data at the scale that will become common over
    the next 10 years is in an early stage. Working prototypes from
    genomics, imaging, astronomy, and geosciences exist and a process
    is beginning to unify formats. However, this process will evolve
    over time, and it will be necessary to upgrade the data,
    potentially multiple times, as backwards-incompatible changes are
    made. To mitigate this risk, the OME and *starfish* teams are
    willing to be responsible for producing containerized scripts to
    upgrade data when this is necessary, and to adhere to schedules
    that limit the frequencies of these updates.

3.  The analysis of image-based transcriptomics assays are not yet
    fully automated, and therefore during the pilot phase, support of
    imaging data will rely on user-supplied parameterizations of
    *starfish*-based workflows. This will mean that initial datasets
    may contain batch effects and be difficult to (absolutely) compare
    to one another. However, while parameter selection decisions could
    bias the absolute abundance of detected spots, the relative
    abundances of spots within datasets should be consistent, and
    presentations at recent single-cell genomics, keystone, and HCA
    science meetings have demonstrated the utility of integrating
    image data with single-cell RNA-seq data, and the fields appetite
    to engage in these experiments. The *starfish* team sees a path
    towards automating parameter selection for these assays, which can
    be expedited by accumulating data in the DCP and building
    community consensus around image data processing. This is a strong
    argument for adding support now, and implies that there will be a
    point in the future when these pipelines are updated, at which
    point it will be necessary to reanalyze the data. If the
    standardized pipelines are built with *starfish*, the *starfish*
    team will commit to ensuring that those pipelines are DCP
    compatible.

### **Prior Art and Alternatives**

A number of imaging formats and platforms exist that could provide parts
of an imaging solution for the HCA. A few have been listed below. Rather
than being direct alternatives though, they may ultimately be of
interest to consumers as extensions, portals, or conversion options.
Widening the scope of the HCA to support ingestion of additional formats
and to integrate other analysis and visualization platforms may be a
natural reaction to community demand.

The initial benefit of the SpaceTx format, library, and consortium,
however, is the on-going generation of initial datasets by individuals
already involved in the HCA, making it an ideal and low-cost prototype
for how imaging can work in the DCP. Additional options can be
prioritized based on the scientific "guardrails".

#### File Formats

  - [<span class="underline">OME-TIFF</span>](https://www.openmicroscopy.org/site/support/ome-model/ome-tiff/specification.html)

  - [<span class="underline">N5</span>](https://github.com/saalfeldlab/n5)

  - [<span class="underline">HDF5</span>](https://www.hdfgroup.org/solutions/hdf5/)

  - [<span class="underline">Zarr</span>](https://zarr.readthedocs.io)

#### Analysis Platforms/Pipelines

  - [<span class="underline">ImageJ</span>](https://imagej.net)

  - [<span class="underline">ilastik</span>](https://ilastik.org)

  - [<span class="underline">QuPath</span>](https://qupath.github.io/)

  - [<span class="underline">Orbit</span>](https://www.orbit.bio/)

  - [<span class="underline">CellProfiler</span>](http://cellprofiler.org)

#### Visualization Platforms

  - [<span class="underline">IDR</span>](https://idr.openmicroscopy.org/about/)

  - [<span class="underline">OMERO</span>](https://www.openmicroscopy.org/omero)

  - [<span class="underline">Cytomine</span>](http://www.cytomine.be)

  - [<span class="underline">PathViewer
    (Commercial)</span>](https://glencoesoftware.com/products/pathviewer/)

## **External contributors**

### *starfish* team

*starfish* is a software library that enables data scientists to create
analysis pipelines for any image-based transcriptomics assay and defines
a file format that supports scalable cloud-based execution of these
pipelines. The *starfish* team at CZI is currently focused on supporting
the SpaceTx consortium, which consists of the core developers of
image-based transcriptomics assays. The SpaceTx consortium aims to
benchmark image-based transcriptomics assays by comparing each assay run
on the same tissue, from a shared probe set. It aims to answer the
question "which assay should I choose, given tissue characteristics, or
the nature of my biological hypothesis?".

### Open Microscopy Environment (OME)

OME maintains open-source software and format standards for the storage
and manipulation of microscopy data. The project’s foundation is an open
metadata specification, the OME Data Model. Using this specification,
OME builds and releases: OME-TIFF, an open file format that combines the
popular, broadly supported TIFF format for storing binary pixel data,
with an OME-based XML metadata header; Bio-Formats, a software plug-in
that reads \>150 proprietary file formats and converts them to the
OME-based model; and OMERO, an enterprise image data management platform
that enables access, processing, sharing, analysis and where appropriate
publication of scientific image data and metadata. Motivated by arising
requirements including those of the HCA, OME is investigating a
cloud-compatible chunked file format for long-term archiving of imaging
data.

1.  A [<span class="underline">Long-Term
    Vision</span>](https://docs.google.com/document/d/1wVYfBRL_omsnlWqUBc72kOm-GLdxPa97vBuV-OxyAcY/edit)
    document contains a fuller discussion on the needs of this format.

[a]: https://github.com/HumanCellAtlas/metadata-schema/issues/623
[b]: https://github.com/HumanCellAtlas/data-store/issues/1885

[Bio-Formats]: https://www.openmicroscopy.org/bio-formats
[ExpressionMatrix]: https://spacetx-starfish.readthedocs.io/en/latest/sptx-format/output_formats/ExpressionMatrix/
[IntensityTable]: https://spacetx-starfish.readthedocs.io/en/latest/sptx-format/output_formats/IntensityTable/
[ISS Vignette]: https://spacetx-starfish.readthedocs.io/en/stable/usage/iss/iss_vignette.html
[SegmentationMask]: https://spacetx-starfish.readthedocs.io/en/latest/sptx-format/output_formats/SegmentationMask/
[Supported Formats]: https://j.mp/bio-formats-list
