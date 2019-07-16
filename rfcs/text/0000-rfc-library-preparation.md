### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# Representing sequencing library preparations in the HCA DCP metadata standard

## Summary

The current HCA DCP metadata model explicitly represents cell suspensions (single cells or multiple cells suspended in some media) but not the sequencing library preparations derived from them. This is creating challenges for contributors, consumers, and DCP implementation teams when submitting, processing, and interpreting sequencing data. 

Here we propose a solution for explicitly identifying library preparation biomaterials in a sequencing experiment by making them a first-class biomaterial type entity in the metadata schema alongside donor, cell suspension, imaged specimen, etc.

## Author(s)

*Recommended format for Authors:*

 `[Mallory Freeberg](mailto:mfreeberg@ebi.ac.uk)`

 `[Laura Clark](mailto:laura@ebi.ac.uk)`

 `[Name](mailto:username@example.com)`

## Shepherd
***Leave this blank.** This role is assigned by DCP PM to guide the **Author(s)** through the RFC process.*

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

### Definitions

**Logical unit** - a set of metadata and data files that are consumed together to provide a context for understanding, processing, and interpreting data. In the HCA DCP, a logical unit is instantiated by a primary or secondary “bundle”, although the meaning of bundle is not stable or clearly defined at the moment.

**Library preparation** (biomaterial) - A collection of DNA fragments that have been prepared for sequencing from a single biological sample. DNA fragments can be prepared from genomic DNA or transcribed from RNA and are usually ligated to adapter and barcode oligonucleotides.

**Library preparation protocol** - A protocol followed that converts endogenous RNA or DNA molecules into a form (cDNA) that can be sequenced. Often this protocol involves transcribing RNA to cDNA and ligating adapter and barcode oligonucleotides to the cDNA.

### Problem background

The current HCA DCP metadata standard explicitly represents cell suspensions: single or multiple cells suspended in some media after dissociation. In plate-based sequencing technologies, a single cell is isolated from other cells by being placed into a well on a plate, and then library preparation is carried out within each well. In droplet-based sequencing technologies, single cells are isolated using microfluidic partitioning (drops in oil) where barcoding and reverse transcription occur. Subsequently, the droplets are broken and cDNAs are pooled for amplification and library construction. 

From a cell suspension, one (Fig. 1A) or more (Fig. 1B) libraries can be prepared and sequenced. Each library preparation contains cDNA molecules representing a distinct and non-overlapping set of cells. The same library preparation can be sequenced more than once (Fig. 1C), each time producing a unique set of data files. For example, a library preparation can be sequenced on multiple flowcell lanes or a library preparation can be re-sequenced at a later time to generate more data. Regardless of how many times a library preparation was sequenced, **all of the sequence data files derived from one library preparation represent the same set of cells and therefore must be processed together**. Stated another way, **the logical unit of data should be based on a library preparation, all of the sequence files that come from it, and all of the biomaterials, protocols, and processes that generated it**.

---

![Figure 1](/rfcs/images/0000-lib_prep_rfc_fig1.png)

**Figure 1: Possible droplet-based sequencing experimental designs**. A) An experiment where one library preparation was made from a cell suspension and then sequenced once. B) An experiment where two library preparations were made from the same cell suspension and then each library preparation was sequenced once. C) An experiment where two library preparations were made from the same cell suspension and then each library preparation was sequenced twice. Red boxes indicate the set of data files that need to be processed together.

---

