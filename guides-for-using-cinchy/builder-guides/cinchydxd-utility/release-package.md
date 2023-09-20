# Release package

## Overview

This page details the architecture of the release package for CinchyDXD 2.0.

## Architecture

The architecture of a release package in CinchyDXD 2.0 separates into a folder for each entity you exported from the source environment. Each folder has the respective data relevant to that entity.


## CinchyDXD 2.0 example release package

The following list is an example of the release package folder architecture and their respective files. 

ExampleRepo
- manifest.json
- Data Experience Definitions
  - Data Definition 1
    - manifest.json
    - model.xml
- Data Sync Configurations
  - Data Sync 1
    - config.xml
    - manifest.json
- Domains
  - Domain 1
    - Queries
      - metadata.json
      - query.sql
  - Domain 2
    - Queries
      - metadata.json
      - query.sql 
- External Secrets Manager
    - metadata.json
- Groups
  - Group 1
    - manifest.json
  - Group 1
    - manifest.json
- Integrated Clients
  - Integrated Client Export
    - manifest.json
- System Colours
- User Defined Functions
  - UDF 1
    - manifest.json
    - script.js

## Visual representation

```mermaid
graph LR
  A[ExampleRepo] --> B[manifest.json]
  style B fill:#FFFF00,stroke:#000000,stroke-width:2px;

  A --> C[Data Experience Definitions]
  C --> D1[Data Definition 1]
  D1 --> E1[manifest.json]
  style E1 fill:#FFFF00,stroke:#000000,stroke-width:2px;
  D1 --> E2[model.xml]
  style E2 fill:#FFA500,stroke:#000000,stroke-width:2px;

  A --> F[Data Sync Configurations]
  F --> G1[Data Sync 1]
  G1 --> H1[manifest.json]
  style H1 fill:#FFFF00,stroke:#000000,stroke-width:2px;
  G1 --> H2[config.xml]
  style H2 fill:#FFA500,stroke:#000000,stroke-width:2px;

  A --> I[Domains]
  I --> J1[Domain 1]
  J1 --> K1[Queries]
  K1 --> L1[metadata.json]
  style L1 fill:#FFFF00,stroke:#000000,stroke-width:2px;
  K1 --> L2[query.sql]
  style L2 fill:#00FFFF,stroke:#000000,stroke-width:2px;
  I --> J2[Domain 2]
  J2 --> K2[Queries]
  K2 --> L3[metadata.json]
  style L3 fill:#FFFF00,stroke:#000000,stroke-width:2px;
  K2 --> L4[query.sql]
  style L4 fill:#00FFFF,stroke:#000000,stroke-width:2px;

  A --> IA[External Secrets Manager]
  IA --> IB[metadata.json]
  style IB fill:#FFFF00,stroke:#000000,stroke-width:2px;

  A --> M[Groups]
  M --> N1[Group 1]
  N1 --> O1[manifest.json]
  style O1 fill:#FFFF00,stroke:#000000,stroke-width:2px;
  M --> N2[Group 2]
  N2 --> O2[manifest.json]
  style O2 fill:#FFFF00,stroke:#000000,stroke-width:2px;

  A --> P[Integrated Clients]
  P --> Q1[Integrated Client Export]
  Q1 --> R1[manifest.json]
  style R1 fill:#FFFF00,stroke:#000000,stroke-width:2px;

  A --> S[System Colours]

  A --> T[User Defined Functions]
  T --> U1[UDF 1]
  U1 --> V1[manifest.json]
  style V1 fill:#FFFF00,stroke:#000000,stroke-width:2px;
  U1 --> V2[script.js]
  style V2 fill:#008000,stroke:#FFFFFF,stroke-width:2px;


  ```
  ## Additional resources

  - [CinchyDXD workflow](/guides-for-using-cinchy/builder-guides/cinchydxd-utility/dxd-workflow.md)
  - [Package the data experience](/guides-for-using-cinchy/builder-guides/cinchydxd-utility/package-the-data-experience.md)