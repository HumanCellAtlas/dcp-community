### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# Research Data Legal Bases and Safeguards
## Summary
This document summarizes the GDPR compliance approach for the HCA DCP’s processing of Donor Research Data (defined as scRNA seq, associated metadata, and gene count and expression matrices).

The audiences for this document are the people on the HCA team working on GDPR compliance, specifically, (1) the Compliance Working Group, (2) the GDPR Advisory Group consisting of reps from Broad, Wellcome, Sanger, EMBL, CZI, and McGill, and (3) any DCP development team members whose partnership groups (1) and (2) will need in order to execute the recommended compliance approach.

The analysis and recommendations in this document will contain and/or inform other work product (e.g. Data Protection Impact Assessment, Legitimate Interest Assessment, overall GDPR compliance narrative) that will eventually be European Data Regulator facing.

Below are links to related and supporting documents that form a baseline for this document:
- [Glossary, including current positions regarding identifiability of data types](https://docs.google.com/document/d/1kAx8F5e0wHTKNKzIb1n01OIAZfslUfreJXBp38p0n1c/edit#)
-   [DPIA draft](https://docs.google.com/document/d/1ircWN3s-OY-1g9BFZEEB18e8NgNycNY6gnG-Vv7f7Mg/edit?ts=5d4995e1#heading=h.qr9larxbh9ql)
-   [Data diagram (draft)](https://drive.google.com/drive/u/0/folders/19_5_DNitRBTHGAJ1dkx-KOHhIkL3dwyL)

## Author(s)
[Shawn Liu](mailto:sliu@chanzuckerberg.com)
[Compliance Working Group](mailto:compliance-wg@data.humancellatlas.org)

## Shepherd
[Sarah Tahiri](mailto:stahiri@broadinstitute.org)

## Motivation
AKA *The Why*: High-level context on GDPR, including recommended Legal Basis for DCP’s processing of Research Data

### a. The GDPR likely applies to the DCP's processing of personal data and Research Data is likely personal data
At a high level, the GDPR is usually implicated whenever a service processes personal data of EU data subjects. More specifically, GDPR applies to “processing of personal data” where the processing activities are related to “offering of goods or services, irrespective of whether a payment of the data subject is required, to such data subjects in the [EU].” ​Art. 3(2)(a).​

Because the DCP is a cloud service (​www.data.humancellatlas.org)​ that will be offered to European users (Data Contributors and Data Users from EU labs) and will likely contain personal data from European donors (Research Data, the focus of this document), the DCP’s processing of covered personal data (e.g. to host and index it to make it available to Data Users) is likely covered by GDPR.

Below is a list of some key (but not exhaustive) categories of GDPR-covered personal data. Note that this document focuses on Donor Research Data, as we believe that this is the most sensitive category of data the DCP processes. We will cover lower sensitivity data categories later in our diligence process.

- More likely to be personally identifiable
  - Donor research data
    - Raw sequencing data (scRNAseq + SAM/BAM files)
    - Sequence and gene count metadata
  - Data Contributors
	  - Name and contact information (e.g., email and institution)
	  - DCP tool usage and analytics information (e.g., they uploaded dataset *x* and agreed to the Data Contributor Agreement at time *t*)
   - Data Users
	   - Name and contact information
	   - DCP tool usage and analytics information (e.g., they downloaded dataset *x* and agreed to the Data User Agreement at time *t*) 
-  Much less likely to be personally identifiable or otherwise not personal data covered under GDPR
	- Gene counts and spatial transcriptomics
		- We believe this data alone (without other identifying data) is highly unlikely to be identifiable. That said, we will still design reasonable safeguards around this data in keeping with our data stewardship approach to designing the entire DCP tool.
   - Data from deceased persons *Recital 27*
	   - There are some exceptions (e.g., Denmark) to this rule that we will of course carefully consider as part of designing the DCP tool.

### b. DCP intends to rely primarily on a non-consensual "scientific research purposes" and "legitimate interests" legal bases for processing Donor Research Data
Under GDPR, all processing of personal data requires what’s called a “legal basis” (see [Article 6 of GDPR](https://gdpr-info.eu/art-6-gdpr/)) and if that data is “sensitive,” (which “genetic data” is), we will need to demonstrate an additional grounds for processing of sensitive data (see [Article 9 of GDPR](https://gdpr-info.eu/art-9-gdpr/)).

After extensive discussion with the GDPR Task Force, Organizing Committee, and outside counsel at Bird & Bird law firm, below are the recommended legal bases for the DCP’s processing of Donor Research Data.
- Article 6: **legitimate interests**. See *Art. 6(1)(f)*.
- Article 9: primarily “**scientific research purposes**” (see *Art. 9(2)(j)*), with “**personal data which are manifestly made public**” (see *Art. 9(2)(e)*) as a backup<sup>1</sup>.

There are a few key things to understand about the selection of these legal bases:
- We are not primarily relying on consent as a legal basis for GDPR. Data Contributors still likely need Donor consent to comply with local human subjects research laws and institutional ethics requirements. However, because it’s not possible for Data Contributors to individually list all potential downstream data controllers of Donor Research Data (e.g. Data Users) at the outset of Donor collection, it will be difficult for Data Contributors to meet GDPR’s consent requirements. Thus, while the HCA will not forbid a Data Contributor’s reliance on consent as grounds for their processing (e.g. uploading Donor Research Data to the DCP), we have chosen to invest and establish in our own non-consensual legal basis for our processing.
- We need to “earn” the non-consensual bases we’ve chosen through designing and building “appropriate safeguards” (see Art. 89(1)) that balance Donors’ fundamental “rights and interests” in their sensitive personal data (see Art. 9(2)(j)) against the DCP’s interests in facilitating scientific research. The rest of this document will focus on scoping out what exactly we define these “appropriate safeguards” to be.
- Finally, every data controller in the DCP ecosystem (Data Contributor, DCP, Data User) needs her own legal basis for processing of personal data. This document focuses on the DCP, as Data Contributors and Data Users will be subject to their own set of local rules that they may need to work with legal counsel at their institutions to navigate. That said, we realize that the system should be designed as a whole and the document below will discuss Data Contributors and Data Users as needed for system coherence.

### c. GDPR Legal Basis for Donor Research Data - summary chart
|   |Data Contributors|HCA/DCP|Data Users|
|---|---|---|---|---|
|scRNAseq + SAM/BAM files<br><br>Sequence and gene count metadata|Data Controller<br> - <u>Art. 6 LB</u>: consent or legitimate interests<sup>2</sup>/public interest<br> - <u>Art. 9 LB</u>: explicit consent or sci. purposes (b/c biometric and health/race/ethnicity)<br><br>Processing Purposes<br> - To do scientific research including:<br> - (i) to create secondary analyses (e.g., gene counts)<br> - (ii) to upload data to DCP so it can be used for the purpose described in the next column|Data Controller<br> - <u>Art. 6 LB</u>: legitimate interests<br> - <u>Art. 9 LB</u>: sci. purposes based on EU or Member State law or "manifestly made public"<br><br>Processing Purposes<br>- Curation and organization of info, including search indexing, so that Data Users can use it for the purpose described in the next column<br>- Determining who can access the data (e.g., who can be a Data User) and how they can use it (e.g., Data User policies)|Data Controller<br>- <u>Art. 6 LB</u>: legitimate interests/public interests<br>- <u>Art. 9 LB</u>: sci. purposes based on EU Member State law or "manifestly made public"<br><br>Processing purposes<br>- Scientific and health research to further understanding of cell biology as well as to support discovery of new drug targets and treatments|   |
|   |   |   |   |   |
|   |   |   |   |   |


<sup>1</sup> Note that GDPR requires only one legal basis. However, it is legally possible and common to set up multiple legal bases via a “waterfall” approach, which is to say, build primarily for one, but also invest in a backup should there be limited circumstances where the primary basis is deemed invalid.
## Detailed Design
*Explain the design in sufficient detail such that the implementation and (if appropriate) the interaction with existing DCP software are both reasonably clear.*

### Unresolved Questions
- *What aspects of the design do you expect to clarify further through the RFC review process?*
- *What aspects of the design do you expect to clarify later during iterative development of this RFC?*