[Slides](https://drive.google.com/open?id=1vyw6N7qn24qBFAMoKL3nXLHcqpqYFq3Y) prepared by Nick Barkus (particularly slides 13-15) from the June 2019 DCP F2F describe why all sequence data files derived from one library preparation must be processed together. Briefly, a library preparation starts with a set of UMI barcodes attached to transcripts (1 barcode per transcript) and then everything gets amplified (potentially unevenly) and sequenced (Fig. 2A). Reads are potentially split between different sets of files if the library preparation is sequenced more than once. During processing, unique UMI barcodes are collapsed - identical copies are only counted once - to calculate the original count (Fig. 2B). If files from the same library preparation are processed separately, a collapsed UMI barcode might appear in each set of files, thus inflating the count and leading to the wrong original count (Fig. 2C). 

---

![Figure 2](/rfcs/images/0000-lib_prep_rfc_fig2.png)

**Figure 2: Processing data files separately inflates UMI counts**. A) Two transcripts - each with a unique UMI barcode - for gene A are amplified during library preparation, and the library preparation is sequenced twice to produce two sets of files. B) Files are processed together, and UMI barcodes are collapsed to produce a count of 2 for gene A. C) Files are processed separately, and UMI barcodes are collapsed separately to produce a count of 1 for gene A in the first set of files and 2 for gene A in the second set of files, resulting in an overall count of 3 for gene A (overestimation).

---

### Current implementation

The current HCA metadata standard does not represent a library preparation as an explicit biomaterial entity. At the moment, library preparations are represented by an *optional* metadata field ([`library_prep_id`](https://github.com/HumanCellAtlas/metadata-schema/blob/b8e48a3c7237010104a162c9013180172c2acaf5/json_schema/type/file/sequence_file.json#L81-L87)) on the sequence file entity that indicates which files come from the same library preparation.

#### Data contributor perspective

When data contributors supply metadata for their sequence files, they can enter a value for the `sequence_file.library_prep_id` field to indicate which sequence files were generated from the same library preparation. This value must be unique within the spreadsheet (and therefore project since 1 spreadsheet = 1 project), but it does not need to be globally unique (unique within the entire HCA DCP). 
 
| FILE NAME (Required) | INPUT CELL SUSPENSION ID (Required) | LIBRARY PREPARATION ID (Optional) |
|:-|:-|:-|
| `sequence_file.file_core. file_name` | `cell_suspension.biomaterial_core. biomaterial_id` | `sequence_file.library_prep_id` |
| SRR7159837_1.fastq.gz | cell_suspension_1 | library_preparation_1 |
| SRR7159837_2.fastq.gz | cell_suspension_1 | library_preparation_1 |
| SRR7159838_1.fastq.gz | cell_suspension_1 | library_preparation_1 |
| SRR7159838_2.fastq.gz | cell_suspension_1 | library_preparation_1 |
| SRR7159839_1.fastq.gz | cell_suspension_1 | library_preparation_2 |
| SRR7159839_2.fastq.gz | cell_suspension_1 | library_preparation_2 |
| SRR7159840_1.fastq.gz | cell_suspension_1 | library_preparation_2 |
| SRR7159840_2.fastq.gz | cell_suspension_1 | library_preparation_2 |

**Table 1**: An example set of sequence file metadata based on the experimental design in Fig. 1C. User-friendly field names in row 1. Fully-qualified programmatic field names in row 2.

---

It is important to consider that, currently, if a valid spreadsheet with the metadata in Table 1 was submitted to the Ingestion Service and exported, it would create 1 bundle per row in Table 1. This is not correct because, at a minimum, <filename>_1/<filename>_2 file pairs must be in the same bundle to be processed together (and, e.g., R1/R2/I1 file triplets). A current workaround done by wranglers is to add a column in the Sequence file tab for the `process.process_core.process_id` field and enter the same value for the groups of files that should go in the same bundle. The solution proposed here for the library preparation problem also solves the problem of wranglers (and data contributors) having to fill in the `process_id` field in order to put the correct set of files in the same bundle. 

Assuming a data wrangler or data contributor has filled in the `process_id` field such that each process is assigned a unique identifier, anchoring a logical unit (bundle) on the ultimate process in the graph results in two bundles (Fig. 3A) which means that each bundle does not contain all the data files from the same library preparation. Ideally, logical units are anchored on the library preparation (Fig. 3B) such that all data files from the same library preparation are logically grouped together.

---

<img src="/rfcs/images/0000-lib_prep_rfc_fig3.png" align="left" height="300" />

**Figure 3: Current and ideal grouping of logical units**. A) Logical unit is anchored on ultimate process in the experimental graph, resulting in two bundles which each contain a subset of the data files produced from the same library preparation (not ideal). B) Logical unit is anchored on the library preparation entity, resulting in one logical unit that contains all the data files produced from the same library preparation (ideal).\
<br/>
<br/>
<br/>
<br/>

