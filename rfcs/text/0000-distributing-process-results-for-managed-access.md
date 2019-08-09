
### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# Distribution of processed data generated from managed access datasets

## Summary

This RFC proposes a strategy which will enable the DCP to distribute standard processing results (count matrices) for datasets where the raw and some processed data files (e.g those who contain sequence data such as BAMs) cannot be distributed openly, without restriction.

## Author(s)

[Laura Clarke](mailto:laura@ebi.ac.uk)  
[Emily Kirby](mailto:ekirby@p3g.org)   
[Timothy Tickle](mailto:ttickle@broadinstitute.org)   

## Shepherd

*Recommended format for Shepherds:*

 `[Name](mailto:username@example.com)`

## Motivation

Many researchers collecting cellular resolution genomic data are doing so using donor consents which require the sequence level and any clinical phenotypic data to only be released to bona fide researchers for specific research use. This can happen for many reasons including the national law where they are based or the types of individuals they are collecting data from e.g individuals with a disease. This means there is a growing set of data suitable for building the Human Cell Atlas (HCA) which can’t be stored, processed and distributed by the Data Coordination Platform (DCP). This proposal suggests minimally viable solution that allows us to provide count matrices for these data to the community that are interoperable with those generated from our openly releasable datasets, ensuring the community can make use of this expanded range of data when building the HCA. 

**Definitions**

**Managed access data** Raw and processed genomic and phenotypic data from individuals whose consent agreements authorise data release only for specific research use to bona fide researchers.

**Open access data** Raw and processed genomic and phenotypic data from individuals whose consent agreement authorise unrestricted sharing and summary level data generated from both open and managed access raw and processed genomic data.

