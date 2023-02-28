---
title: "Getting Started: Monitoring"
---

## Getting Started: Monitoring
Our monitoring service can be accessed at [zabbix.pleiades.uni-wuppertal.de](https://zabbix.pleiades.uni-wuppertal.de/) with the credentials "pleiades" and "pleiades".
Zabbix collects various information regarding current and past resource usage and quotas.

### Dashboard
After login you are typically greeted by the user overview dashboard:
[![Dashboard overview](../assets/img/zabbix/user_dashboard.jpg)](../assets/img/zabbix/user_dashboard.jpg)

There are also multiple sub-pages available, covering an overview and login-node-specific metrics:
[![Dashboard overview](../assets/img/zabbix/dashboard_pages.jpg)](../assets/img/zabbix/dashboard_pages.jpg)

The overview dashboard presents aggregated metrics and you can select the time frame of presented information on the top right.
The visible widgets are:
* Number of free (idle) worker or gpu nodes
* Current time
* **SLURM Allocated Cores by Group** (2 plots): Current allocation of Cores per group as reported by Slurm
* **Effective Share used by User Group**: Slurm CPU resource usage for each group (resets each month)
* **Effective Spare of GPU resources by User Group**: Slurm GPU resource usage for each group (resets each month)
* **Beegfs Group Quota** (% and TB): Information on how much space is available on BeeGFS per group

More detailed pages about each class of login nodes provide information about:
* **Number of logged in users**
* **CPU usage**
* **Memory usage**

You can change the displayed dashboard through `Monitoring > Dashboard > All dashboards > user overview`:

[![Dashboard overview](../assets/img/zabbix/dashboard.jpg)](../assets/img/zabbix/dashboard.jpg)

All available user dashboards are:
[![Dashboard overview](../assets/img/zabbix/available_dashboards.jpg)](../assets/img/zabbix/available_dashboards.jpg)


### GPU Dashboard
Another dashboard shows detailed information of all five gpu nodes, `gpu2100[1-5]`:
[![Dashboard overview](../assets/img/zabbix/gpu_dashboard.jpg)](../assets/img/zabbix/gpu_dashboard.jpg)

You can select separate pages for each gpu node, which provide detailed information:
[![Dashboard overview](../assets/img/zabbix/gpu_dashboard_detail.jpg)](../assets/img/zabbix/gpu_dashboard_detail.jpg)

These pages can help you answer questions like:
* How much memory is available on a specific GPU? (8 GPUs per node)
* How good is the utilization of a specific GPU?
* How busy is the node CPU or memory?
* How busy is the nodes network interface?
* How much power does the GPU consume?

If you know which GPUs your job is using, or if you use a whole node exclusively, this approach can help to assess your software performance.


### Detailed Information
To get detailed information about a single node, e.g. to see if a node you are currently working on is using its resources, you can go to the `Monitoring > Graphs` tab.

First you need to select a group of hosts, e.g. `Worker Nodes`:
[![Select monitoring group](../assets/img/zabbix/graph1.jpg)](../assets/img/zabbix/graph1.jpg)

Within this group, you can select a specific host, e.g. `wn21001` or `gpu21001`:
[![Select host in group](../assets/img/zabbix/graph2.jpg)](../assets/img/zabbix/graph2.jpg)

The `Graph` drop down menu provides a range of metrics to investigate, e.g. `CPU` or `Memory utilization`.
A graph in the bottom represents the data for this particular host with the usual options to specify a time frame:
[![Graphs for gpu21001](../assets/img/zabbix/graph3.jpg)](../assets/img/zabbix/graph3.jpg)