---

#### Data consumer perspective

Converting the first row from Table 1 (and additional sequence file metadata for that row) to JSON produces:

```
{
    "describedBy": "https://schema.humancellatlas.org/type/file/9.0.0/sequence_file",
    "schema_type": "file",
    "file_core": {
      "file_name": "SRR7159837_1.fastq.gz",
      "format": "fastq.gz"
    },
    "read_index": "read1",
    "lane_index": 7,
    "read_length": 26,
    "library_prep_id": "library_preparation_1"
}
```

As described for Fig. 1C, all the sequence files derived from one library preparation must be processed together as a logical unit. In order to find all sequence files generated from the same library preparation, one must identify all the sequence files that (1) have the same value for `sequence_file.library_prep_id` AND (2) are from the same project (have the same project UUID) because `library_prep_id` is only unique within a project. This query is not ideal because (1) it is schema-aware, (2) it depends on an optional field, and (3) it queries over two variables (`library_prep_id` and project UUID) which makes the query slow.

### Challenges

The current implementation for representing sequence library preparations in the HCA DCP creates challenges for both HCA data contributors and consumers. Some of those challenges include:

1. Data contributors are not required to indicate which data files represent sequencing the same library preparation more than once. Being able to do this is necessary to accurately represent the experimental design.
1. Data consumers find it challenging to determine which data files are from the same library preparation and therefore must be processed together. All data generated from one sequencing library preparation must be processed together. 
1. Data consumers find it challenging to understand the relationship between cell suspensions from plate-based and droplet-based sequencing technologies because of granularity differences (1 cell versus many cells). This inconsistency creates confusion for data consumers when they are browsing or searching for data.

### User Stories

As a data contributor, I would like to be able to indicate which data files were generated from my library preparations so that I can represent my experiment accurately.

As a data consumer, I would like to be able to identify which data files are from the same library preparation so that I can process all sequencing files which belong together at the same time.

As a data consumer, I would like to be confident that data files from the same library preparation were processed together so that I know the expression matrices are accurate.

## Scientific "guardrails" [optional]

*Describe recommended or mandated review from HCA Science governance to ensure that the RFC addresses the needs of the scientific community.*

TBD

## Detailed Design

### Overview

We will resolve the challenges described above by **creating a first-class library preparation biomaterial entity** as part of the HCA metadata standard. This approach will allow the metadata model to explicitly represent which sequencing data files came from the same library preparation even if the data files were generated across multiple sequencing runs (i.e. multiple processes). This approach would also enable data consumers to more easily identify all data files associated with the same library preparation (i.e. from one logical unit) which is required for correct data processing. Unless and until the fundamental way that DNA sequencing experiments are performed changes, all sequencing-based experiments will involve generating library preparations; therefore, the addition of this entity to the metadata model will be used by all current and future sequence-based datasets.

For cellular resolution experiments, the library preparation entity will have a cell suspension entity as input and sequence file entities as outputs. For bulk cell experiments, the library preparation entity will have a specimen (or organoid or cell line) entity as input and sequence file entities as outputs. Figure 3 below shows the current metadata model representing a droplet-based experiment sequencing a single cell suspension to produce two sets of two files (Fig. 4A). From this experimental design, it is not known whether the two sets of files represent one logical unit or two logical units. If the sequence files were produced from the same library preparation (Fig. 4B), then they represent one logical unit and must be processed together. If the sequence files were produced from different library preparation (Fig. 4C), then they represent two logical units and must be processed separately.

---

<img src="/rfcs/images/0000-lib_prep_rfc_fig4.png" align="left" height="500" />

**Figure 4: Determining logical units from droplet-based experimental designs**. A) An experiment modeled using the current metadata model which depicts four sequence files derived from one cell suspension. It is unclear what the logical units are. B) An experiment modeled using the proposed metadata model which depicts four sequence files derived from one library preparation. It is clear that the four files represent one logical unit (red outline). C) An experiment modeled using the proposed metadata model which depicts four sequence files derived from two library preparations. It is clear that the four files represent two logical units. Arrows in graphs represent processes. Note that in B), two processes were used to derive the two sets of files. Both processes belong to this one logical unit.

