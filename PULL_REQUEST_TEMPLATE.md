<!--
See [RFC Process Instructions](https://github.com/HumanCellAtlas/dcp-community/blob/master/rfcs/text/0001-rfc-process.md) for process instructions.

* Work in Progress: if this RFC is a work in progress, prefix the title with WIP. This indicated to the community that 
this PR is not ready for review.
-->


## Check List
### Community Review
- [ ] Add Labels
  - *For software RFCs*: add the "rfc-community-review" and the appropriate software project name (such as "Data 
  Store") labels. When the RFC impacts multiple DCP software projects, then the "Architecture" label MUST be the 
  project name. If uncertain about the appropriate project name, then Ask a PM on the 
  [HCA #dcp-project-mgmt](https://humancellatlas.slack.com/messages/C53S0GBS7) slack channel
  - *For governance RFCs*: Add the "rfc-community-review" and the "Governance" labels.
- [ ] **April 1**: Community Review Last Call <!--Should be at least 2 weeks-->
- [ ] Request review on Slack channel [#dcp](https://humancellatlas.slack.com/messages/C51A8EDQQ).
- [ ] **Shepherd**: Summarize the review discussion for the Approvers in the top-level summary comment in the RFC pull 
request

### Oversight Review
- [ ] **April 8**: Oversight Review Last Call <!--Should be at least 1 weeks-->
- [ ] Replace "rfc-community-review" with "rfc-oversight-review"
- [ ] Request Review on Slack Channels. Include a link to the PR, and the last call date.
  - For *software RFCs*: [#tech-architecture ](https://humancellatlas.slack.com/messages/C6SGU0R3J)
  - For *governance RFCs*: [#dcp-project-mgmt](https://humancellatlas.slack.com/messages/C53S0GBS7)
- [ ] Add RFC as an agenda item to next meeting:
  - DCP Architecture meeting for *software RFCs*
  - DCP PM meeting for *governance RFCs*
  
### Approval
- [ ] Replace "rfc-oversight-review" with "rfc-approved"
- [ ] Rename the RFC from 0000-my-feature.md to ####-my-feature.md (with leading zeros) where #### is the next 
 available RFC number
- [ ] Create a link between the approved RFC and its pull request by updating the DCP PR section in the RFC template 
 and pushing the commit.

### Finally
Once all tasks are complete merge the PR in and Announce the approval in 
[#dcp](https://humancellatlas.slack.com/messages/C51A8EDQQ).