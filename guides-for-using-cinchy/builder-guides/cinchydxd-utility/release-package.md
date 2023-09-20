# Release package

## Overview

This page details the architecture of the release package for CinchyDXD 2.0.

## Architecture

The architecture of a release package in CinchyDXD 2.0 separates into a folder for each entity you exported from the source environment. Each folder has the respective data relevant to that entity.


## Example release package

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
  
  A --> C[Data Experience Definitions]
  C --> D1[Data Definition 1]
  D1 --> E1[manifest.json]
  D1 --> E2[model.xml]
  
  A --> F[Data Sync Configurations]
  F --> G1[Data Sync 1]
  G1 --> H1[manifest.json]
  G1 --> H2[config.xml]
  
  A --> I[Domains]
  I --> J1[Domain 1]
  J1 --> K1[Queries]
  K1 --> L1[metadata.json]
  K1 --> L2[query.sql]
  I --> J2[Domain 2]
  J2 --> K2[Queries]
  K2 --> L3[metadata.json]
  K2 --> L4[query.sql]
  
  A --> IA[External Secrets Manager]
  IA --> IB[metadata.json]
  
  A --> M[Groups]
  M --> N1[Group 1]
  N1 --> O1[manifest.json]
  M --> N2[Group 2]
  N2 --> O2[manifest.json]
  
  A --> P[Integrated Clients]
  P --> Q1[Integrated Client Export]
  Q1 --> R1[manifest.json]
  
  A --> S[System Colours]
  
  A --> T[User Defined Functions]
  T --> U1[UDF 1]
  U1 --> V1[manifest.json]
  U1 --> V2[script.js]

  ```
  ## Additional resources

  - [CinchyDXD workflow](/guides-for-using-cinchy/builder-guides/cinchydxd-utility/dxd-workflow.md)
  - [Package the data experience](/guides-for-using-cinchy/builder-guides/cinchydxd-utility/package-the-data-experience.md)