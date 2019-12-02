### DCP PR:

***Leave this blank until the RFC is approved** then the **Author(s)** must create a link between the assigned RFC number and this pull request in the format:*

`[dcp-community/rfc#](https://github.com/HumanCellAtlas/dcp-community/pull/<PR#>)`

# Research Data Legal Bases and Safeguards
<b>The contents of this informational RFC are a current-state draft and subject to change.</B> Authors of this Informational RFC are confident that the majority of the guidance is appropriate and acceptable, but additional cycles of review by internal and external stakeholders and regulatory experts may require that the guidance provided herein be updated.

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
At a high level, the GDPR is usually implicated whenever a service processes personal data of EU data subjects. More specifically, GDPR applies to “processing of personal data” where the processing activities are related to “offering of goods or services, irrespective of whether a payment of the data subject is required, to such data subjects in the [EU].” Art. 3(2)(a).

Because the DCP is a cloud service (www.data.humancellatlas.org) that will be offered to European users (Data Contributors and Data Users from EU labs) and will likely contain personal data from European donors (Research Data, the focus of this document), the DCP’s processing of covered personal data (e.g. to host and index it to make it available to Data Users) is likely covered by GDPR.

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
<i></i>|Data Contributors|HCA/DCP|Data Users
-------|-----------------|-------|----------
<B>scRNAseq + SAM/BAM files<BR><BR>Sequence and gene count metadata</B>|<b>Data controller</b><BR>- Art. 6 LB: consent or legitimate interests<sup>2</sup>/public interest<br>- Art. 9 LB: explicit consent or scientific purposes (b/c biometric and health/race/ethnicity)<BR><BR><b>Processing purposes</b><BR>- To do scientific research, including: (i) to create secondary analyses (e.g., gene counts), (ii) to upload data to DCP so it can be used for the purpose described in the next column|<b>Data controller</b><BR>- Art. 6 LB: legitimate interests<BR>- Art 9. LB: Scientific purposes based on EU or Member State law or "manifestly made public"<BR><BR><b>Processing purposes</b><BR>- Curation and organization of information, including search indexing, so that Data Users can use it for the purpose described in the next column<br>- Determining who can accses the data (e.g., who can be a Data User) and how they can use it (e.g., Data User policies)|<b>Data controller</b><BR>- Art. 6 LB: legimitate interests/public interests<br>- Art. 9 LB: Scientific purposes based on EU member state law or "manifestly made public"<BR><BR><b>Processing purposes</b><BR>- Scientific and health research to further understanding of cell biology as well as to support discovery of new drug targets and treatments
<b>Gene counts and spatial transcriptomics (e.g., TIFF files)</b>|- Not personal data<BR>- If personal data, then bases as above|- Not personal data<BR>- If personal data, then bases as above|- Not personal data<BR>- If personal data, then bases as above
<b>Data from deceased individuals</b>|- GDPR does not cover personal data from deceased persons (see recital 27)<BR>- If personal data (per member state law), then legal bases as above|- GDPR does not cover personal data from deceased persons (see recital 27)<BR>- If personal data (per member state law), then legal bases as above|- GDPR does not cover personal data from deceased persons (see recital 27)<BR>- If personal data (per member state law), then legal bases as above
	

<sup>1</sup> Note that GDPR requires only one legal basis. However, it is legally possible and common to set up multiple legal bases via a “waterfall” approach, which is to say, build primarily for one, but also invest in a backup should there be limited circumstances where the primary basis is deemed invalid.

<sup>2</sup> LI doesn't work for public institutions.