---

Figure 5 below shows the current metadata model representing a plate-based experiment sequencing three single cell suspensions to produce three sets of two files (Fig. 5A). From this experimental design, it is clear that each set of files represents one logical unit (red, blue, and magenta outlines). The experimental design and logical units are equally as clear under the new proposed metadata model (Fig. 5B). For plate-based sequencing, each cell goes through a library construction protocol to produce a library preparation. These library preparations are then pooled, sequenced, and demultiplexed such that per-cell suspension sequence files are provided.

---

<img src="/rfcs/images/0000-lib_prep_rfc_fig5.png" align="left" height="460" />

**Figure 5: Determining logical units from plate-based experimental designs**. A) An experiment modeled using the current metadata model which depicts six sequence files derived from three single cell suspension. Logical units are indicated (red, blue, and magenta outlines). B) An experiment modeled using the proposed metadata model which depicts six sequence files derived from three single cell library preparations. Logical units are indicated (red, blue, and magenta outlines). Arrows in graphs represent processes. 
 
**NB**: An experimental design where the same library preparation is sequenced more than once is not shown here. This is either (1) not even feasible or (2) highly unlikely to occur given that there isn’t a lot of material to sequence the same cell more than once.

---

This proposal will require modifications to some DCP components (see “Implementation changes for DCP components” section below for more details). Importantly, with a new library preparation entity, the logical unit of data should be anchored on the library preparation, not the ultimate process used to generate a sequence file (colloquially referred to as the “assay process” in the Ingestion Service).

### Metadata schema for library preparation entity

**Standard biomaterial fields**. The new `library_preparation.json` schema will follow the schema structure indicated in the Metadata Schema Style Guide and will include the standard properties (fields) for biomaterial type schemas, including:

```
"describedBy":  {
    "description": "The URL reference to the schema.",
},
"schema_version": {
    "description": "The version number of the schema in major.minor.patch format.",
},
"schema_type": {
    "description": "The type of the metadata schema entity.",
},
"provenance" : {
    "description": "Provenance information provided by the system.",
},
"biomaterial_core" : {
    "description": "Core biomaterial-level information.",
}
```
 
Where the `biomaterial_core.json` schema contains the following properties:

```
"describedBy": {
    "description": "The URL reference to the schema.",
},
"biomaterial_id":{
    "description": "A [project-wide] unique ID for the biomaterial.",
},
"biomaterial_name": {
    "description": "A short, descriptive name for the biomaterial that need not be unique.",
},
"biomaterial_description": {
    "description": "A general description of the biomaterial.",
},
"ncbi_taxon_id" : {
    "description": "A taxonomy ID (taxonID) from NCBI.",
},
"biosamples_accession": {
    "description": "A BioSamples accession.",
},
"insdc_sample_accession": {
    "description": "An International Nucleotide Sequence Database Collaboration (INSDC) sample accession.",
},
"supplementary_files": {
    "description": "A list of filenames of biomaterial-level supplementary files.",
},
"genotype*": {
    "description": "Genotype of the biomaterial.",
},
"HDBR_accession*": {
    "description": "A Human Developmental Biology Resource (HDBR) sample accession.",
}
```
> *Consider removing `genotype` and `HDBR_accession` from core as they don't make sense here and in other biomaterials.

And the `provenance.json` schema contains the following properties (all supplied by Ingestion Service):

```
"describedBy":  {
    "description": "The URL reference to the schema.",
},
"schema_version": {
    "description": "The version number of the schema in major.minor.patch format.",
},
"submission_date": {
    "description": "When project was first submitted to database.",
},
"submitter_id": {
    "description": "ID of individual who first submitted project.",
},
"update_date": {
    "description": "When project was last updated.",
},
"updater_id": {
    "description": "ID of individual who last updated project.",
},
"document_id": {
    "description": "Identifier for document.",
},
"accession": {
    "description": "A unique accession for this entity, provided by the broker.",
}
```

