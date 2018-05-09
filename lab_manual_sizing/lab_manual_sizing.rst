.. _lab_manual_sizing:

-------------------
Lab - Manual Sizing
-------------------

Overview
++++++++

Walkthrough a sizing exercise where you manually manipulate a rvtools export, and manually configure the Nutanix nodes.

There are times when doing a sizing for a prospect or customer is more complicated. They may have multiple locations with multiple vCenters, and there buy multiple RVTools exports. This can require you to manually manipulate the data to get the information you need.

In this lab we will use a singe RVTools that contains multiple sites and clusters. The prospect is looking to move to Nutanix, and do a datacenter consolidation.

Getting Sizing Specs from RVTool
++++++++++++++++++++++++++++++++

When you look at an RVTools export, there are few Tabs that have the information you want.

- **tabvHost** - This tab gives you information about the current hosts, number of clusters contained in the rvtools, number of VMs, and number of vCPUs.
- **tabvCPU** - This tab gives you a breakdown of the vCPUs assigned to each VM.
- **tabvMemory** - This tab gives a breakdown of memory assignment, and usage for all of the VMs.
- **tabvPartition** - This tab gives you a breakdown of assigned storage, and free|unused storage.

Calculating vCPU|Core Ratio
...........................

Lets use the **tabvHost** to calculate the number of Cores and vCPUs for each location, as well as the total of the two.

.. figure:: images/manual_sizing_01.png

We can see the following information for each location:

- **Beaverton - Total Cores** - 118
- **Beaverton - Total vCPUs** - 225
- **Beaverton vCPU|Core Ratio** - 1.91|1

- **Wilsonville - Total Cores** - 36
- **Wilsonville - Total vCPUs** - 131
- **Wilsonville vCPU|Core Ratio** - 3.64|1

- **Total Cores** - 154
- **Total vCPUs** - 356
- **Total vCPU|Core Ratio** - 2.3|1

So looking at the two locations individually and together we could safely go with a 3|1 ratio. Taking into consideration the age of the existing server hardware and CPUs, we could also be to choose 4|1.

.. note:: We used the *=sum(cell:cell)* and *=cell/cell* formulas to get these numbers

Calculating vCPU per Cluster
............................

Lets use the **tabvCPU** to calculate the number of vCPUs for each cluster.

.. figure:: images/manual_sizing_02.png

We can see the following information for each cluster:

- **Total vCPUs Cluster - Beaverton** - 182
- **Total vCPUs Cluster - Beaverton-2** - 52
- **Total vCPUs Cluster - Wilsonville** - 155
- **Total vCPUs Cluster - MGMT** - 8

.. note:: We used the *=sum(cell:cell)* formula to get these numbers

Calculating Memory
..................

Lets use the **tabvMemory** to calculate the memory for each cluster.

.. figure:: images/manual_sizing_03.png

We can see the following information for each cluster:

- **Total Assigned Memory per Cluster - Beaverton** - 567gig
- **Total Assigned Memory per Cluster - Beaverton-2** - 317gig
- **Total Assigned Memory per Cluster - Wilsonville** - 513gig
- **Total Assigned Memory per Cluster - MGMT** - 16gig

.. note:: We used the *=sum(cell:cell)* formula to get these numbers

Calculating Storage
...................

Lets use the **tabvPartition** to calculate the storage for each cluster.

.. figure:: images/manual_sizing_04.png

We can see the following information for each cluster:

- **Total Storage per Cluster - Beaverton** - 27tb
- **Total Storage per Cluster - Beaverton-2** - 3tb
- **Total Storage per Cluster - Wilsonville** - 14tb
- **Total Storage per Cluster - MGMT** - 280gig

.. note:: We used the *=sum(cell:cell)* formula to get these numbers

Manual Sizing
+++++++++++++

Now lets take the information we calculated above, and do our sizing.

Create the Scenario
...................

Open http://sizer.nutanix.com

Login with your **My Nutanix Login** credentials.

Click **+ Create New Scenario**, and create a demo Scenario.

Enter the following information, and click **Create**:

- **Scenario Name** - Manual Sizing Lab
- **Vendor Model** - Nutanix Models
- **Scenario Objectives**:
- **Executive Summary** - Data Center Refresh and Consolidation.
- **Requirements** - All Flash, General Server Virt. vCPU 356 / Mem 1,412gig / Storage 45tb / 3|1 vCPU|Core ratio.
- **Constraints** - Any information you have regarding Constraints.
- **Assumptions** - Any information you have regarding Assumptions.
- **Risks** - Any information you have regarding Risks.
- **Description** - Consolidating a Datacenter and a Colo into On-Prem Data Center.

.. figure:: images/manual_sizing_05.png

Add Workloads
.............

To highlight the current vSphere clusters and locations, we will treat the information from each cluster as a workload.

Added the Beaverton Cluster:

Click **+ Add Workload**.

Enter the following information, and click **Next**:

- **Workload Name** - Beaverton
- **Workload Type** - RAW Input

Fill out the following fields and click **Save**:

- **vCPUs** - 182
- **vCPU:pCore ratio** - 3
- **RAM** - 567
- **HDD Storage** - 0
- **SSD Storage** - 27
- **Container Replication Factor** - RF2
- **Disable Compression for Pre-Compressed Data** - No
- **Erasure Coding** - Unchecked
- **Block Awareness** - Unchecked
- **Encrypt Storage for VM** - Unchecked
- **Data Protection** - No

Added the Beaverton-2 Cluster:

Click **+ Add Workload**.

Enter the following information, and click **Next**:

- **Workload Name** - Beaverton-2
- **Workload Type** - RAW Input

Fill out the following fields and click **Save**:

- **vCPUs** - 52
- **vCPU:pCore ratio** - 3
- **RAM** - 317
- **HDD Storage** - 0
- **SSD Storage** - 3
- **Container Replication Factor** - RF2
- **Disable Compression for Pre-Compressed Data** - No
- **Erasure Coding** - Unchecked
- **Block Awareness** - Unchecked
- **Encrypt Storage for VM** - Unchecked
- **Data Protection** - No

Added the Wilsonville Cluster:

Click **+ Add Workload**.

Enter the following information, and click **Next**:

- **Workload Name** - Wilsonville
- **Workload Type** - RAW Input

Fill out the following fields and click **Save**:

- **vCPUs** - 52
- **vCPU:pCore ratio** - 3
- **RAM** - 513
- **HDD Storage** - 0
- **SSD Storage** - 14
- **Container Replication Factor** - RF2
- **Disable Compression for Pre-Compressed Data** - No
- **Erasure Coding** - Unchecked
- **Block Awareness** - Unchecked
- **Encrypt Storage for VM** - Unchecked
- **Data Protection** - No

Added the MGMT Cluster:

Click **+ Add Workload**.

Enter the following information, and click **Next**:

- **Workload Name** - MGMT
- **Workload Type** - RAW Input

Fill out the following fields and click **Save**:

- **vCPUs** - 8
- **vCPU:pCore ratio** - 3
- **RAM** - 16
- **HDD Storage** - 0
- **SSD Storage** - 1
- **Container Replication Factor** - RF2
- **Disable Compression for Pre-Compressed Data** - No
- **Erasure Coding** - Unchecked
- **Block Awareness** - Unchecked
- **Encrypt Storage for VM** - Unchecked
- **Data Protection** - No

We now have all of our workloads added.

Review/Modify Recommended Nodes
...............................





Reviewing The Manual Sizing
+++++++++++++++++++++++++++

Takeaways
+++++++++
