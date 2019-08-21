# Human Cell Atlas Data Coordination Platform - Our Strategy

## Measuring our Impact

This document provides background context for the overall aims and the specific aims, including:
* ***value assumptions*** that define why we believe our aims are the right aims, and
* ***indicators for success*** that define how we assess our progress against these aims

### Aim 1

**Create a data resource that maximizes the value and use of Human Cell Atlas data across the scientific community.**

#### Value Assumptions

Scientists expect the DCP to be a comprehensive resource, containing all data that has been generated in their area of research interest. If they aren’t confident that all the data they need is present, if metadata they need to interpret the data are missing, or if the data quality is too low, they will download HCA data from other resources.

Scientists will use raw data from the DCP and “output data” from DCP processing pipelines as long as it is well described, and includes rich, detailed information about how the data was generated (including sample descriptions and protocol information).

HCA metadata and data will generate significant scientific impact for a variety of researchers (namely the spectrum from computational biologists to wet-lab biologists) if represented in an accessible manner according to their technical needs and capabilities.

Data generators will deposit data in the DCP because the DCP provides valuable incentives, such as accession numbers that can be cited in publications.

#### Indicators for Success

| Specific aim | Item | Indicator |
| -----------: | ---- | :-------- |
| 1 | a. | All data used in the Human Cell Atlas or tissue-specific cellular atlases (e.g. brain atlas) is present in the DCP |
|   | b. | Data deposited match known volumes and assay types where these are available, for example when listed in project funding awards (e.g. seed network RFAs, MRC development cell atlas awards, Human Cell Atlas Horizon 2020 awards) |
|   | c. | The impact of DCP data is demonstrated in portals that utilise it |
|   | d. | The impact of DCP data is demonstrated in publications that cite it |
| 2 | a. | A high proportion of protocols used in the DCP are documented in detail in protocols.io |
|   | b. | Projects are pre-registered with the DCP when they begin, as determined by project funding awards where available. Data arrives later |
| 3 | a. | DCP accession numbers, DOIs and other means of citation of DCP data are more prevalent in publications than the corresponding submission at an additional resource |


### Aim 2

**Provide researchers access to data that will enable the Human Cell Atlas to answer fundamental questions in all aspects of biology.**

#### Value Assumptions

The two main types of scientists targeted by the DCP (computational biologists and “wet-lab” researchers in smaller groups) will search for data of interest using a limited, focused number of relatively simple characteristics before running their chosen analysis tools on that set of data.

Scientists will utilise data from the DCP as long as it is easy to access. If metadata they need are missing, or if it is difficult to access and work with DCP data, they will try to source the data from elsewhere.

Scientists will access analysed data that has been generated in their area of research interest in order to exploit it using trusted analysis tools.

Scientists will explore the data in the DCP via a user interface in order to find data relevant to their research interest, before using programmatic techniques and tools to perform more in-depth analyses. They expect the DCP to provide a consistent experience across user interfaces and APIs.

In order for it to be easy to use or develop analysis tools, data in the DCP must be easy to use and the experience of discovering and using this data must be the same whether data is accessed via user interfaces or APIs.

#### Indicators for Success

| Specific aim | Item | Indicator |
| -----------: | ---- | :-------- |
| 1 | a. | The same scientists return to the DCP to access data as new datasets are published (i.e. the DCP is retaining existing scientists because the data is useable) |
|   | b. | Analysed data is accessed more commonly than raw data |
|   | c. | User requests for additions or modifications to the metadata available are relatively rare |
| 2 | a. | User feedback and UX research indicates a high degree of satisfaction with DCP user interfaces and APIs |
| 3 | a. | Relative usage of the UI for data searches, and the API for direct data access, show consistent patterns over time, rather than changing significantly depending on which access mode is easier |
|   | b. | Search analytics show datasets are discovered using a small number of key searches common to a wide number of scientists |
|   | c. | All data used in the Human Cell Atlas or tissue-specific cellular atlases (e.g. brain atlas) is present in the DCP |


### Aim 3

**Perform regular data releases of high value to single cell researchers.**

#### Value Assumptions

The two main types of scientists, when consuming others’ data from the platform, value consistency of data and metadata, in terms of standards, versions and quality control over the speed of access.

Scientists will analyse comprehensive data using available analysis tools as long as this is easy to do, and to be easy, the DCP must provide access to data that is “pre-harmonised” (i.e. all uses the same standards, processing pipelines and QC criteria).