The new `library_preparation.biomaterial_core.biomaterial_id` field will replace the function of the current `sequence_file.library_prep_id` field in the sequence file schema. Data contributors will be required to provide this field to ensure the correct experimental design is represented. It is unclear how the requirement will be enforced: at the moment there is no validation for the presence of a particular JSON type in a logical unit (i.e. bundle).

The required fields for the library preparation entity will be:
- `library_preparation.biomaterial_core.biomaterial_id`
- `library_preparation.biomaterial_core.ncbi_taxon_id`

**New library preparation-specific fields**. The new `library_preparation.json` schema will additionally include a field for capturing INSDC experiment accessions if the project is already archived. This field (`process.insdc_experiment.insdc_experiment_accession`) generally represents a single library preparation in the archives (ENA, SRA, DDJB), and currently lives awkwardly in a process module for historical reasons. Additional library preparation-specific fields could be added in the future.

### Projection of library preparation entity in metadata spreadsheet

Adding a new first-class biomaterial entity has the potential to add a lot of complexity to the metadata model. One area we want to avoid adding complexity is in the metadata spreadsheet, which is already quite complex for data contributors to fill out. Given that there are only two required (user-supplied) metadata fields proposed for library preparation entities, it is possible to keep the metadata spreadsheet relatively unchanged. 

Data contributors will supply a project-wide unique ID for each library preparation in the Sequence file tab using the `library_preparation.biomaterial_core.biomaterial_id` field in place of the current `sequence_file.library_prep_id` field. When the spreadsheet is imported, the Ingestion Service will generate a globally unique UUID for each library preparation entity. Unlike other biomaterials, there would not be a separate spreadsheet tab for library preparation entities for two reasons. 1. Adding a spreadsheet tab adds considerable complexity for data contributors. 2. There are only two fields that need to be supplied by the data contributor for which generating a whole new tab creates high overhead, especially considering one of the fields replaces a field already in the Sequence file tab. Without the ability to automatically infer `library_preparation.biomaterial_core.ncbi_taxon_id`, this field would need to be added to the Sequence file tab and would need to be filled in by contributors. For example:
 
| FILE NAME (Required) | INPUT CELL SUSPENSION ID (Required) | INPUT LIBRARY PREPARATION ID (Required) | NCBI TAXON ID (Required) |
|:-|:-|:-|:-|
| `sequence_file.file_core. file_name` | `cell_suspension.biomaterial_core. biomaterial_id` | `library_preparation.biomaterial_core. biomaterial_id` | `library_preparation.biomaterial_core. ncbi_taxon_id` |
| SRR7159837_1.fastq.gz | cell_suspension_1 | library_preparation_1 | 9606 |
| SRR7159837_2.fastq.gz | cell_suspension_1 | library_preparation_1 | 9606 |
| SRR7159838_1.fastq.gz | cell_suspension_1 | library_preparation_2 | 9606 |
| SRR7159838_2.fastq.gz | cell_suspension_1 | library_preparation_2 | 9606 |
| SRR7159839_1.fastq.gz | cell_suspension_1 | library_preparation_3 | 9606 |
| SRR7159839_2.fastq.gz | cell_suspension_1 | library_preparation_3 | 9606 |
| SRR7159840_1.fastq.gz | cell_suspension_1 | library_preparation_4 | 9606 |
| SRR7159840_2.fastq.gz | cell_suspension_1 | library_preparation_4 | 9606 |

**Table 2**: An example set of sequence file metadata based on Fig. 1B experimental design. User-friendly field names in row 1; fully-qualified programmatic field names in row 2. Text in red indicates spreadsheet differences between Table 1 and Table 2.

---

For the simplest experiments where a library preparation is sequenced only once (droplet- or plate-based), there will be a 1:1 relationship between `cell_suspension.biomaterial_core.biomaterial_id` and `library_preparation.biomaterial_core.biomaterial_id`. It might not be obvious to data contributors why they need to supply two different IDs; the importance will be clearly described in contributor-facing documentation for filling out the metadata spreadsheet.

### Interpretation of library preparation entity by data consumers

