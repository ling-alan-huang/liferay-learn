# Upgrading Search for Liferay 7.3

After upgrading Liferay, there are additional steps to upgrade the search experience. The exact steps depend on your existing search engine installation and Liferay version, but you'll always want to start by [backing up your existing indexes](./backing-up-elasticsearch.md).

```important::
   Always consult Liferay's `Breaking Changes <../../../../liferay-internals/reference/7-3-breaking-changes.md>`__ before upgrading. One such breaking change that impacts the upgraded search experience: `Dynamic Data Mapping fields in Elasticsearch have changed to a nested document <../../../../liferay-internals/reference/7-3-breaking-changes.md#dynamic-data-mapping-fields-in-elasticsearch-have-changed-to-a-nested-document>`__. 

   This change affects custom code that executes queries in the Elasticsearch index using ``ddm__keyword__*`` and ``ddm__text__*`` fields, as well as Search Widget configurations (e.g., Custom Filter or Custom Facet widgets) that make use of those fields. Manual updates to code and configurations are required to account for the change.
```

## Search Upgrade Overview

The list below just scratches the surface of the search upgrade picture: it doesn't cover more complicated scenarios (such as upgrading Liferay Enterprise Search modules). Find the scenario that matches your Liferay version, LES version (if using LES), and your current search engine stack. Use the *Upgrade Steps* column to guide the upgrade.

| Upgrading from Liferay Version [+ LES Version] | Upgrading From Search Engine Version | Upgrade Steps |
| :-------- | :---------------- | :-------------- |
| **Liferay 7.2** | Elasticsearch 7.9+ | 1. [Connect Liferay to Elasticsearch 7.](../connecting-to-elasticsearch.md)<br><br>2. [Configure security.](../securing-elasticsearch.md)<br><br>3. [Upgrade Liferay.](../../../../installation-and-upgrades/upgrading-liferay/upgrade-basics/upgrade-overview.md)<br><br>4. [Re-index search & spell check indexes.](../../../../installation-and-upgrades/upgrading-liferay/upgrade-basics/post-upgrade-considerations.md) |
| **Liferay 7.2 + LES 3.0** (*Monitoring*, *Learning to Rank*) | Elasticsearch 7.9+ | 1. Connect Liferay to Elasticsearch 7.9+.<br><br>2. Configure security.<br><br>3. Install Kibana 7.9+ if you are currently using *Kibana and Monitoring*.<br><br>4. Install and configure LES Monitoring if you are currently using Kibana and *Elasticsearch Monitoring/X-Pack Monitoring*.<br><br>5. [Upgrade Liferay.](../../../../installation-and-upgrades/upgrading-liferay/upgrade-basics/upgrade-overview.md)<br><br>6. [Re-index search & spell check indexes.](../../../../installation-and-upgrades/upgrading-liferay/upgrade-basics/post-upgrade-considerations.md) |
| **Liferay 7.2** | Elasticsearch 7.3.x-7.8.x | 1. [Upgrade to Elasticsearch 7.9+.](upgrading-to-elasticsearch-7.md)<br><br>2. Follow [Common Upgrade Steps](#common-upgrade-steps) |
| **Liferay 7.2 + LES 2.0** (*Monitoring*, *Learning to Rank*) | Elasticsearch 7.3.x-7.8.x | 1. [Upgrade to Elasticsearch 7.9+.](upgrading-to-elasticsearch-7.md)<br><br>2. Follow [LES Upgrade Steps](#les-upgrade-steps) |
| **Liferay 7.2** | Elasticsearch 6.x | 1. [Upgrade to Elasticsearch 7.9+.](upgrading-to-elasticsearch-7.md)<br><br>2. Follow [Common Upgrade Steps](#common-upgrade-steps) |
| **Liferay 7.2 + LES 2.0** (*Security*, *Monitoring*, *Learning to Rank*) | Elasticsearch 6.x | 1. [Upgrade to Elasticsearch 7.9+.](upgrading-to-elasticsearch-7.md)<br><br>2. Follow [LES Upgrade Steps](#les-upgrade-steps) |
| **Liferay 7.1** | Elasticsearch 6.x | 1. [Upgrade to Elasticsearch 7.9+.](upgrading-to-elasticsearch-7.md)<br><br>2. Follow [Common Upgrade Steps](#common-upgrade-steps) |
| **Liferay 7.1 + LES 2.0** (*Security*, *Monitoring*, *Learning to Rank*) | Elasticsearch 6.x | 1. [Upgrade to Elasticsearch 7.9+.](upgrading-to-elasticsearch-7.md)<br><br>2. Follow [LES Upgrade Steps](#les-upgrade-steps) |
| **Liferay 7.0** | Elasticsearch 6.x | 1. [Upgrade to Elasticsearch 7.9+.](upgrading-to-elasticsearch-7.md)<br><br>2. Follow [Common Upgrade Steps](#common-upgrade-steps) |
| **Liferay 7.0** | Elasticsearch 2.x | 1. [Install Elasticsearch 7.9+.](../installing-elasticsearch.md)<br><br>2. Follow [Common Upgrade Steps](#common-upgrade-steps) |
| **Liferay 7.2, 7.1** | Solr 7.x | **Switch to Elasticsearch:**<br><br>1. [Install Elasticsearch 7.9+.](../installing-elasticsearch.md)<br><br>2. Follow [Common Upgrade Steps](#common-upgrade-steps) for configuring Liferay 7.3 with Elasticsearch<br> or follow [LES Upgrade Steps](#les-upgrade-steps) for Liferay 7.3 + Liferay Enterprise Search (LES) 3.0<br><br>**or**<br><br><br>**Upgrade Solr (deprecated):**<br><br>1. [Set up Solr 8.x.](../../solr.rst)<br><br>2. [Upgrade Liferay.](../../../../installation-and-upgrades/upgrading-liferay/upgrade-basics/upgrade-overview.md)<br><br>3. [Re-index search & spell check indexes.](../../../../installation-and-upgrades/upgrading-liferay/upgrade-basics/post-upgrade-considerations.md) |
| **Liferay 7.0** | Solr 5.x | Use steps for Solr 7.x above. |

## Common Upgrade Steps

```important::
   `Back up the search indexes <./backing-up-elasticsearch.md>`__ before proceeding with these steps.