## Detailed Design
AKA <i>The What, How, and Who</i>: What are the legitimate interests and appropriate safeguards?
### a. What is needed to firm up Article 6 legimitate and public interests?
This should be mostly internal analysis and documentation that lawyers within the GDPR Advisory Group can lead on. Specifically, we need to create a “Legitimate Interest Assessment” (LIA) that balances the DCP’s desired processing against donors’ fundamental data rights and interests. We have started that work here ([LINK](https://docs.google.com/document/d/1CphwZhKu7BTlV1rA6fpf0RjCoVA47Xps/edit)) and will aim to finish it up by end of November.

Given the goals of the project and the safeguards sketched out below, we are not too concerned about demonstrating a legitimate need here for the HCA’s processing.

We believe this work will also help Data Users. While it will ultimately be up to each individual Data User to verify that their uses of DCP data are legitimate and lawful under GDPR, we think we can help lay the groundwork for them by including some of their anticipated and common uses in our LIA, which we plan to make public in order to further transparency as well as to serve as a resource for the community.

### b. What is needed to firm up Article 9 "appropriate safeguards" for scientific research?
The GDPR is not prescriptive on “appropriate safeguards.” This is intentional so as to leave room for organizations to innovate. For our work, our “safeguards categories” presented below are informed by a survey of relevant health and research laws (HIPAA in US and various EU member state research laws), best practices in modern cloud information security (FISMA and NIST 800-53), and ethical best practices as provided via consultation with the HCA’s Ethics Working Group.

Thus, these “safeguard categories” are aimed to meet GDPR (Art. 89 “appropriate safeguards”, Art. 25 privacy by design, Art. 5 overall fairness of data processing) as well as stand up generally to best practices regarding sharing and processing of genetic data.

Process-wise, here is what we are envisioning for internal and external components:
- <b>Internal process.</b> Each category of safeguards has a legal and product owner. This pair is responsible for working together to create a 2-pager proposal for code, processes, and words to manage the core issue the safeguard category focuses on. The job of the legal owner is to translate regulatory requirements into a specific product recommendation and the job of the product owner is to review that those requirements are feasible from a technical and product perspective. The output 2-pagers would then be opened for feedback from the GDPR Advisory Group, Compliance WG, and Ethics WG. The package of final 2-pagers would be signed off by the OC and then be translated into RFCs and put onto tech roadmaps. 
   - High-level compliance objectives are captured in the [DCP Strategic Priorities](https://docs.google.com/document/d/16aKhDBKzHSiCvD_QbxDxKr8YztGnKYugE7vZWtq-3fE/edit). Objectives will be added over time as solutions proposals for each category of safeguard are developed and translated into technical work plans.
- <b>External process.</b> Once we have landed on firm proposals for all these categories, a small group would present our compliance approach to EU Data Regulators sometime in H1 2020. This engagement is necessary because these systems are novel, the laws are broad and standards-based (rather than prescriptive), and thus it’s vital to get the relevant regulators’ buy-in and feedback as part of our compliance strategy.

### c. What "appropriate safeguards" are recommended for the DCP?
Level of data access|Open (Tier 0)|Registered (Tier 1)|Managed (Tier 2)
--------------------|-------------|-------------------|----------------
Analogy|"The Smithsonian"|"A Public Library"|"Special Archives"
Data Type|scRNA seq (raw files) and non-identifying metadata<br>- non-EU consented<BR>- EU deceased (but still ethical consent)<BR>All secondary analyses<BR>- Expression matrices<BR>- Gene Counts<BR>- Spatial transcriptomics|scRNA seq (raw files) and metadata<BR>- EU living persons|Further consideration required to define what falls into this bucket and if those data are a scientific priority
GDPR Art. 89 "Appropriate Safeguards"|__Information Security__ controls to protect data integrity against unwanted deletion or modification<BR><BR><i>Product owner</i>: Sarah Tahiri||
<i></i>|<b>Data Contributor representations upon data upload</b> (e.g., clickthrough agreement flow) and (potentially) spot audits of underlying consent forms to give HCA confidence that they meet GDPR bar for making data fully open<BR><BR><i>Legal owner</i>: Shawn Liu||
<i></i>|<b>Donor control in the form of support for withdrawal of sample</b>. Donors (or their authorized family members if deceased) should be able to withdraw samples previously uploaded to the DCP and DCP should create code and process to support this GDPR-based data right. Note: withdrawal would be going forward only and would not undo prior downloads or uses.<BR><BR><i>Legal owner</i>: Shawn Liu<BR><i>Product owner</i>: UCSC (Kevin Osborn/Brian O’Connor)<BR><i>Links</i>: see current working tech/legal [2-pager](https://docs.google.com/document/d/1H6FURI9Igf6u4snEm6Co9pSH4e23cy83zJ_Uiyu3_7Q/edit?userstoinvite=lblauvel@ucsc.edu&ts=5dcb8f80&actionButton=1)<BR><i>Status</i>: reviewed with tech team; awaiting review with Bird & Bird||
<i></i>|<b>Transparency resources</b>. Basic resources (e.g., HCA DCP Privacy Policy, Privacy Notice for Donors, simple and understandable FAQ) to explain to data donors and public how HCA processes Donor Research Data.<BR><BR><i>Legal owner</i>: Jasper Bovenburg<BR><i>Status</i>: Doc with timelines forthcoming||
<i></i>|<b>Miscellaneous compliance</b>: (1) Data Protection Impact Assessment to document internal analysis of privacy risks and mitigations and (2) Lawful transfer mechanisms<BR><BR><i>Legal owner</i>: Jasper Bovenburg<BR><i>Status</i>: Doc with timelines forthcoming||
<i></i>|<b>Basic Data User Terms</b> potentially via clickthrough<BR><BR><i>Legal owner</i>: Shawn Liu<BR><i>Product owner</i>: Trevor H (?)<BR><i>Consulted</i>: McGill team (Emily and Alex)||
<i></i>|<b>Data minimization practices for metadata</b>. We as DCP do not want any inherently identifiable data to be uploaded as part of Research Metadata and will create code and processes to minimize the risk.<BR><BR><i>Legal owner</i>: Shawn Liu<BR><i>Product owner</i>: Laura Clarke<BR><i>Status</i>: Draft 2-pager to Laura by 11/25/2019||
<i></i>|<i></i>|<b>Engagement with relevant EU data regulators/institutional stakeholders<b> to receive their feedback and buy-in|
<i></i>|<i></i>|<b>Data User registration</b>, including appicable contract ("Data User Terms") whereby User confirms that they are using the data for legitimate scientific research purposes|
	

### Unresolved Questions
- *What aspects of the design do you expect to clarify later during iterative development of this RFC?*

