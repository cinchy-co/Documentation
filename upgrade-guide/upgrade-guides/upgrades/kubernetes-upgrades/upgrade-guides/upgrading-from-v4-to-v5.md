# Upgrading from v4 to v5


## Upgrading to v5 on Kubernetes

When you upgrade to Cinchy v5 on Kubernetes you are creating a separate instance. You will need to plan the migration of your database, and then follow the [deployment installation guide.](../deployment-guides/)

## Upgrading to v5 on IIS

To upgrade from v4+ to v5+ on IIS, [review the documentation here.](upgrades/#3.-upgrading-on-iis-v4-to-v5+)

## Compatibility issues between databases or versions

If you choose to upgrade to Cinchy v5 on PostgreSQL, please review the following features that aren't currently available in that deployment.

These may be implemented in future versions of the platform.

* No Partitioning
* No Geospatial features
* No Column Indexing
* No Full Text Indexing
* No Column Level Encryption

You may also wish to review the list of **CQL functions currently unsupported in PGSQL.** Please note that this is a living document and that the development team is actively working on function translations between databases. Make sure to check back for the most up to date information.

* [CQL Functions](https://app.gitbook.com/o/-LDtM6UlhGoQ91uwM5SF/s/-MBtHkNqYteSDPDzpqqZ/\~/changes/Vg2sE80mlPeAJNYXHm3i/the-basics-of-cql/cql-functions-master-list)
