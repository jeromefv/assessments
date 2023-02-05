# Chapter 50. Tracking Replication Progress

Replication origins make it easier to use logical replication solutions on top of logical decoding. They help solve the two common situations below.

- Tracking replication progress
- Changing replication behavior based on row origin
    - For example, setting up bi-directional replication to prevent loops

## Name and ID properties

Replication origins have two properties, a name and an ID. The name is free-form text and refers to the origin across systems. Use unique names to identify different replication solutions. The ID is an alternative value for space saving situations.

> NOTE: ID must not be shared across systems.

## Managing replication origins

The functions in the table below are actions for replication origins. View the replication origins in the system catalog with `pg_replication_origin`.

| Action | Code |
| --- | --- |
| Create | `pg_replication_origin_create()` |
| Drop | `pg_replication_origin_drop()` |

## Strategies

Track replay progress when building replication solutions to avoid common issues. For example, confirming the progress and success of your data replication is helpful for when the applying process, or the whole cluster, dies. 

In comparison, run-time overhead and database bloat may happen with other solutions, such as when repeatedly updating a row in a table.

Using the replication origin infrastructure with the `pg_replication_origin_session_setup()` function in a remote node, you can mark a session as replaying. Also, the `pg_replication_origin_xact_setup()` function can configure the LSN and commit time stamp for each source transaction. This configuration is safe for crashes during the replication progress. 

The `pg_replication_origin_status` view shows replay progress for all replication origins. Retrieve an individual origin's progress, such as when resuming replication, with the functions below.

| Action | Code |
| --- | --- |
| Retrieve progress from any origin | `pg_replication_origin_progress()` |
| Retrieve progress for the origin in the current session | `pg_replication_origin_session_progress()` |

Replication origins avoid issues such replicating replayed rows again, cycles in the replication, and other inefficiencies This is helpful for complex replication topologies with multiple systems migrating. Configuring your replication using the functions above allows you to tag output plugin callbacks with more information. This includes the replication origin of the generating session for every change and transaction.

This lets you filter rows by origin in the output plugin. The `filter_by_origin_cb` callback also filters the logical decoding change stream based on the source. Using the callback for filtering prioritizies efficiency over flexibility when compared to using the output plugin.
