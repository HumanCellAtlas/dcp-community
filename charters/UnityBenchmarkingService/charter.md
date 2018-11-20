# Unity Benchmarking Service

## Description

The Unity benchmarking service automates the process of running and benchmarking data processing pipelines.

## Objectives

The Unity benchmarking service aims to automate benchmarking for data processing pipelines, enforce standards for consistent reporting for historic comparison and enable the those external to the DCP implementation team visibility and opportunity to demonstrate improvements to data processing pipelines using the same infrastructure as the HCA data processing pipelines team. The Unity benchmarking service will be used by those making data processing pipelines for the HCA data processing service and will be hosted in a public-facing manner; so anyone can benchmark their data processing pipelines against benchmarked pipelines in the DCP.

## In-scope

* Benchmarking for standardized HCA data processing pipelines will be automated and included in Unity
* Data used to run pipelines during benchmarking will be standardized and enforced to promote comparable analysis runs
* Benchmarking data can change without notice but will be consistent in file format and file schema with previous data. If file format or schema do change (due to updates in the DCP or in standardized data processing pipelines) notice will be given to users
* Leaderboards showing how pipelines that process the same assay differ in performance will be created as different pipelines for the same assay are collected
* Documentation for the requirements of pipelines to be benchmarked by the Unity Benchmarking Service
* Documentation on benchmarking process and outputs

### Scientific "guardrails"

* Pipelines benchmarking standardized HCA data processing pipelines are reviewed by HCA scientific leadership (Analysis Working Group) and treated as standardized data processing pipelines. Changes to these specific benchmarking pipelines will directly affect how standardized HCA data processing pipelines are optimized and comment on what are better scientific results produced by the data processing pipeline they benchmark.
* Metrics and how metrics are used in the leaderboards are reviewed by the HCA scientific leadership (Analysis Working Group).

## Out-of-scope

* Educational training or user assistance on how to create pipelines
* Recommendations on how to improve pipelines based on benchmarking
* Pipelines benchmarking community data processing pipelines are not required to be included in the Unity benchmarking service (but are allowed). Community members may benchmark community pipelines using data and file format standards in the system. If community pipelines process assays not currently processed by standard HCA pipelines or assays not being scoped by the team, setting up the system will require a significant amount of work and will be evaluated given team bandwidth

## Milestones and Deliverables

__2018:__ Infrastructure operational and secure, HCA data processing pipelines team has initiated using system with a standardized HCA data processing pipeline.   
__2019:__ System updated to include all data processing pipelines operational as standardized HCA pipelines.   
__2019:__ Leaderboards are created for standardized HCA pipelines.   

## Roles

### Project Leads

[Kylee Degatano](mailto:kdegatano@broadinstitute.org)   
[Timothy Tickle](mailto:ttickle@broadinstitute.org)   

### Product Owner

[Vicky Horst](mailto:vicky@broadinstitute.org)

### Technical Lead

[Jonathan Bistline](mailto:bistline@broadinstitute.org)

## Communication

### Slack Channels

[HumanCellAtlas/unity-benchmarking](https://humancellatlas.slack.com/messages/unity-benchmarking): To answer questions and get feedback on pipeline benchmarking   

## Github repositories
[Unity](https://github.com/HumanCellAtlas/unity): Source code for the Unity service.