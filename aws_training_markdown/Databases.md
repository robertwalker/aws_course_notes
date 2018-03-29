Relation Database Managment Systems (RDBMS)
-------------------------------------------

### DB Vendors Supported by AWS

* MS SQL Server
* Oracle
* MySQL
* PostgreSQL
* Amazon Aurora
* MariaDB

Exam will cover RDBMS in general and may have some Aurora specific questions

### Non Relational Databases (No-SQL)

* Database
  * Collection = Table
  * Document = Row
  * Key Value Pairs = Fields

Amazon's No-SQL database is DynamoDB. Uses JSON structure for data.

Data Warehousing
----------------

Used for business intelligence and working with very large and complex data sets.

### Tools Examples

* Cognos
* Jaspersoft
* SQL Server Reporting Services
* Oracle Hyperion
* SAP NetWeaver

### OLTP vs OLAP

* Online Transaction Processing (OLTP)
* Online Analytics Processing (OLAP)

OLTP and OLAP differ in terms of the types of queries you run.

### OLTP Example

Order Number: 2120121

Fetches a row of data such as Name, Date, Delivery Address, Delivery Status, etc.

### OLAP Example

Net profit for EMEA and Pacific for the Digital Radio Product.

Fetches large numbers of records to support queries such as:

* Sum of radios sold in EMEA
* Sum of radios sold in Pacific
* Unit cost of radio in each region
* Sales proice of each radio
* Sales price -- unit cost

Used for data warehousing. Usually stored in a separate database from the one used for production applications. OLAP systems are designed to support very complex data queries as referenced above.

Data Warehousing databases use a different type of architecture both from a database perspective and infrastructure layer.

Elasticache
-----------

Web service used to improve performance by storing very frequently accessed data in-memory (RAM) key value store.

### Caching Options

* Memcached
* Redis

Summary
-------

* RDS - OLTP
  * MS SQL
  * MySQL
  * PostgreSQL
  * Oracle
  * Aurora
  * MariaDB
* DynamoDB - No-SQL
* RedShift - OLAP
* Elasticache - In-memory high performance key/value cache storage

Automated Backups
-----------------

### Automated Backups

Automated backup are stored in S3. Provide free storage equal to the size of your database.

Automated backups are deleted when RDS instance is deleted.

### Database Snapshots

User intitiated manual database backups. Not deleted when RDS instance is deleted.

Instance I/O is suspended for the duration of the snapshot defined window for backup.

### Restoring Backups

Restored backups from either automated or manual snapshots are restored to a new RDS instnce with a new DNS endpoint.

### Encryption

* Now supported for all RDS instance types (Aurora, MySQL, Oracle, MS SQL, etc.)
* Leverages AWS Key Management (KMS)
* Automated backups, snapshots, and read replicas are encrypted
* Encrypting existing databases is not yet supported. Must first create a copy.

Multi-AZ
--------

Used for disaster recovery only (database failover). Exact copy of production database. Multi-AZ is on by default for Aurora, must be manually confgured for others.

Read Replicas
-------------

Read replicas are used for improved read performance and scaling.

### Supported Engines

* MySQL
* PostgreSQL
* MariaDB
* Aurora

### Details

* Used for scaling, not for disaster recovery
* Must have automated backups enabled
* Can have up to 5 replicas
* Can have read replicas of read replicas (watch out for latency)
* Esch read replica will have its own DNS endpoint
* You can have read replicas that have Multi-AZ
* Read replicas can be promoted to their own database. Breaks the replication.
* Can have read replica in a second region

DynamoDB
--------

* Stored on SSD storage
* Spread across 3 geographically distinct data centers
* Eventual Consistent Reads (Default)
  * Consistency between copies usually reached within 1 second
  * Best Read Performance
* Strongly Consistent Reads
  * Returns a result that reflects all writes that received a successful response prior to the read

### Pricing

* Provisioned Throughput Capacity
  * Write throughput $0.0065 per hour for every 10 units
  * Read throughput $0.0065 per hour for every 50 units
* Storage costs of $0.25Gb per month

Redshift
--------

Fast and powerful, fully managed, petabyte-scale scale data warehousing at an affordable price (compared to competitors in this space) of around $1,000 per terabyte per year.

### OLAP

See OLAP Example above...

### Configuration

* Single Node (160Gb)
* Multi-Node
  * Leader Node (manages client connections and received queries)
  * Compute Node (store data and perform queries and computations
  * Up to 128 Compute Nodes

### Columnar Data Storage

* Organizes data based on columns instead of rows
* Optimized for column-based queries used for data warehousing and analytics
* Optimized for aggregate computations over columnar data
* Columnar data is stored sequentially requiring far fewer I/O operations improving query prerformance

### Advanced Compression

* Columnar data compresses far more efficiently than row-based data
* Employs multiple compression techniques
* Does not require indexes or materialized views
* Uses less storage space than traditional RDMBS

### Massive Parallel Processing (MPP)

* Automatically distributes data and query load across all nodes
* Easy to add nodes to your data warehouse
* Scalable to maintain fast query performance as your data warehouse grows

### Pricing

* Compute Node Hours
  * Total number of hours you run across all your compute nodes for the billing period
  * Billed for 1 unit per node per hour
  * Wil not be charged for leader node hours; only compute nodes incur charges
* Backup
* Data Transfer (only within a VPC, not outside it)

### Security

* Encrypted in transit using SSL/TLS
* Encrypted at rest using AES-256
* By default takes care of key management
  * Manage you own keys thorugh HSM
  * AWS Key Managment Service (KMS)

### Availability

* Currently only available in 1 AZ
* Can restore snapshots to new AZs in the event of an outage

Elasticache
-----------

Web services for in-memory cache in the cloud.

* Memcached
* Redis

Aurora
------

Only available on AWS. Compatible with MySQL.

* Performance is 5 times better than MySQL
* Provides high availability
* Cost effective, especially compared to commercial solutions such as Oracle
* Starts with 10Gb
* Scales in 10Gb increments to 64Tb (Storage autoscaling)
* Compute resources can scale up to 32vCPUs and 244GB of memory
* 2 copies of your data is maintained in each AZ, with minimum of 3 AZs (6 copyies of your data)
* Transparently handles loss of database copies without affecting availability
  * 2 copies can be lost for write availability
  * 3 copies can be lost for read availability
* Self-healing

### Replicas

* 2 types of replicas are available
  * Aurora Replicas (currently 15)
  * MySQL Read Replicas (currently 5)