Data consumers will be able to find all data that need to be processed together by querying for all logical units (bundles) that are associated with a single library preparation entity as defined by its Ingestion Service-supplied UUID. For example, the query:

```
{
  "es_query": {
    "query": {
      "match": {
        "files.library_preparation_0_json.provenance.document_id": "fd420633-6b3d-4361-ac0d-b57b8d9214d7"
      }
    }
  }
}

```

will return all bundles (primary and secondary) that contain the specified library preparation.

Data consumers will not need to depend on multi-bundle notifications for processing all data files from the same library preparation.. All data files from a single library preparation will be put in the same logical unit if submitted at one time (in one submission envelope). Caveats:
1. Still unclear what the logical unit for an imaging experiments will be. More details under “Unresolved questions”.
1. Still unclear how to create logical units if data files from the same library preparation are submitted at different times (in more than one submission envelope). Is this a complex update to an existing logical unit? Or a new logical unit that re-uses many metadata files?

Data consumers will benefit from the metadata model now aligning with INSDC “experiments” and “runs”. This alignment will also make archiving to ENA/SRA more straightforward.
- Library preparation is equivalent to an INSDC experiment
- Single set of files from a library preparation is equivalent to an INSDC run

| FILE NAME | INPUT LIBRARY PREPARATION ID | INSDC EXPERIMENT ACCESSION | INSDC RUN ACCESSIONS |
|:-|:-|:-|:-|
| `sequence_file.file_core. file_name` | `library_preparation.biomaterial_core. biomaterial_id` | `process.insdc_experiment. insdc_experiment_accession` | `sequence_file. insdc_run_accessions` |
| SRR7159837_1.fastq.gz | library_preparation_1 | SRX3364233 | SRR7159837 |
| SRR7159837_2.fastq.gz | library_preparation_1 | SRX3364233 | SRR7159837 |
| SRR7159838_1.fastq.gz | library_preparation_1 | SRX3364233 | SRR7159838 |
| SRR7159838_2.fastq.gz | library_preparation_1 | SRX3364233 | SRR7159838 |

**Table 3**: An example set of sequence file metadata based on Fig. 1B experimental design showing how INSDC experiment accession aligns with library preparation ID and how INSDC run accession aligns with individual sets of files (i.e. a “run”). User-friendly field names in row 1; fully-qualified programmatic field names in row 2. **NB**: The `process.insdc_experiment.insdc_experiment_accession` field can be moved to the library preparation entity. 

---

### Implementation changes for DCP components

#### Ingestion Service

- Logical units will be anchored on a library preparation entity UUID (or possibly the process entity that is input to the library preparation entity) in the experimental graph (exporter).
- Schema template generator will be modified to (1) put the library preparation entity fields in the Sequence file tab, and (2) exclude all the library preparation entity fields except the required ones. 
- Experimental graph (instantiated by links.json) will represent that (1) a library preparation protocol is followed to produce a library preparation from a cell suspension (or other biomaterial for bulk sequencing) and that (2) a sequencing protocol is followed to produce sequence file(s) from a library preparation. Users of the ingest-api will supply these links during programmatic submission. Otherwise, Ingestion Service (ingest-broker) will create the following links from metadata in the Sequence file tab and some graph assumptions (Fig. 6):
    - Cell suspension :left_right_arrow: Process_1 :left_right_arrow: Library preparation :left_right_arrow: Process_0 :left_right_arrow: Sequence file
    - Process_1 :left_right_arrow: Library preparation protocol
    - Process_0 :left_right_arrow: Sequencing protocol
    - Declare rules about linking
    
---
    
![Figure 6](/rfcs/images/0000-lib_prep_rfc_fig6.png)

**Figure 6: Diagram of how protocol linking will change with new library preparation entity**. A) Current metadata model showing that links to Sequencing protocol and Library preparation protocol entities are generated from the last process (arrow) in the graph. B) New metadata model showing that a link to the Sequencing protocol is generated from the ultimate process (process_0) in the graph, and a link to the Library preparation protocol is generated from the penultimate process (process_1) in the graph.

---

#### Data Storage Service

