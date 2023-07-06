# Upgrading from v4 to v5

## Table of Contents

| Table of Contents                                                                                                                              |
| ---------------------------------------------------------------------------------------------------------------------------------------------- |
| [#1.-upgrading-to-v5-on-kubernetes](upgrading-from-v4-to-v5.md#1.-upgrading-to-v5-on-kubernetes "mention")                                     |
| [#2.-upgrading-to-v5-on-iis](upgrading-from-v4-to-v5.md#2.-upgrading-to-v5-on-iis "mention")                                                   |
| [#3.-compatibility-issues-between-databases-versions](upgrading-from-v4-to-v5.md#3.-compatibility-issues-between-databases-versions "mention") |

## 1. Upgrading to v5 on Kubernetes

When you upgrade to Cinchy v5 on Kubernetes you are creating a separate instance. You will need to plan the migration of your database, and then follow the [deployment installation guide.](../deployment-installation-guides/)

## 2. Upgrading to v5 on IIS

To upgrade from v4+ to v5+ on IIS, [review the documentation here.](upgrading-cinchy-versions/#3.-upgrading-on-iis-v4-to-v5+)

## 3. Compatibility Issues Between Databases/Versions

If you choose to upgrade to Cinchy v5 on PostgreSQL, please review the following features that aren't currently available in that deployment.

These may be implemented in future versions of the platform.

* No Partitioning
* No Geospatial
* No Column Indexing
* No Full Text Indexing
* No Column Level Encryption

You may also wish to review the list of **CQL functions currently unsupported in PGSQL.** Please note that this is a living document and that the development team is actively working on function translations between databases. Make sure to check back for the most up to date information.

* [CQL Functions](https://app.gitbook.com/o/-LDtM6UlhGoQ91uwM5SF/s/-MBtHkNqYteSDPDzpqqZ/\~/changes/Vg2sE80mlPeAJNYXHm3i/the-basics-of-cql/cql-functions-master-list)