Scientists will analyse all DCP data, including “historical datasets” (that don’t conform to the latest standards and aren’t possible to automatically migrate), but expect the DCP to curate these historical datasets for them and aren’t prepared to do this themselves.

Scientists will infrequently update their analysis methods, so data from prior releases will continue to be used for a long time after it was initially made available.

#### Indicators for Success

| Specific aim | Item | Indicator |
| -----------: | ---- | :-------- |
| 1 | a. | Access statistics for DCP data releases are higher than access statistics for the “live” DCP view |
| 2 | a. | Access statistics for data specific subsets are higher than access statistics for the “live” DCP view |
| 3 | a. | Access statistics for migrated datasets in the DCP release are higher than access statistics for unmigrated data in the “live” DCP view |
|   | b. | Frequent requests to curate historical datasets that do not conform to the latest standards, and are therefore missing from the latest releases, are received by DCP helpdesk |
|   | c. | A number of historical datasets perceived in the community as having high value, but that are non-conformant to latest standards, are disproportionately heavily accessed from previous data releases |
| 4 | a. | Access statistics for reprocessed DCP data are higher than access for the original data |
| 5 | a. | Access statistics for previous data releases is initially higher than access statistics for the latest release, but access diminishes over time |


### Aim 4

**Build alignment amongst the Human Cell Atlas community around a core set of computational methods and analyses.**

#### Value Assumptions

The Human Cell Atlas community are experimenting with a variety of methods of data processing and analysis, and it is currently unclear which methods are “best”.

Scientists expect the DCP to provide information about the most commonly used methods for processing and analysis of cellular resolution data.

Until convergence emerges, scientists will process data in a variety of ways, and are likely to publish on the accuracy of methods, including those in the DCP, significantly changing the “state of the art”.

Scientists expect that all data generated by the most commonly utilised technologies can be analysed by DCP processing pipelines

Analysing integrated data across experiments is most easily accomplished when technical variation is minimized or controlled for. When performing such integrative analyses, scientists will expect DCP data to be produced from “harmonized” data processing pipelines.

Scientists expect that the latest and greatest analysis methods are quickly available within the DCP, and if the methods they need are not available, will work with the DCP analysis community to contribute them.

Scientists expect to benchmark datasets by processing and analysing them with computational methods within or alongside the DCP and comparing them to results from other methods.

#### Indicators for Success

| Specific aim | Item | Indicator |
| -----------: | ---- | :-------- |
| 1 | a. | DCP technology reports are highly accessed by a variety of users |
|   | b. | DCP technology reports receive positive feedback from the wider single cell community |
| 2 | a. | The number of datasets that are generated and deposited in the DCP, but that cannot be analysed by DCP pipelines, is very low |
| 3 | a. | Scientists access data processed by core DCP pipelines more frequently than data processed by community-contributed pipelines |
| 4 | a. | Processed data from community-contributed pipelines is heavily accessed |


### Aim 5

**Create standards for the community to use to describe single cell experimental designs, including assay types, data and metadata.**

#### Value Assumptions

Scientists would like the DCP to provide guidance on how to describe single cell experiment designs and would be prepared to invest time in accurately providing this information if it these standards are accepted by the community.

Scientists want the DCP to drive convergence around a small number of methods, and expect the DCP to support this by facilitating direct comparison of  the quality of results acquired from different experiment designs.

Scientists care about a core set of relatively simple fields to characterise cell types, but it is currently unclear what these are across the Human Cell Atlas (e.g. all tissues or all sampling techniques).

For this set of fields, scientists would like data to be very well described, with detailed metadata and annotations to a standard set of ontologies.

Scientists would like DCP data to be consistently described and interoperable with all cellular resolution data (both inside and outside the platform).

#### Indicators for Success

| Specific aim | Item | Indicator |
| -----------: | ---- | :-------- |
| 1 | a. | Most commonly used experimental designs in the DCP correlate to higher QC results |
| 2 | a. | Summary tables of DCP data volume, aggregated by key metadata, e.g. which assays are applied to which tissues, are highly accessed. |
| 3 | a. | Captured biological metadata (biological materials and experiment designs) are frequently used to characterise cell types |
|   | b. | Captured computation metadata (analysis methods, QC and file formats) are used when determining which data to use in the characterisation of cell types |
| 4 | a. | Portals combine and visualise data from within the DCP and from at least one other source |
|   | b. | Publications that perform integrative analyses of data from within the DCP and from at least one other source are highly cited |
