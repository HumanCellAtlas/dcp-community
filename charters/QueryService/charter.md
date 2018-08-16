
# Query Service


## Description
The Query Service is an Application Programming Interface (API) for querying Human Cell Atlas (HCA) bundle metadata with different query languages.

## Objectives
1. Build, operate, and maintain a metadata API query service that allows a variety of queries to be performed against bundle metadata stored in HCA Data Coordination Platform (DCP) Data Storage Service (DSS)
1. Develop a query language specification and metadata Extract, Transform, Load (ETL) pattern to accommodate intuitive querying
1. Conduct user experience (UX) research to validate query language usability, understand remaining usability gaps in DCP data searchability and discoverability
1. Develop a scalable public API for external indexing and other DSS event consumers
1. Migrate existing ElasticSearch (ES) query subsystem to this public API
1. Deploy all query systems as stand-alone, modular services
1. Make DCP index and query systems agnostic to DCP schema evolution

## In-scope
* Common index event bus and deployment infrastructure
    * Finalize support for multiple metadata indexers in DSS
    * Expose event bus for bundle lifecycle event subscriptions
    * Scalable, fault-tolerant queuing mechanism: Amazon Web Services (AWS) Simple Queuing Service (SQS) push or similar
* Query access service API and query language
    * Define public DSS API for modular, pluggable query engines, and hand off to DSS team for long-term ownership (this includes API to trigger index/query processing, and to handle end-user queries)
    * In collaboration with Expression Service project, determine how Query and consumer API interact and are made complementary
    * Explore technical methods of isolating index and query systems from metadata schema evolution
* ElasticSearch search engine
    * Migrate ES engine to pluggable interface, and factor into separate DCP service
    * Improve isolation from metadata schema evolution
* Structured Query Language (SQL) query engine
    * SQL metadata query prototype and pilot release
        * Socialize with community, investigate use cases, and determine utility of SQL query for these use cases
        * If prototype is viable, implement on the pluggable API, and launch as separate DCP service
        * Implementation details
            * Database service configuration and deployment infrastructure
            * API front-end
            * ETL layer and database schema
            * Event handler that adds and removes bundle metadata from the index
            * Unit, integration, and scale tests
    * Documentation for API and query language use with metadata
    * Demonstration use of service; developer example code to enable fast and easy third party adoption of API
    * Provide feedback to DCP about public API suitability in building layered services (e.g. can a new Query microservice be built using only public DSS API?)

### Scientific "guardrails"
* UX validation of API and query language with tertiary analysis community

## Out-of-scope
* Search user interface (UI), search app and/or tertiary analysis work
* GraphQL or other "query" languages or environments (ES and SQL should be sufficient to prove utility of design)
* Upkeep and improvement of existing ES indexer ETL layer

## Milestones
* **EOY 2018** - architectural work done, ES migrated to public API, improved resilience to metadata schema evolution.
* **Early 2019** - pilot/preview implementation of SQL query framework; feature complete, suitable for limited use, but not scalable
* **Mid-2019** - performant, highly scalable version, suitable for interactive data serving across entire HCA data corpus


## Roles

### Project Lead
[Genevieve Halliburton](mailto:ghaliburton@chanzuckerberg.com)

### Product Owner
[Matt Weiden](mailto:mweiden@chanzuckerberg.com)

### Technical Lead
[Andrey Kislyuk](mailto:akislyuk@chanzuckerberg.com)


## Communication

### Slack Channels
HumanCellAtlas/query-service: Discussion of query service

## Github repositories
* https://github.com/HumanCellAtlas/query-service
