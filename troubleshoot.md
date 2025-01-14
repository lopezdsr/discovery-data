---

copyright:
  years: 2019, 2021
lastupdated: "2021-03-12"

subcollection: discovery-data

content-type: troubleshoot

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:important: .important}
{:deprecated: .deprecated}
{:codeblock: .codeblock}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Getting help
{: #troubleshoot}

If you cannot find a solution to the issue you are having, try the resources available from the **Developer community** section of the table of contents.

You can also get help by creating a case here: [{{site.data.keyword.cloud_notm}} Support](https://cloud.ibm.com/unifiedsupport/supportcenter){: external}.

## Setting the shard limit in Discovery for Cloud Pak for Data ![Cloud Pak for Data only](images/desktop.png)
{: #shard-limit}
{: troubleshoot}

In {{site.data.keyword.discoveryshort}} version 2.2.0, there is a limit to the number of shards that can stay open on a cluster. In development instances, the limit is 1,000 open shards, and in production instances, the limit is two data nodes, which is equal to 2,000 open shards, or 1,000 open shards per data node. After you reach either limit, you cannot create any more projects and collections on your cluster, and if you try to create a new project and collection, you receive an error message.

This limit is due to the fact that, when you install {{site.data.keyword.discoveryshort}} version 2.2.0, Elasticsearch version 7.8.0 automatically runs on your clusters. Because this version of Elasticsearch runs on your clusters, a new cluster stability configuration becomes available that limits the number of open shards to 1,000 for each Elasticsearch data node.

If you are unable to create new projects and collections and you receive errors, first check the status of your Elasticsearch cluster and the number of shards on that cluster. Consider increasing the number of data nodes on your cluster to support more shards. This method is optimal for maximizing performance. However, an increased number of nodes uses more memory. If the number of shards reaches the limit, you can also increase the limit in a data node. For more information about increasing the shard limit in a node, see [Increasing the shard limit](/docs/discovery-data?topic=discovery-data-troubleshoot#increase-shards).

This limit of 1,000 shards does not apply to versions of {{site.data.keyword.discoveryshort}} that are earlier than 2.2.0.
{: note}

### Increasing the shard limit
{: #increase-shards}

1. Log in to your {{site.data.keyword.discoveryshort}} cluster.
1. Access your data node.
1. Enter the following command:

   ```sh
   oc exec -it $(oc get pod -l app=elastic,ibm-es-data=True -o jsonpath='{.items[0].metadata.name}') -- bash
   ```
   {: pre}

1. Enter the following command, replacing the `<>` and the content inside with your port number:

   ```sh
   curl -X POST http://localhost:<port_number>/_cluster/health?pretty
   ```
   {: pre}

   If you do not know what your port number is, enter the following command to find it:

   ```sh
   oc get pod -l app=elastic,ibm-es-data=True -o json | jq .items[].spec.containers[].ports[0].containerPort | head -n 1`
   ```
   {: pre}

   The curl `POST` command returns a value for `active_primary_shards`. If you have one data node that has a value larger than 1,000 or if you have two data nodes that have a value larger than 2,000, you must increase the shard limit to create new projects and collections in your cluster.

   If you increase this limit, the cluster becomes less stable because it contains an increased number of shards.
   {: important}

1. Enter the following command to increase the number of shards, replacing `<port_number>` with your port number and `<total_shards_per_node>` and `<max_shards_per_node>` with the new shard limit that you want to assign to a node:

   ```sh
   curl -X POST http://localhost:<port_number>/_cluster/settings -d '{
     "persistent": {
       "cluster.routing.allocation.total_shards_per_node":<total_shards_per_node>, "cluster.max_shards_per_node":<max_shards_per_node>
     }
   }' -XPUT -H 'Content-Type:application/json'
   ```
   {: pre}

   After you increase the shard limit, you can create more projects and collections on your cluster.

## Clearing a lock state
{: #troubleshoot-ls}
{: troubleshoot}

![Cloud Pak for Data only](images/desktop.png) **Installed only**: When the `gateway` pod restarts, it runs a database validation plug-in that checks for changes and applies the latest change sets to the shared database. If the pod is restarted while this check is in process, the plug-in might remain in a lock state, preventing the service from starting. Manual database intervention might be needed to clear the lock.

If the Discovery API does not come online or if the `gateway-0` pod looks like it is in a constant crash loop, you can try checking the Liberty server logs for the API service located here: `/opt/ibm/wlp/output/wdapi/logs/messages.log`

The logs would indicate if Liquibase is failing and unable to run. If the system is locked, you might see something similar to the following:

```
[11/7/19 5:07:51:491 UTC] 0000002f liquibase.executor.jvm.JdbcExecutor I SELECT LOCKED FROM public.databasechangeloglock WHERE ID=1
[11/7/19 5:07:51:593 UTC] 0000002f liquibase.lockservice.StandardLockService I Waiting for changelog lock....
[11/7/19 5:08:01:601 UTC] 0000002f liquibase.executor.jvm.JdbcExecutor I SELECT LOCKED FROM public.databasechangeloglock WHERE ID=1
[11/7/19 5:08:02:091 UTC] 0000002f liquibase.lockservice.StandardLockService I Waiting for changelog lock....
[11/7/19 5:08:12:097 UTC] 0000002f liquibase.executor.jvm.JdbcExecutor I SELECT LOCKED FROM public.databasechangeloglock WHERE ID=1
[11/7/19 5:08:12:197 UTC] 0000002f liquibase.lockservice.StandardLockService I Waiting for changelog lock....
[11/7/19 5:08:22:203 UTC] 0000002f liquibase.executor.jvm.JdbcExecutor I SELECT ID,LOCKED,LOCKGRANTED,LOCKEDBY FROM public.databasechangeloglock WHERE ID=1
[11/7/19 5:08:22:613 UTC] 0000002f com.ibm.ws.logging.internal.impl.IncidentImpl I FFDC1015I: An FFDC Incident has been created: "org.jboss.weld.exceptions.DeploymentException: WELD-000049: Unable to invoke public void liquibase.integration.cdi.CDILiquibase.onStartup() on liquibase.integration.cdi.CDILiquibase@7f02a07 com.ibm.ws.container.service.state.internal.ApplicationStateManager 31" at ffdc\_19.11.07\_05.08.22.0.log
```

It is possible to manually unlock the plug-in. If you have {{site.data.keyword.discoveryshort}} 2.1.4 or earlier, enter the following command on the postgres database that the `gateway-0` pod is looking at:

```
psql dadmin
UPDATE DATABASECHANGELOGLOCK SET LOCKED=FALSE, LOCKGRANTED=null, LOCKEDBY=null where ID=1;
```
{: pre}

If you have {{site.data.keyword.discoveryshort}} 2.2.0 or later, enter the following command on the postgres database that the `gateway-0` pod is looking at:

```
oc exec -it wd-discovery-postgres-0 -- bash -c 'env PGPASSWORD="$PG_PASSWORD" psql "postgresql://$PG_USER@$STKEEPER_CLUSTER_NAME-proxy-service:$STKEEPER_PG_PORT/dadmin" -c "UPDATE DATABASECHANGELOGLOCK SET LOCKE
D=FALSE, LOCKGRANTED=null, LOCKEDBY=null where ID=1"'
```
{: pre}

If you can then restart the gateway pod, everything should resume normally.

## Environment variable settings for Smart Document Understanding
{: #troubleshoot-sdu}
{: troubleshoot}

![Cloud Pak for Data only](images/desktop.png) **Installed only**: There are two environment variables that need to be adjusted for Smart Document Understanding in {{site.data.keyword.discoveryfull}} version 2.1.0. This was resolved in version 2.1.1, see [2.1.1 release, 24 Jan 2020](/docs/discovery-data?topic=discovery-data-release-notes#24jan2020).

```
SDU_PYTHON_REST_RESPONSE_TIMEOUT_MS
SDU_YOLO_TIMEOUT_SEC
```
{: pre}

Both of these should be set to their respective `hour` value. These values must be set in the `<release-name>-watson-discovery-hdp` ConfigMap.

The values should be:

`SDU_PYTHON_REST_RESPONSE_TIMEOUT_MS` should be set to `3600000`

`SDU_YOLO_TIMEOUT_SEC` should be set to `3600`

These values should be set when the software is installed or reinstalled.

## Troubleshooting error messages
{: #troubleshoot-errors}

If you receive error messages that are related to timeouts and insufficient memory for your enrichments, you can enter the following commands to change timeout and memory settings to potentially resolve these error messages:

- Notice message: `<enrichment_name>: Document enrichment timed out`
  
  Suggested action: Increase the document processing timeout. Enter the following command to increase the default timeout from 10 to 20 minutes:
  
  ```
  oc patch wd wd --type=merge --patch='{"spec": {"orchestrator": {"docproc": {"defaultTimeoutSeconds": 1200 } } } }'
  ```
  {: pre}

- Notice message: `<enrichment_name>: Document enrichment failed due to lack of memory`
  
  Suggested action: Increase the orchestrator container memory limit. Enter the following command to increase the memory limit from 4 Gi to 6 Gi:
  
  ```
  oc patch wd wd --type=merge --patch='{"spec": {"orchestrator": {"resources": {"limits": {"memory": "6Gi"} } } } }'
  ```
  {: pre}

- Notice messsage: `Indexing request timed out`
  
  Suggested action: Increase the timeout for pushing documents to Elasticsearch. Enter the following command to increase the default timeout from 10 to 20 minutes:

  ```
  oc patch wd wd --type=merge --patch='{"spec": {"shared": {"elastic": {"publishTimeoutSeconds": 1200 } } } }'
  ```
  {: pre}