```

Upgrade scenarios for systems not including LES apps include these steps:

1. Move to Elasticsearch 7.9+ by [upgrading to Elasticsearch 7.9+](upgrading-to-elasticsearch-7.md) or, if you're coming form a Solr installation or Elasticsearch 2.x, [install Elasticsearch 7.9+](../installing-elasticsearch.md) from scratch.

1. [Connect Liferay to Elasticsearch 7.9+](../connecting-to-elasticsearch.md)

1. [Configure security.](../securing-elasticsearch.md)

1. [Upgrade Liferay.](../../../../installation-and-upgrades/upgrading-liferay/upgrade-basics/upgrade-overview.md)

1. [Re-index the search and spell check indexes.](../../../../installation-and-upgrades/upgrading-liferay/upgrade-basics/post-upgrade-considerations.md)

1. [Re-index the Workflow Metrics indexes.](../../../../process-automation/workflow/using-workflows/workflow-metrics-reports.md#re-indexing-workflow-metrics)

1. Test the search experience in the upgraded system to ensure everything is working as expected.

## LES Upgrade Steps

```important::
   `Back up the search indexes <./backing-up-elasticsearch.md>`__ before proceeding with these steps.
```

Systems using LES apps must follow these additional steps:

1. Install Kibana 7.9+ if you are currently using Kibana and Monitoring.

1. Install and configure LES Monitoring app if you are currently using Kibana and Elasticsearch Monitoring/X-Pack Monitoring.

1. Install LES Learning to Rank app if you are currently using this in your environment.


## Test the Upgraded Search Experience

Manually test the upgraded search experience to ensure the features you depend on work as expected. If something is not working or is behaving differently than you expect, review [Liferay's Breaking Changes](./../../../../liferay-internals/reference/7-3-breaking-changes.md).

## LES Applications Renamed

> **LES Subscribers**

Though not explicitly linked to the Liferay CE/DXP 7.3 release, these apps were renamed to better reflect their functionality and to emphasize their identity as LES apps:

| Functionality | Old App Name | New App Name | 7.2 Configuration File | 7.3 Configuration File |
| ------------- | ------------ | ------------ |------------ | ------------ |
| Monitoring the Elasticsearch cluster | Liferay Connector to X-Pack Monitoring [Elastic Stack 6.x] | Liferay Enterprise Search Monitoring | `com.liferay.portal.search.elasticsearch6.xpack.monitoring.web.internal.configuration.XPackMonitoringConfiguration.config` | `com.liferay.portal.search.elasticsearch.monitoring.web.internal.configuration.MonitoringConfiguration.config` |
| Securing the Elasticsearch cluster | Liferay Connector to X-Pack Security [Elastic Stack 6.x] | Liferay Enterprise Search Security | No action required, this app is not available for DXP 7.3, features are integrated into the Elasticsearch 7 connector bundled with DXP 7.3. |
| Using machine learning to optimize the search algorithm | Liferay Connector to Elasticsearch Learning to Rank | Liferay Enterprise Search Learning to Rank | No changes. |

For Liferay 7.2 widget and configuration names were unchanged. In Liferay 7.3 the monitoring widget and the configurations were renamed.

The renaming of apps and configurations has the following upgrade impacts:
1. The LES Monitoring widget is now named *Elasticsearch Monitoring*. During startup, a module upgrade step runs, renaming the app when _Liferay Enterprise Search Monitoring_ for DXP 7.3 is deployed. No action is required.
1. The configuration file name changed from `com.liferay.portal.search.elasticsearch6.xpack.monitoring.web.internal.configuration.XPackMonitoringConfiguration.config` to `com.liferay.portal.search.elasticsearch.monitoring.web.internal.configuration.MonitoringConfiguration`. The properties are the same as before. During portal startup, a module upgrade step runs, renaming the configuration. No action is required.
1. The Kibana base path to the monitoring widget has changed. You must change the original setting in `kibana.yml`:

   ```yaml
   server.basePath: "/o/portal-search-elasticsearch-xpack-monitoring/xpack-monitoring-proxy"
   ```

   to

   ```yaml
   server.basePath: "/o/portal-search-elasticsearch-monitoring/monitoring-proxy"
   ```

Liferay 7.3 supports only Elasticsearch 7.9+ via the out-of-the-box Liferay Connector to Elasticsearch 7. The matrix of pre-upgrade stacks you are migrating from is more numerous, and it's important to understand the high-level steps required to navigate safely from your existing stack to the Liferay 7.3 stack. 

## What's Next 

Now that you know your upgrade path, start upgrading to use Liferay 7.3 with the latest [Elasticsearch](./upgrading-to-elasticsearch-7.md) (recommended) or [Solr](../../solr.html) (now deprecated as of Liferay 7.3) search engine.

## Additional Information

* [Upgrading Elasticsearch](../getting-started-with-elasticsearch.md)
* [Getting Started with Elasticsearch](../getting-started-with-elasticsearch.md)
* [Installing Elasticsearch](../installing-elasticsearch.md)
* [Connecting to Elasticsearch](../connecting-to-elasticsearch.md)
* [Securing Elasticsearch](../securing-elasticsearch.md)
* [Upgrading Liferay](../../../../installation-and-upgrades/upgrading-liferay/upgrade-basics/upgrade-overview.md)