- No changes will be needed.
- Clients may need to update queries and subscriptions (some described below).

#### Upload Service

- No changes will be needed.

#### Data Processing Pipelines

- Subscription queries will reference a library preparation entity instead of cell suspension entity, where appropriate.
- If multiple-bundles:1-workflow remains a use case, pipelines can query for all “bundles” with a particular library preparation UUID in order to trigger a single workflow.

#### Data Brower

- Is anything coded against cell suspension?
- How will this affect displaying rows in “Sample” view?

#### Matrix Service

- Is anything coded against cell suspension?
- Will there be disruptions to how Matrix Service identifies single cells if we insert a library preparation entity between cell suspension and sequence file (thinking especially for plate-based technologies)?

#### Query Service

- Is anything coded against cell suspension?
- Will queries be made more difficult under this proposal?


### Acceptance Criteria [optional]

- Data processing pipelines workflows can successfully be started and run on all data files from the same library preparation.
- Data consumers can find all data files associated with the same sequencing library preparation.
- Data contributors can complete a metadata spreadsheet without much more difficulty than before (how is this measured?).

### Unresolved Questions

#### To solve during RFC process
- How will the DCP enforce the requirement of having the library preparation entity in every logical unit (i.e. bundle)?
- How will current production sequencing datasets be updated? All of the current datasets will need to be updated as all of them will need to have library preparation entities added. This is a complex update - change in “bundle” structure by adding a library_preparation_0.json entity - so it can not be done with simple AUDR. If the update happens after GA but before complex AUDR, the UUIDs will have to be maintained somehow.

#### To solve at future date

- How might per-plate logical units be defined? The plate ID field is currently in the cell suspension schema. Creating a logical unit of “all data per plate” will not be made easier or harder if this proposal is accepted.
- Could this approach work in an analogous way for imaging experiments? I.e. anchor the logical unit on imaged specimen? It sounds like we don’t yet know yet what the “logical” unit is for image-based experiments. One option to anchor on the imaged specimen, but there are also other options.
- What are the naming conventions for user-supplied IDs and system-supplied, globally unique UUIDs? What are the rules for the names of the fields themselves? What are the rules for what values are valid for these fields? Related to metadata-schema issue [#733](https://github.com/HumanCellAtlas/metadata-schema/issues/733).

### Drawbacks and Limitations [optional]

Implementing the proposed design would require updating the current sequencing datasets in the production DSS to include a library preparation entity. This update would be a bundle structure update and would not be doable using the simple AUDR mechanism, but could possibly be done with complex AUDR. Updating primary bundles might affect downstream products (e.g. secondary bundles, matrices).

This RFC is based on the current bundle structure and DCP design. If bundle structure or DCP design changes in the future (e.g. data processing pipelines can process more than one bundle with one workflow), the impact on library preparation modelling should be considered. Related to this [pre-RFC](https://docs.google.com/document/d/1akc8WrjxllNRkIAKcFD-8H3PsbrZQZjTZfgTI8KWuOI/edit#heading=h.s8wrvv1ovoyj). 

### Prior Art [optional]

Previous [Library preparation entity RFC draft](https://docs.google.com/document/d/15-UkjbAlGDGhOPw1Zu09YdoaGw2Zeq5Y3uDlGsrz6pw/edit)
[Slides](https://drive.google.com/open?id=1vyw6N7qn24qBFAMoKL3nXLHcqpqYFq3Y) from June 2019 DCP F2F meeting outlining library preparation issue from data processing pipelines perspective.
[Slides](https://docs.google.com/presentation/d/1aV7Lrq8SWNxp3Jy98RxUh2SJrLiFPO4Y6AyaH3F1EUI/edit#slide=id.g599322258d_0_8) from June 2019 DCP F2F meeting outlining what is contained in bundles (logical units).

### Alternatives [optional]

The first attempt to clarify where library preparations fit in the experimental design was to add the optional `sequence_file.library_prep_id` field to the metadata standard. Having this field can “get the job done”, but at a high cost to consumers for needing to find it and difficulty for DCP components to use it to define “logical units” for co-processing.