**European Genome Phenome Archive (EGA)** [EMBL-EBI archive](https://www.ebi.ac.uk/ega/) established to support the distribution of managed access data, providing data contributors a secure storage mechanism for their genomic data and a toolset which enables data contributors to manage access rights to their own data.

**HCA Data Contributor Agreement** an agreement entered into between the Data Contributor and the HCA, prior to contribution and upload of data to the HCA DCP.  

## User Stories

As a **Researcher with a keyboard**, I want to find data that is similar to my own.  I download an expression matrix for that data, then analyze/visualize it locally using my own workflow. I would like this to work irrespective of the access permissions for the raw data and alignments for the data.

As a **Researcher with a pipette**, I want to use the Data Browser and/or tertiary analysis portals to look for interesting patterns of a gene or cell type expression.  For example, I might look for all the tissues where my gene of interest is expressed, or try to find a combination of gene expressions to identify a pathway in my disease of interest.

As a **Researcher with a pipette**, I would like to be able to share all my cellular resolution, human genomic data with the HCA DCP, including data which needs to be distributed via managed access as well as data which can be openly released. I want this to support the aim to build an atlas of all human cells. 

## Scientific "guardrails" 

This plan will be reviewed by members of the [Compliance working group]([https://github.com/HumanCellAtlas/dcp-community/blob/master/charters/Compliance-WG/charter.md](https://github.com/HumanCellAtlas/dcp-community/blob/master/charters/Compliance-WG/charter.md)) who must ensure the proposed design meets their guidelines for responsible handling of managed access genomic data.

## Detailed Design

The proposed data flow for managed access data takes some significant departures from the standard DCP data flow. 

1. The DCP will not redistribute genomic data files which require managed access (fastq and bam).
2. Genomic data files are still submitted via the ingestion service and HCA data and metadata validation but then brokered to the European Genome-Phenome Archive (EGA) rather than being exported to the Data Store. The EGA has long-standing experience in storing and distributing datasets which need managed rather than open access support which the DCP can leverage. 
3. The standard data processing needed by the HCA DCP is done in a secure analysis platform (Terra). Any files produced which can only be distributed via managed access control would need to be resubmitted to the EGA and files and metadata which can be openly released could be submitted to the DSS via ingest.

![Image of managed access data flow from contributor to user](../images/0000-managed-access-data-flow-dcp.png)
[source](https://docs.google.com/drawings/d/11rY0wmXBVhk2b7rqfifpVg-jt4GrxIXIUYPERa-QYig/edit)

Figure 1: Proposed managed access data flow though DCP. 1. Managed access data can be received alongside open access data through the Ingestion Service. 2. Managed access genomic data files are routed to the EGA via the EMBL-EBI Universal Submission Interface, having been validated according to HCA data and metadata rules. 3. Managed access data flows from the EGA to Terra, which provides a secure environment to run HCA data processing pipelines, only DCP team members with permission to access the data will be able to access data and run pipelines on that data in this platform. 4. Outputs from the data processing pipelines are securely submitted back to the Ingestion Service. 5. Openly releasable summary data such as count matrices are written to the Data Store along with all openly releasable metadata with URLs that point users back to the EGA for raw data access. 6. The Browser, Matrix service and any third-party services can still display and process matrices from all data regardless of the location of the raw data.
    
#### Metadata changes

This design change brings a requirement to annotate what data usage is acceptable for a particular project. We propose this is done using the [GA4GH recommended Data Use Ontology]([https://github.com/EBISPOT/DUO](https://github.com/EBISPOT/DUO)) which allows us to annotate a particular project if it is suitable for such things as [not for profit use only]([https://www.ebi.ac.uk/ols/ontologies/duo/terms?iri=http%3A%2F%2Fpurl.obolibrary.org%2Fobo%2FDUO_0000018](https://www.ebi.ac.uk/ols/ontologies/duo/terms?iri=http%3A%2F%2Fpurl.obolibrary.org%2Fobo%2FDUO_0000018)) or [health or medical or biomedical use only]([https://www.ebi.ac.uk/ols/ontologies/duo/terms?iri=http%3A%2F%2Fpurl.obolibrary.org%2Fobo%2FDUO_0000006#](https://www.ebi.ac.uk/ols/ontologies/duo/terms?iri=http%3A%2F%2Fpurl.obolibrary.org%2Fobo%2FDUO_0000006#)). Initially these terms will allow users to restrict their searches to projects whose data use matches their needs. Eventually the presence of these codes will facilitate automated access decisions being made.

#### Data Access Committee (DAC) establishment and operation

The EGA model has each data contributor operating their own data access committee and making their own decisions about authorizing access to their own managed-access data. The DCP will work with the HCA Ethics working group to provide data contributors who don’t already have an established DAC with template data access agreements and best practice for DAC operation.

#### DCP team member permissions to process data from the EGA

DCP team members won’t automatically gain access to data we broker into the EGA. A process will need to be established to gain official permission for named members of the DCP who need to be able to access the data to run the data processing pipelines in Terra to view the data. A possible solution to this challenge for new data will be to add a clause to the HCA data contributor agreement which states that by submitting managed access data to the HCA DCP you give permission for specific pipelines to be run in the Terra by DCP team members.

#### Data import and export to Terra for secure data processing

The pipeline execution service for both the HCA DCP and Terra are compatible and so all pipelines developed for or ran in the DCP will be able to run in the Terra environment. The process of import and export to and from the system will need to be evaluated by DevSecOps in order to be operated in a secure manner and potentially brought into the security perimeter of the secure analysis system. There are opportunities for manual and automatically triggered runs of data depending on the needs of the solution. In either case, the actor managing the execution of the analysis will be authenticated and needs the authority from the data generator to access and process the data.

## Acceptance Criteria 

* A data consumer can download a matrix file which contains results for a mixture of open and managed access datasets

* A data contributor with a dataset that has been collected using a mixture of open and managed donor consent (e.g a combination of dead and alive donors) can submit the whole dataset to via the DCP ingest system and be confident that all data will be responsibly shared under the terms of the consent it was collected.

#### Success Criteria

An implementation of this proposal can be considered successful if 

*   The total volume of data in the DCP increases at a higher rate than previously in the two quarters after delivery of this feature. This should be true as the DCP can now accept a broader range of datasets than it could previously
*   The ratio of managed access to open access data in the system increases over time until it reflects the general ratio of managed to open human cellular resolution genomic data available in the sequence archives. 

## Unresolved Questions

#### Design questions which should be resolved during the RFC process

*   Does ingest need to support any additional security features on top of https to accept submission of managed access data? What does the security perimeter for this data look like and which components will need to be in that security perimeter?
*   What assumptions does the lack of fastq files and BAM files in the DSS break?
*   Can we display metadata and point people to the EGA for access to those files?
*   How do we prevent the raw data associated metadata being written to the DSS from triggering notifications that then break downstream services? Will that be a problem?
*   What review needs to be done to ensure that the data access agreement will allow the redistribution of matrix files and metadata?
*   Who is responsible for applying for and managing access rights for the DCP team members who operate/use the secure platform
    *   Could this be resolved as a clause in the HCA Data Contributor agreement rather than through independendent applications
*   What would it take to leverage the DCP event stream and some components (like Lira) in the security perimeter to make sure the automation of this is as close to current DCP engineering as possible?
*   Could the design here also allow us to accept third party matrix files where we can’t or don’t want to host the raw data from other projects such as the up and coming efforts funded by the NIH such as [HubMAP]([https://commonfund.nih.gov/hubmap](https://commonfund.nih.gov/hubmap)) and [HTAN]([https://www.cancer.gov/research/key-initiatives/moonshot-cancer-initiative/implementation/human-tumor-atlas](https://www.cancer.gov/research/key-initiatives/moonshot-cancer-initiative/implementation/human-tumor-atlas)) 

#### Questions for future iterations of the design

*   Can and should the alignment files that are produced as part of standard pipelines be resubmitted to the EGA? What outputs for each assay would be appropriate to store in the DCP as opposed to a secure environment?
*   Are there other qualitative metrics than the count matrices that should be fed back to the DCP that would otherwise be derived from the BAM files
*   Are there other archives of controlled access data that should be considered as part of this design process (dbGAP)?

## Drawbacks and Limitations [optional]

This proposal will mean that the HCA DCP cannot provide researchers with access to the raw and aligned sequencing data from cellular resolution experiments which are consented for managed access distribution. This will limit researchers ability to ask questions of the data relating to how genetics of the individual or population affect the identity and state of the cells studied. This is functionality that could be enabled in further iterations of the managed access strategy but should not be expected in initial offerings described here.

This proposal represents a compromise solution to allow us to combine the growing quantity of cellular resolution genomic data which has been collected with managed access consents with the openly consented data and provide the community with the count matrices and other summary data which is core to understanding how to build the atlas. 

This work will not allow us to support distribution of managed access data directly from the DCP. A strategic decision is needed if that is a desirable functionality for the DCP to service and an understanding of the technical and logistical requirements of that work versus the DCP implementation team starting to implement a full complement of the functionality required to support redistribution of managed access data. We should also consider if we will need to implement this plan REGARDLESS of if we have our own controlled access implementation. There may always be a need for supporting this data pathway.

## Prior Art 

[Analysis of five years of controlled access and data sharing compliance at the International Cancer Genome Consortium](https://www.nature.com/articles/ng.3499?proof=true&draft=journal) This is a good article about running a DAC

[Data Sharing in the Post-Genomic World: The Experience of the International Cancer Genome Consortium (ICGC) Data Access Compliance Office (DACO)](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1002549) This is a good reference on creating a tiered access data repository

[DUO overview](https://docs.google.com/presentation/d/1B4jsqnZIqwxLjL8Y1q41kYFNN8BsmmiEC6I9_WZZPt0/edit#slide=id.p)) from GA4GH Basel presentation

[Data Use & Researcher Identities](https://www.ga4gh.org/work_stream/data-use-researcher-identities-duri-2/) This are GA4GH led effort to standardize and automate the granting of access based on data use and researcher identity patterns.

[DCP User stories](https://docs.google.com/document/d/1uYdNEnBjl_tMJdGSh8f3VFDd0aw_hhhs2egyvkIfOeg/edit) (note managed access user stories represents edits based on those contained within here)

[EGA DAC Account Management 2016](https://drive.google.com/open?id=1_XjKzWQVjF9_tT97LYyKqsz58vnVv6Ta)

[EGA DAC Support 2019](https://drive.google.com/open?id=10uEcRilASh0YvfxqiLLm5VMNgTpKBbP0)

## Alternatives 

[Managed access data WIP](https://github.com/HumanCellAtlas/dcp-community/pull/98)

 
