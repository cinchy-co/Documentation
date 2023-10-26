# Release package

## Overview

This page details the architecture of the release package for CinchyDXD 2.0.

## Architecture

The architecture of a release package in CinchyDXD 2.0 separates into a folder for each entity you exported from the source environment. Each folder has the respective data relevant to that entity.


## CinchyDXD 2.0 example release package

The following list is an example of the release package folder architecture and their respective files. 

```markdown
Release Package
│   manifest.json
├───Data Experience Definitions
│   └───Demo
│           metadata.json
│           model.xml
│
├───Data Sync Configurations
│   ├───Data Sync 1
│   │       config.xml
│   │       metadata.json
│   │
│   ├───Data sync 2
│   │       config.xml
│   │       metadata.json
│   │
│   └───Sync basicauth endpoint
│       │   config.xml
│       │   metadata.json
│       │
│       └───Listener Configs
│           ├───Example1
│           │       connection-attributes.json
│           │       metadata.json
│           │       topic.json
│           │
│           └───Example2
│                   connection-attributes.json
│                   metadata.json
│                   topic.json
│
├───Domains
│   ├───Domain 1
│   │   │   metadata.json
│   │   │
│   │   └───Saved Queries
│   │       ├───SQ1
│   │       │   │   metadata.json
│   │       │   │   query.sql
│   │       │   │
│   │       │   └───Webhooks
│   │       │       └───Webhook 1
│   │       │               metadata.json
│   │       │
│   │       ├───SQ2
│   │       │   │   metadata.json
│   │       │   │   query.sql
│   │       │   │
│   │       │   └───Webhooks
│   │       │       └───Webhook 2
│   │       │               metadata.json
│   │       │
│   │       └───PostInsallQuery 
│   │               metadata.json
│   │               query.sql
│   │
│   └───Domain 2
│       │   metadata.json
│       │
│       └───Tables
│           ├───Sample Table
│           │   ├───Entitlements
│           │   │       metadata.json
│           │   │
│           │   ├───Formatting Rules
│           │   │       metadata.json
│           │   │
│           │   ├───Reference Data
│           │   │   ├───Sample Table with HLink or  MSH
│           │   │   │       config.xml
│           │   │   │       data.csv
│           │   │   │       metadata.json
│           │   │   │
│           │   │   └───Sample Table without HLink or MSH
│           │   │           config.xml
│           │   │           data.csv
│           │   │           metadata.json
│           │   │
│           │   └───Views
│           │       └───Default
│           │           │   metadata.json
│           │           │
│           │           └───Columns
│           │                   metadata.json
│           │
│           ├───Sample Table Types Link Target
│           │   ├───Entitlements
│           │   │       metadata.json
│           │   │
│           │   ├───Formatting Rules
│           │   │       metadata.json
│           │   │
│           │   ├───Reference Data
│           │   │   └───Sample Table Types Link Target
│           │   │           config.xml
│           │   │           data.csv
│           │   │           metadata.json
│           │   │
│           │   └───Views
│           │       └───View 1
│           │           │   metadata.json
│           │           │
│           │           └───Columns
│           │                   metadata.json
│           │
│           └───Nested Link Target
│               └───Reference Data
│                   └───Nested DXD Link Target
│                           config.xml
│                           data.csv
│                           metadata.json
│
├───External Secrets Manager
│   └───Local Secrets
│       ├───Secret 1
│       │       metadata.json
│       │
│       └───Secret 2 2
│               metadata.json
│
├───Groups
│   ├───G1
│   │       metadata.json
│   │
│   ├───G2
│   │       metadata.json
│   │
│   └───G3
│           metadata.json
│
├───Integrated Clients
│   └───IG 1
│           metadata.json
│
└───User Defined Functions
    ├───UDF 1
    │       metadata.json
    │       script.js
    │
    └───UDF 2
            metadata.json
            script.js
```


## Visual representation

The example below is a simplified visual representation of the folder structure. 

```mermaid
graph LR
    A[Release Package]
    A --> B[Data Experience Definitions]
    B --> BA[Demo]
    BA --> BAA[metadata.json]
    BA --> BAB[model.xml]
    
    A --> C[Data Sync Configurations]
    C --> CA[Data Sync 1]
    CA --> CAA[config.xml]
    CA --> CAB[metadata.json]
    C --> CB[Data Sync 2]
    CB --> CBA[config.xml]
    CB --> CBB[metadata.json]
    C --> CC[Sync basicauth endpoint]
    CC --> CCA[config.xml]
    CC --> CCB[metadata.json]
    CC --> CCC[Listener Configs]
    CCC --> CCCC[Example1]
    CCCC --> CCCCA[connection-attributes.json]
    CCCC --> CCCCB[metadata.json]
    CCCC --> CCCCC[topic.json]
    CCC --> CCCD[Example2]
    CCCD --> CCCDA[connection-attributes.json]
    CCCD --> CCCDB[metadata.json]
    CCCD --> CCCDC[topic.json]

    A --> D[Domains]
    D --> DA[Domain 1]
    DA --> DAA[metadata.json]
    DA --> DAB[Saved Queries]
    DAB --> DABA[SQ1]
    DABA --> DABAA[metadata.json]
    DABA --> DABAB[query.sql]
    DAB --> DABB[SQ2]
    DABB --> DABBA[metadata.json]
    DABB --> DABBB[query.sql]

    A --> E[External Secrets Manager]
    E --> EA[Local Secrets]
    EA --> EAA[Secret 1]
    EAA --> EAAA[metadata.json]
    EA --> EAB[Secret 2]
    EAB --> EABA[metadata.json]

    A --> F[Groups]
    F --> FA[G1]
    FA --> FAA[metadata.json]
    F --> FB[G2]
    FB --> FBA[metadata.json]
    F --> FC[G3]
    FC --> FCA[metadata.json]
    
    A --> G[Integrated Clients]
    G --> GA[IG 1]
    GA --> GAA[metadata.json]
    
    A --> H[User Defined Functions]
    H --> HA[UDF 1]
    HA --> HAA[metadata.json]
    HA --> HAB[script.js]
    H --> HB[UDF 2]
    HB --> HBA[metadata.json]
    HB --> HBB[script.js]




  ```
  ## Additional resources

  - [CinchyDXD workflow](/guides-for-using-cinchy/builder-guides/cinchydxd/dxd-workflow.md)
  - [Package the data experience](/guides-for-using-cinchy/builder-guides/cinchydxd/package-the-data-experience.md)