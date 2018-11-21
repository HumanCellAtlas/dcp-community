# Unity Benchmarking Service

## Description

The Unity Benchmarking Service automates the process of running and benchmarking data processing pipelines.

## Objectives

The Unity Benchmarking Service aims to automate benchmarking for data processing pipelines, enforce standards for consistent reporting for historic comparison and enable the those external to the DCP implementation team visibility and opportunity to demonstrate improvements to data processing pipelines using the same infrastructure as the HCA data processing pipelines team. The Unity Benchmarking Service will be used by those making data processing pipelines for the HCA data processing service and will be hosted in a public-facing manner; so anyone can benchmark their data processing pipelines against benchmarked pipelines in the DCP.

## Definitions

__Community pipeline:__ A pipeline that is not officially used as a standard in the HCA project, but runs in the DCP and is operated by the Data Processing Service to enable community members to work with a diversity of submitted data until they have a standardized pipeline. These pipelines are not as vetted as standardized pipelines.  
__HCA Analysis Working Group (AWG):__ A group of domain specialists that advise on and monitor scientific choices made in the Human Cell Atlas Project.  
__Standardized Pipeline:__ An official pipeline of the HCA project.  

## In-scope

* Benchmarking for standardized HCA data processing pipelines will be automated and included in Unity
* Data used to run pipelines during benchmarking will be standardized and enforced to promote comparable analysis runs
* Benchmarking data can change without notice but will be consistent in file format and file schema with previous data. If file format or schema do change (due to updates in the DCP or in standardized data processing pipelines) notice will be given to users
* Leaderboards showing how pipelines that process the same assay differ in performance will be created as different pipelines for the same assay are collected
* Documentation for the requirements of pipelines to be benchmarked by the Unity Benchmarking Service
* Documentation on benchmarking process and outputs

### Scientific "guardrails"

* Pipelines benchmarking standardized HCA data processing pipelines are reviewed by HCA scientific leadership (Analysis Working Group) and treated as standardized data processing pipelines. Changes to these specific benchmarking pipelines will directly affect how standardized HCA data processing pipelines are optimized and comment on what are better scientific results produced by the data processing pipeline they benchmark.
* Metrics and their use in leaderboards must be reviewed by the HCA scientific leadership (Analysis Working Group).

## Out-of-scope

* Educational training or user assistance on how to create pipelines
* Recommendations on how to improve pipelines based on benchmarking
* Resources will be available in Unity to benchmark standardized HCA pipelines. These resources will be assay specific and can be reused by community pipelines processing similar data. Setting up resources in Unity specifically for community pipelines that will not be used for standardized HCA pipelines is outside the scope of the HCA Data Processing Pipelines Team.

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
