# DP-900: Microsoft Azure Data Fundamentals

## 1. Describe Core DAta Concepts

### 1.1 Azure Data Fundamentals: Data Workloads 

#### 1.1.1 Workload Management

- A data solution typically must meet certain Service Level Agreements or SLAs.
-  a complete data workload process which consists of one or all of the following. Loading the data into the system, transforming the data into a suitable state and format, and querying the data.
-  A database is an instance, like SQL Server, that typically stores one type of data. A data warehouse, on the other hand, typically stores data from many sources and allows that data to be combined in complex ways for reporting. It may include a database such as SQL Server, but also supporting services.
-  When we talk about a data warehouse workload then it includes all of the following:
    * **Process of loading data:** In the case of a data warehouse using SQL Server, this may be an external process that loads data directly into SQL Server or into another process or processes that are in between.
    * **Performing analysis and reporting:** So, this is all the servicing done to allow users to query the data and also generate reports.
    * **Managing the data:** In a distributed cloud system this may involve moving or replicating the data between regions, for instance.
    * **Exporting the data:** For example, to archive data in a data lake or to move the data to another repository.

-  if a data warehouse is managing multiple workloads, how can you control workload resources? Well, as an example, we can look at Azure's warehousing solution, Synapse Analytics. Synapse Analytics' querying language, Synapse SQL, allows you to assign resource classes. This assigns memory resource to the query based on a role membership associated to the user making the query.
- A more advanced form of controlling workload resources came with Dedicated SQL Pools. With Dedicated SQL Pools, workloads are assigned resources using three concepts:
1. **Workload classification**, which assigns a workload to a specific workload group. **Workload classification** also involves setting the...
2. **Workload importance**, which controls whether the workload gets resources before other workloads.
3. **Workload isolation**, which is a way to guarantee dedicated resources for critical workloads.

- As mentioned, dedicated SQL pools allow for workload isolation, depending on the workload group that a request comes in on. With workload isolation, resources are held exclusively and can't be used by any other workload until the execution is complete. This guarantees execution performance for critical workloads that have to complete under a strict SLA. With workload isolation, you can define resource amounts per request.

Another thing you can do is not only reserve resources for a workload, but also cap what resources they do use to prevent a workload from starving other workloads. This gives you a high degree of control over the resources used per workload. And finally, workload isolation can be used to apply other rules related to workflows such as query timeouts, allowing yet another layer of workflow control.

#### 1.1.2 Data Solutions



#### 1.1.3 Data Storage




#### 1.1.4 Transactional Workloads



#### 1.1.5 Analytical Workloads



#### 







### 1.2 Azure Data Fundamentals: Data Analytics



## 2. Identify considerations for relational data on Azure

### 2.1 Azure Data Fundamentals: Relational Data Workloads

### 2.2 Azure Data Fundamentals: Relational Data Management

### 2.3 Azure Data Fundamentals: Provisioning & Configuring Relational Data Services

### 2.4 Azure Data Fundamentals: Azure SQL Querying Techniques


## 3. Describe considerations for working with non-relational data on Azure

### 3.1 Azure Data Fundamentals: Non-Relational Data Workloads

### 3.2 Azure Data Fundamentals: Non-Relational Data Services

### 3.3 Azure Data Fundamentals: Azure Cosmos DB

### 3.4 Azure Data Fundamentals: Non-relational Data Management




