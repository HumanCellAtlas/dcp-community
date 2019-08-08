### **DCP PR:**

*Leave this blank until the RFC is approved then the Author(s) must
create a link between the assigned RFC number and this pull request in
the format:*

\[dcp-community/rfc\#\](https://github.com/HumanCellAtlas/dcp-community/pull/\<PR\#\>)

**Informational RFC: Managed Access Data in the Datas Control Platform**
========================================================================

**Summary**
-----------

This RFC describes the requirements for the DCP to support storing and
controlling access to managed access data.

**Author(s)**
-------------

*Recommended format for Authors:*

\[Kevin
Osborn\]([[mailto:kosborn2\@ucsc.edu]](mailto:kosborn2@ucsc.edu))

\[Trent
Smith\]([[mailto:trent.smith\@chanzuckerberg.com]](mailto:trent.smith@chanzuckerberg.com))

\[Brian
Hannafious\]([[mailto:bhannafi\@ucsc.edu]](mailto:bhannafi@ucsc.edu))

\[Brian
O'Connor\]([[mailto:broconno\@ucsc.edu]](mailto:broconno@ucsc.edu))

**Shepherd**
------------

\[Name\](mailto:username\@example.com)

**Motivation**
--------------

Many researchers collecting cellular resolution genomic data are doing
so using donor consents which require the sequence level and any
clinical phenotypic data to only be released to bona fide researchers
for specific research use. This can happen for many reasons including
the national law where they are based or the types of individuals they
are collecting data from e.g individuals with a disease. This means
there is a growing set of data suitable for building the Human Cell
Atlas (HCA) which can't currently be stored, processed and distributed
by the Data Coordination Platform (DCP).

The processing of genetic and genomic information necessarily raises
questions as to what information is 'identified or identifiable' (and
thus subject to GDPR) and what information is 'anonymous or anonymized'
(and thus not subject to GDPR). These difficulties are both legal (for
the purposes of determining GDPR's scope of application) and scientific
-- for the purposes of determining what data is sensitive for data
subjects, for the purposes of determining what information the data can
reveal about the data subject (either alone or in combination with other
datasets). Scientific difficulties also inhere, for the purposes of
determining what external data can be combined with the data subject's
data to reveal their identity.

This proposal will describe the architectural components and the
security compliance requirements necessary for the DCP to be approved as
an archive for controlled access data from a variety of nations. There
are specific guidelines that must be met before controlled access data,
especially US government research data, can be stored onto the system.
The safe storage rules for US data is different from the rules in other
countries (though there is a lot of overlap). Each set of rules will
have specific requirements that must be met. This RFC will also outline
the steps required to meet basic security compliance requirements
necessary before controlled access data can be stored in the system.

### **Definitions**

***Managed access data*** - Raw and processed genomic and phenotypic
data from individuals whose consent agreements authorise data release
only for specific research use to bona fide researchers.

***Open access data*** - Raw and processed genomic and phenotypic data
from individuals whose consent agreement authorise unrestricted sharing
and summary level data generated from both open and managed access raw
and processed genomic data.

***HCA Data Contributor Agreement*** - an agreement entered into between
the Data Contributor and the HCA, prior to contribution and upload of
data to the HCA DCP.

***Data Access Committee (DAC) -*** A committee responsible for deciding
who should be able to access data from a study, and what use the
researcher can make of that data. Each researcher submits a request of
who they are, which data they want, and what they will use it for. Each
type of use requires a separate request to the DAC.

### **User Stories**

As a Researcher with a keyboard, I want to find data that is similar to
my own. I download an expression matrix for that data, then
analyze/visualize it locally using my own workflow. I would like this to
work while enforcing the access permissions for the data, returning
results for which I have permission.

As a Researcher with a pipette, I want to use the Data Browser and/or
tertiary analysis portals to look for interesting patterns of a gene or
cell type expression. For example, I might look for all the tissues
where my gene of interest is expressed, or try to find a combination of
gene expressions to identify a pathway in my disease of interest. I
would like this to work while enforcing the access permissions for the
data, returning results for which I have permission.

As a Researcher with a pipette, I would like to be able to share all my
cellular resolution, human genomic data with the HCA DCP, including data
which needs to be distributed via managed access as well as data which
can be openly released. I want this to support the aim to build an atlas
of all human cells.

As a researcher with a pipette, I would like to manage a Data Access
Committee (DAC) so that I can control access to individuals and groups
as follows: (NOTE: this is the researcher-run DAC model, which is the
least burden to the DCP, but the highest burden to the researcher)

1.  (Submitter/Wrangler role) I would like to grant or revoke read-write
    > access to individuals or groups for data submission purposes

2.  (Consumer role) I would like to grant or revoke read-only access to
    > individuals or groups based on the type of use they are requesting

3.  (Admin role) I would like to grant or revoke read-write access to
    > individuals or groups for data management purposes (including
    > possible redaction of data)

The objects being controlled are files, bundles, and collections, as
well as setting permissions for all objects related to a project.

As a researcher with a pipette, I would like to set an embargo for
access to my data so that I may finish my research and publish results
based on my data before others. I would like to restrict access to my
data for a set period of time.

**Scientific \"guardrails\"** 
------------------------------

This plan will be reviewed by members of the \[Compliance working
group\]([[https://github.com/HumanCellAtlas/dcp-community/blob/master/charters/Compliance-WG/charter.md]](https://github.com/HumanCellAtlas/dcp-community/blob/master/charters/Compliance-WG/charter.md))
who will ensure the proposed design meets their guidelines for
responsible handling of managed access genomic data.

**Detailed Design**
-------------------

A tiered access model:

**Open Access**: The HCA project heavily favors open-access data. This
is the primary use case for the DCP. Users will not be required to log
in to gain access to any openly consented data. Searches executed for
unauthenticated users will only return open access data.

**Controlled Access**: To access controlled data the user will have to
log in to the system to identify themselves. Authentication will happen
via OIDC mechanisms. Once authenticated, the user's searches will return
open access data as well as controlled access data to which they have
access.

**Controlled Objects:** Access can be controlled on files, bundles and
collections. Individuals and groups will be granted access via DAC
approval. This process will follow the DUOS model (see link below) being
defined by GA4GH where the types of uses are categorized. This allows
the DAC to answer some or all access requests programmatically.

**Not supported:** We will not provide a notification mechanism to let
users know when permissions have been changed.

**High level design of Authorization:** As stated above, all access to
controlled data will require the user to be authenticated first. This
user ID is then used for authorization. A repository of permissions will
act as the authorization module that stores what actions an individual
or group can take on an object. A service called Fusillade will be the
interface to this permissions repository. Each service will need to
consult Fusillade to determine access before returning results to a
user. For instance, to access controlled data through the DSS API will
first require a user to login (using the login command). Then the API
will use the identity as a token passed in with command like \`hca dss
get-file \--uuid \<UUID here\> ...\`. The DSS will then check with
Fusillade to see if that user has permission to read the file that
matches that UUID. If so then the user will receive the file, and if not
then they will receive a permissions error code. All DCP modules will
have to follow the same model. Any user or module attempting to access
controlled data in the DSS will need to authenticate first. Then they
will need to use that ID key along with their request for data. The DSS
will then check with Fusillade before granting access.

**Impact on other DCP APIs:** All programmatic access to the DCP will
need to follow the same pattern when trying to access controlled data in
the DCP. They will need to provide a login API so that the user can be
authenticated and a user key obtained. Here is a current list of the DCP
APIs that will be impacted.

-   Matrix API

<!-- -->

-   Azul API (this is the browser back end API, which is not currently public)

-   Query service

-   DSS API - REST endpoints

-   DSS API - Python bindings

-   Ingest API

Each of these APIs rely on accessing data in the DSS, and will need to
provide the user ID key when making calls to the DSS.

### **Acceptance Criteria** 

The acceptance criteria will be made up of a mix of functional and
regulatory checks. We will obviously need to check that the access
controls function as designed, and access is granted or denied when it
is supposed to. We will also need to prepare for review by various
regulatory bodies. HCA will ultimately need to decide on which
regulatory agencies to seek approval from. For the purposes of this RFC
it is sufficient to be reviewed for the GDPR Data Protection Impact
Assessment, and work though the NIST 800-53 Access Control [control
family](https://nvd.nist.gov/800-53/Rev4/family/Access%20Control) checks
(25 separate controls).

**Functional tests:** Here is an oversimplified set of tests to give the
reviewers a sense of what would need to be tested.

Positive tests:

-   Explicit rights: Users can access data where they have been granted
    > access. This set of tests would include checking access to files,
    > bundles and collections where the user or a group that the user
    > belongs to has been granted explicit access.

-   Implicit rights: Users can access public data where they have not
    > been granted any specific access rights. This set of tests would
    > include checking access to files, bundles and collections.

Negative tests:

-   Explicit Denial: Users will be denied access to controlled data
    > where they have not been granted explicit access. This set of
    > tests would include checking access to files, bundles and
    > collections for a user without access, or a user that does not
    > belong to a group with access.

**Access controls for NIST 800-53** are very well prescribed. They can
be summarized as follows: (Consult the document for the authoritative
definition of work)

-   Privileged users are managed from a central place

-   There is a prescribed onboarding and offboarding process for all
    > users

-   There is an audit trail for onboarding and offboarding

-   Every "layer" of the system authenticates. Each component in the
    > system authenticate and all the connections authenticate somehow
    > even if on a "trusted" network.

-   Operate on a least privilege model with developers and users getting
    > the rights that they require, and not more. We will have a process
    > for elevating privileges

-   We will require a warning banner that you're entering a secure area
    > on login and/or registration. We will want to wrap this in some
    > sort of Terms Of Service that prompts users each time they change.
    > Audit the acceptance of the terms of service.

-   Must provide a logon and logoff capability from the UI. Ideally we
    > will have an automatic logout due to inactivity like a bank does.

This list is meant to get the flavor of what will be required by the
regulations for NIST 800-53. The Access Control control family of checks
exist inside a much larger framework of controls made up by the rest of
NIST 800-53, and it should be noted that completing the other controls
in the document would be required for certification of the DCP to FISMA
moderate levels so that it can store controlled access data generated by
funds from NIH grants.

It should be noted again that though these controls are specific to
access control requirements in the USA, they are expected to represent a
good starting point for other controls from other nations.

### **Unresolved Questions**

### Design questions which should be resolved during the RFC process

-   Are search results filtered by access control?

-   (related) Is Metadata always searchable

-   What certifications are required to meet the feature request
    > "support controlled data in the DCP"? FISMA moderate? GDPR? What
    > else?

-   Should we have the researchers run the DAC? Perhaps providing them
    > with a template?

### **Prior Art** 

[[Analysis of five years of controlled access and data sharing
compliance at the International Cancer Genome
Consortium]](https://www.nature.com/articles/ng.3499?proof=true&draft=journal)
: Good article about running a DAC

[[Data Sharing in the Post-Genomic World: The Experience of the
International Cancer Genome Consortium (ICGC) Data Access Compliance
Office
(DACO)]](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1002549)
: A good reference on creating a tiered access data repository.

[[DUO
overview]](https://docs.google.com/presentation/d/1B4jsqnZIqwxLjL8Y1q41kYFNN8BsmmiEC6I9_WZZPt0/edit#slide=id.p)
from GA4GH Basel presentation

[[Data Use & Researcher
Identities]](https://www.ga4gh.org/work_stream/data-use-researcher-identities-duri-2/)
Effort to standardize and automate the granting of access based on data
use and researcher identity patterns.

[[DCP User
stories]](https://docs.google.com/document/d/1uYdNEnBjl_tMJdGSh8f3VFDd0aw_hhhs2egyvkIfOeg/edit)
(note controlled access user stories missing)

[[NIST-800-53]](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53Ar4.pdf)
FedRAMP

[[Fusillade
RFC]](https://docs.google.com/document/d/1qD4bgnO684GtcLBErxYNZ5gvMaQPF8DsIdUBua5ZDBs/edit#heading=h.x01v6l5tv57z)
describes the architecture for access control as implemented in the DCP

### **Alternatives** 

[[Near-term strategy for Managed Access in the
DCP]](https://docs.google.com/document/d/1g_20xRCshmr5gmgr0OY64wW34r0DYxVRFpo6MhlXWkM/edit?pli=1#heading=h.hld7rftpusci).
This approach uses a pre-existing external archive (data store) that
already supports controlled access data. Identifiable data would be
stored there with the derived non-identifiable data (expression data)
being stored in the DCP.
