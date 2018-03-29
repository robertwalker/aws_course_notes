Simple Storage Service (S3)
---------------------------

S3 is Object Based Storage. Spread across multiple devices and facilities.

* Object Based Storage
* File from 0 to 5 TB
* Unlimited storage
* Buckets store files
* S3 names must be globally unique
* URL to Bucket -- https://s3-us-east-1.amazonaws.com/bucket_name
* Upload successful return HTTP OK (200)

### Data Consistency

* Read after Write consistencey for PUTS of new Objects
* Eventual Consistency for overwrite PUTS and DELETES

### Key Value Store

* Key (Name of the object)
* Value (The data stored as a sequence of bytes)
* Version ID
* Metadata
* Subresources
  * Access Control Lists
  * Torrent

### The Basics

* Build for 99.99% availability
* Amazon guanantees 99.9% availability
* Eleven 9's durability (99.999999999%)
* Tiered storage available
* Lifecycle Management
* Versioning
* Encryption
* Secured via ACLs and Bucket Policies

### Storeage Tiers/Classes

* S3 -- 99.99% availability, 99.999999999% durability
* S3-IA (Infrequently Accessed)
* Reduced Redundancy Storage (RRS) -- 99.99% durability and 99.99% availability
* Glacier -- Very cheap archival service with 3-5 hour access times

### Charges

* Storage
* Requests
* Storage Management Pricing
* Data Transfer Pricing -- Uploading data is free. Moving data has a charge

### Transfer Acceleration

Fast, easy and secure transfer of files over long distances between end users and S3 bucket. Takes advantage of CloudFront's globallly distributed edge locations over optimized network path.

**Important:** Read the S3 FAQ before taking the exam. It comes up A LOT!

### Buckets

* Buckets are configured in the Global region
* Minimum size for object is 0 bytes
* Control access via ACL or Bucket Policies
* Buckets and objects are private by default

### Versioning

* Once versioning is enabled it cannot be disabled
* Versioning can be suspended but not disabled

### Encryption

* In transit with SSL/TLS.
* At Rest -- Server Side Encryption:
  * S3 Managed Keys -- SSE-S3
  * AWS Key Management Service, Managed Keys -- SSE-KMS
  * Server Side Encryption with Customer Provided Keys -- SSE-C
* Client Side Encryption

### Cross-region Replication

* Bucket versioning is required
* Needs a role (can be created on demand)
* Applies only to new objects
* Existing object would need to be manually transferred to replica
* Use `aws` command line to copy (`aws cp --recursive`) existing objects to replica
* Delete markers are replicated
* Removing a delete marker or individual version is NOT replicated
* Cannot replicate to multiple buckets or use daisy chaining

### Lifecycle Managment

* Can be used in conjunction with versioning
* Can be applied to current and previous versions
* Trasition to the Standard - Infrequent Access Storage Class (30 days min)
* Archive to Glacier Storage Class (60 days min)
* Permanently Delete (Including Glacier) (180 days min)

---

CloudFront (CDN)
----------------

CloudFront is Amazon's Content Delivery Network (CDN).

### Key Terminology

* Edge Location -- This is the location where content will be cached
* Origin -- Location of original files (S3 Bucket, EC2 Instance, Elastic Load Balancer or Route53)
* Distribution -- This is the given name of te CDN consisting of Edge Locations

Storage Gateway
----------------

Your Data Center (Virtual appliance hipervisor) ---> AWS (S3/Glacier)

* File Gateway (NFS)
* Volume Gateway (iSCSI)
  * Stored Volumns
  * Cached Volumns
* Tape Gateway (VTL)

Snowball
--------

Previously called Import/Export.

* Snowball -- Physical disk appliance sent to you by Amazon
* Snowball Edge -- 100TB of data with compute capabilities (Lambda functions)
* Smowmobile -- 100PB Semi-trailer truck

S3 Transfer Acceleration
------------------------

Use Edge Locations and CloudFront to accelerate file uploads.

Static Website Hosting
----------------------

Endpoint URL follows pattern `http://bucket_name.s3-website-us-east-1.amazonaws.com`