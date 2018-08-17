
# Query Service


## Description
The Query Service is an Application Programming Interface (API) for querying Human Cell Atlas (HCA) bundle metadata with intuitive query syntax. The first iteration of the Query Service Module will investigate [ElasticSearch (ES) Query Language](https://www.elastic.co/guide/en/elasticsearch/reference/current/_introducing_the_query_language.html) and  [Structured Query Language (SQL)](https://en.wikipedia.org/wiki/SQL).

Services the Query Module provides may complement or subsume the current ES search functionality in the Data Storage Service (DSS).

## Objectives
1. Build, operate, and maintain a metadata API query service that allows a variety of queries to be performed against bundle metadata stored in HCA Data Coordination Platform (DCP) DSS
1. Develop a query language specification for intuitive querying of HCA metadata
1. Conduct user experience (UX) research to validate query language usability, understand remaining usability gaps in DCP data searchability and discoverability
1. Develop a scalable public API for external indexing and other DSS event consumers
1. Deploy all query systems as stand-alone, modular services
1. Make DCP index and query systems agnostic to DCP schema evolution

## In-scope
* Common index event bus and deployment infrastructure
    * Finalize support for multiple metadata indexers in DSS
    * Expose event bus for bundle lifecycle event subscriptions
    * Scalable, fault-tolerant queuing mechanism for loading bundle lifecycle events
* Query access service API and query language
    * Public API for modular query engine plugins
    * In collaboration with the Expression Matrix Service, determine how Query and consumer API interact and are made complementary
* ElasticSearch search engine
    * Stand-alone ES search service, integrated into search plugin interface
    * Increase isolation from metadata schema evolution
* Structured Query Language (SQL) query engine
    * Stand-alone SQL query service, integrated into search plugin interface
    * SQL metadata query prototype and pilot release
        * Socialize with community, investigate use cases, and determine utility of SQL query for these use cases
        * If prototype is viable, implement on the pluggable API, and launch as separate DCP service
    * Database service configuration and deployment infrastructure
    * ETL layer and database schema design
* Documentation
    * Documentation for API and query language use with metadata
    * Demonstration use of service; developer example code to enable fast and easy third party adoption of API
* UX Research and Feedback
    * UX validation of API and query language with [tertiary portal](https://www.humancellatlas.org/data-sharing) community
    * Provide feedback to DCP about public API suitability in building layered services (e.g. can a new Query microservice be built using only public DSS API?)
    * Provide feedback to the metadata group on schema changes that could support intuitive queries of the metadata

## Out-of-scope
* Search user interface, search app and/or tertiary portal work
* GraphQL or other "query" languages or environments (ES and SQL should be sufficient to prove utility of design)
* Upkeep and improvement of existing ES indexer ETL layer

## Milestones
* **EOY 2018** - Architectural work done, pilot/preview implementation of SQL query framework; feature complete, suitable for limited use, but not scalable
* **Early 2019** - DSS ES migrated to public API, improved resilience to metadata schema evolution
* **Mid-2019** - Performant, highly scalable version, suitable for interactive data serving across entire HCA data corpus


## Roles

### Project Lead
[Genevieve Haliburton](mailto:ghaliburton@chanzuckerberg.com)

### Product Owner
[Matt Weiden](mailto:mweiden@chanzuckerberg.com)

### Technical Lead
[Andrey Kislyuk](mailto:akislyuk@chanzuckerberg.com)


## Communication

### Slack Channels
HumanCellAtlas/query-service: Discussion of query service

## Github repositories
* https://github.com/HumanCellAtlas/query-service
