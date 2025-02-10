# Module 3: Storage and Database

## A) Storage:

**Week 3: Storage on AWS - Lecture Notes**

**I. Introduction to Week 3 Storage**

*   **Recap:**  We've covered compute (EC2) and networking (VPC).
*   **Current Goal:**  Set up storage and databases for the Employee Directory application.  The app is currently non-functional because it has no place to store employee data (information and photos).
*   **This Week's Focus:**
    *   Overview of AWS storage services (compare and contrast).
    *   Creating an Amazon S3 bucket for employee images.
    *   Introduction to AWS database services, focusing on:
        *   Amazon Relational Database Service (RDS)
        *   Amazon DynamoDB
*   **Important:**  Review quizzes and reading materials between video lessons for supplemental information.

**II. Storage Types on AWS**

*   **Application Data Needs:** Our Employee Directory app has diverse storage needs:
    *   Application Files: Operating system, software, system files.
    *   Static Assets: Employee photos (headshots).
    *   Structured Data: Employee name, title, location (this requires a database, covered later).
*   **Two Main Storage Types:**
    *   **Block Storage:**
        *   Files are split into fixed-size chunks (blocks).
        *   Efficient for frequent, small updates (changing a single character only requires updating the relevant block).
        *   Better for high transaction rates and frequently updated data (e.g., application files).
    *   **Object Storage:**
        *   Files are treated as single, whole units.
        *   Updating even a small part of a file requires updating the *entire* file.
        *   Suitable for data that is accessed often but modified rarely (e.g., static content like photos).
*   **Matching Storage to Use Case:**
    *   Employee Photos (static, rarely modified): Object storage is a good fit.
    *   Application/System Files (frequently updated): Block storage is preferable.
*   **Upcoming Topics:** We'll discuss AWS services for both block (e.g., EBS, Instance Store) and object (e.g., S3) storage.
*   **Reminder:** Review the provided notes for a refresher on storage types.

**III. Amazon EC2 Instance Storage and Amazon Elastic Block Store (EBS)**

*   **Block Storage for EC2:**  Every EC2 instance needs block storage.  This can be:
    *   **Boot Volume:**  For the operating system.
    *   **Data Volume:**  For additional storage.
*   **Analogy:** Think of your laptop:
    *   Internal Drive (Built-in):  Like EC2 Instance Store.
    *   External Drive (Connected): Like Amazon EBS.
*   **Instance Store:**
    *   *Directly Attached Storage:* Physically attached to the underlying server hosting the EC2 instance.
    *   *Advantage:* Very fast due to proximity.
    *   *Major Disadvantage:*  *Ephemeral* - Data is lost if the instance is stopped or terminated.  The lifecycle of the storage is tied to the instance.
*   **Amazon EBS (Elastic Block Store):**
    *   *Network-Attached Storage:*  Separate drives (volumes) of a user-defined size that are connected to EC2 instances over the network.
    *   *Key Features:*
        *   Multiple EBS volumes can be attached to a single EC2 instance.
        *   Secure communication: Only the attached instance can access the data.
        *   Detachable and Re-attachable:  Volumes can be moved between instances within the same Availability Zone (AZ). Some instance and volume types support EBS Multi-Attach (attaching to multiple instances simultaneously).
        *   *Persistent Storage:* Data remains even if the instance is stopped or terminated.  The EBS volume's lifecycle is independent of the instance.
    *   **When to Use EBS:**  Workloads requiring data persistence.
* **EBS Volume Types:**
    *   **SSD-backed:** Optimized for performance.
    *   **HDD-backed:** Optimized for throughput and cost.
        *   (Detailed comparison in the readings.)
*   **Backups (Snapshots):**
    *   EBS volumes are backed up using *snapshots*.
    *   Snapshots are *incremental* (only changes are saved) and stored *redundantly*.
    *   Used for disaster recovery: Create new volumes from snapshots to restore data.

**IV. Object Storage with Amazon S3 (Simple Storage Service)**

*   **Why Not EBS for Photos?**
    *   EBS volumes are typically connected to only one EC2 instance (Multi-Attach is limited).  Scaling the app becomes problematic.
    *   EBS volumes have size limitations.  Storing a large number of high-resolution photos could become an issue.
*   **Amazon S3:**
    *   *Standalone Storage Solution:* Not tied to compute (no mounting to EC2).
    *   *Storage for the Internet:* Access data via URLs from anywhere.
    *   *Scalability:*
        *   Store virtually unlimited objects.
        *   Individual object size limit: 5 TB.
    *   *Underlying Storage Type:* Object storage (flat structure, unique identifiers).
    *   *Distributed Storage:* Data is stored across multiple facilities within an AWS region.
    *   *High Availability and Durability:* Designed for 99.99% availability and 11 nines of durability.
*   **Key S3 Concepts:**
    *   **Bucket:**  A container for objects.  You *must* create a bucket before uploading anything to S3.
    *   **Object:**  A file stored in S3.
    *   **Folders:**  Used for organization within buckets (optional).
*   **Creating an S3 Bucket (Console Demo):**
    1.  Search for "S3" in the AWS Management Console.
    2.  Click "Create bucket".
    3.  **Region Specificity:** Choose a region (ideally close to your application infrastructure, e.g., Oregon).
    4.  **Globally Unique Bucket Name:**  Must be DNS compliant (no special characters, no spaces).  This name is used in the bucket's URL. Example: `employee-photo-bucket-sr-001`
    5.  Leave other settings as defaults and click "Create".
*   **Uploading Objects:**
    1.  Select the bucket from the list.
    2.  Click "Upload".
    3.  Click "Add files" and choose the file to upload.
    4.  Click "Upload".
*   **Object URL:**
    *   Format: `[Bucket URL]/[Object Key]`
    *   Bucket URL: Generated by AWS based on the bucket name.
    *   Object Key: The name of the object within the bucket.
*   **Access Control (Permissions):**
    *   *Default: Private:*  Everything in S3 is private by default. Only the creator (AWS account) can access resources.
    *   *Access Denied:*  Attempting to access a private object via its URL without authentication results in an "Access Denied" error.
    *   *Making Objects Public:*  It's a deliberate process to prevent accidental data exposure.
    *   **Steps to Make an Object Public (using ACLs):**
        1.  **Disable Block Public Access:** In the bucket's Permissions tab, edit "Block public access (bucket settings)" and uncheck "Block all public access". Confirm the change.
        2.  **Enable ACLs:** In the bucket's Permissions tab, edit "Object Ownership" and select "ACLs enabled". Acknowledge and save.
        3.  **Make Object Public:** Go to the object's details page, select "Actions" -> "Make public using ACL".
        4.  **Verify:** Access the object's URL; it should now be publicly viewable.
    *   **Granular Access Control (Recommended):**
        *   **IAM Policies:** Attached to users, groups, and roles.  Define permissions to access S3 resources.
        *   **S3 Bucket Policies:**  Attached directly to buckets (in JSON format, like IAM policies). Specify allowed/denied actions on the bucket.  Can be used to grant access to other AWS accounts or allow read-only access to anonymous users.  Bucket policies apply to all objects within the bucket.
*   **Recap:** S3 uses buckets to store objects, and access is controlled through IAM policies and bucket policies.

**V. Choose the Right Storage Service (Interactive Quiz)**

*   **Scenario 1: Transcoding Large Media Files**
    *   **Requirement:** Store original and transcoded media files for at least a year.  Uses AWS Lambda.
    *   **Answer:** Amazon S3
    *   **Reasoning:**
        *   Lambda functions don't use EBS.
        *   EBS might not be cost-effective for large files.
        *   Instance Store is ephemeral (data loss on instance termination).
*   **Scenario 2: MySQL Database on EC2**
    *   **Requirement:**  Fast, durable storage for order and customer information (frequently accessed and updated).
    *   **Answer:** Amazon EBS
    *   **Reasoning:**
        *   Requires storage attached to compute (EC2).
        *   Instance Store is fast but not durable (data loss on instance termination is unacceptable for critical data).
*   **Scenario 3: Temporary Calculation Data**
    *   **Requirement:**  Web application needs fast, temporary storage for calculations.  Speed and cost are priorities.
    *   **Answer:** EC2 Instance Store
    *   **Reasoning:**
        *   Storage attached to compute.
        *   Data is temporary, so durability is not a concern.
        *   Instance Store is often included in the EC2 instance price, making it cost-effective.
*   **Scenario 4: Shared WordPress Installation**
    *   **Requirement:**  Multiple EC2 instances need shared access to the WordPress installation and user uploads (traditionally stored on the local file system).
    *   **Answer:** Amazon Elastic File System (EFS) - *Covered in earlier reading*
    *   **Reasoning:**
        *   S3 is object storage, not a file system, and cannot be mounted.
        *   EFS is a distributed file system that can be mounted on multiple EC2 instances, providing shared access to the WordPress files.

---

## B) Database:
**Lecture Notes: AWS Databases for an Employee Directory Application**

**A) Exploring Databases on AWS**

*   **Context:**
    *   We're building an employee directory application (add, view, edit, delete employee data).
    *   Data needs to be stored in a database.
    *   The architecture diagram designates Amazon RDS (Relational Database Service).

*   **Relational Databases (General Overview):**
    *   Widely used across industries.
    *   RDBMS (Relational Database Management Systems) allow creating, managing, and using relational databases.
    *   **Option 1: Databases on EC2:**
        *   You can install and operate database applications (like MySQL, PostgreSQL, etc.) on EC2 instances.
        *   Good for migrating existing databases to AWS.
        *   **Responsibilities:**  You manage OS, database engine installation, multi-AZ setup, replication, patching, and updates.  AWS handles the physical infrastructure.
    *   **Option 2: Managed Databases (Amazon RDS):**
        *   AWS handles much of the "undifferentiated heavy lifting" (instance management, patching, upgrades, installation).
        *   **Your Responsibilities:** Database creation, maintenance, optimization (schema, indexing, stored procedures, encryption, access control).
        *   RDS is a managed service, reducing operational burden.

*   **Upcoming Readings:**
    *   History of enterprise relational databases.
    *   What relational databases are and how they are used.  (Important for those unfamiliar with databases).

**B) Amazon Relational Database Service (RDS)**

*   **Introduction:**
    *   RDS makes it easier to set up, operate, and scale relational databases in the cloud.
    *   Focus:  Demonstration of RDS setup.

*   **RDS Console Demonstration:**
    *   **Step 1: Create Database:**
        *   Choose "Easy create" (for standard best practices like backups and high availability).
        *   "Standard create" option available for granular control.
    *   **Step 2: Engine Options:**
        *   MySQL, PostgreSQL, MariaDB, Oracle, Microsoft SQL Server, Amazon Aurora.
        *   **Amazon Aurora:** AWS-specific, designed for cloud scalability and durability.  Drop-in compatible with MySQL/PostgreSQL.  Up to 5x faster (MySQL) or 3x faster (PostgreSQL).  Consider for high performance/storage needs.
        *   **Choice for Demo:**  MySQL (simple database, no high-performance needs).
    *   **Step 3: Instance Size:**
        *   Similar to choosing an EC2 instance size.
        *   **Choice for Demo:** Free tier eligible instance.
    *   **Step 4: Credentials:**
        *   Database name, admin username, and password.
    *   **Step 5: Launch:**
        *   Instance creation takes a few minutes.

*   **High Availability with RDS:**
    *   **Placement:** RDS DB instances are placed within a subnet in a VPC (like EC2 instances).
    *   **Subnets and AZs:** Subnets are bound to a single Availability Zone (AZ).
    *   **Best Practice:** Replicate solutions across at least two AZs for high availability (production workloads).
    *   **RDS Multi-AZ Deployment:**
        *   Launches a secondary DB instance in another subnet/AZ.
        *   RDS manages data replication (synchronization).
        *   **Failover Management:**
            *   One instance is primary, the other is secondary.
            *   App connects to a single endpoint.
            *   If the primary fails, the secondary is promoted; the endpoint remains the same (no code changes).
            *   **Application Requirement:**  Ensure the app can reconnect (handle momentary outages, update DNS cache, reconnect to the endpoint).

*   **Conclusion:**
    *   RDS significantly simplifies database operation and reduces overhead.

**C) Purpose-Built Databases on AWS**

*   **Key Concept:** Choose the *right* database for your business requirements, not a "one-size-fits-all" approach.

*   **Limitations of Relational Databases (RDS):**
    *   Not always the best choice for all needs.
    *   AWS offers "purpose-built" databases optimized for specific use cases.

*   **Re-evaluating the Employee Directory App:**
    *   **Original Choice:** RDS.
    *   **Issue:**  RDS might be overkill.  The app only stores one record per employee (lookup table).  No complex relationships.  RDS features add unnecessary complexity.
    *   **Billing:** RDS charges per hour of instance runtime, regardless of usage.  The app has higher usage during the week and no usage on weekends.

*   **Introduction to Amazon DynamoDB:**
    *   **NoSQL Database:** Ideal for key-value pairs or document data.
    *   **Scalability:** Works well at massive scale.
    *   **Performance:** Millisecond latency.
    *   **Billing:** Charges based on usage (data reads/writes), not hourly.  Better fit for the employee directory's usage pattern.

*   **Other AWS Database Offerings (Examples):**
    *   **Amazon DocumentDB:** Content management, catalogs, user profiles.
    *   **Amazon Neptune:** Graph database for social networks, recommendation engines, fraud detection.
    *   **Amazon QLDB (Quantum Ledger Database):** Immutable ledger for systems requiring 100% immutability (auditing, compliance, financial records).

*   **Key Takeaway:** AWS provides a variety of database services; choose the best tool for the job.  Leverage AWS's expertise in managing these databases at scale.

**D) Introduction to Amazon DynamoDB**

*   **Core Concepts:**
    *   **Serverless Database:** No need to manage underlying infrastructure.
    *   **Standalone Tables:**  Unlike relational databases, DynamoDB uses standalone tables (not related to each other).
    *   **Data Organization:** Data is organized into *items*, and items have *attributes*.
    *   **Scalability:** DynamoDB manages storage scaling automatically.
    *   **High Availability:** Data is stored redundantly across AZs and mirrored across drives.
    *   **Performance:** Millisecond response time.

*   **NoSQL vs. SQL:**
    *   **Relational Databases (SQL):**
        *   Rigid schemas, complex relationships, constraints.
        *   Good for many use cases, but can have performance/scaling issues under stress.
    *   **NoSQL Databases (DynamoDB):**
        *   Flexible schemas:  Add/remove attributes at any time.  Items don't need the same attributes.
        *   Simpler queries: Focus on collections of items from one table.
        *   Designed for high performance and scalability.

*   **DynamoDB Use Cases:**
    *   Purpose-built: Not the best fit for *every* workload.
    *   Good for datasets with some variation and high access rates.

*   **Architecture Modification:**
    *   The architecture is updated to use DynamoDB instead of RDS.
    *   An "employees" table will be created.

*   **Console Demonstration (with "Seph"):**
    *   **Step 1: Navigate to DynamoDB Service.**
    *   **Step 2: Create Table:**
        *   Designate a primary key (unique identifier).  In this case, `employeeID`.
        *   Accept default settings.
    *   **Application Compatibility:** The app was designed to use either RDS or DynamoDB, so minimal changes are needed.
    *   **Testing:**
        *   Add a new employee via the website (hosted on EC2).
        *   Verify the new item in the DynamoDB console (refresh and scan items).

*   **Conclusion:**
    *   DynamoDB setup is very simple for a lookup table.
    *   More information about DynamoDB will be in the upcoming reading.

Key takeaways:

*   Understand the difference between running databases on EC2 vs. using managed services like RDS.
*   Know the different database engine options available on RDS, including Amazon Aurora.
*   Understand the concept of Multi-AZ deployments for high availability in RDS.
*   Recognize that AWS offers purpose-built databases, and choosing the right one is crucial.
*   Understand the basic concepts of DynamoDB (NoSQL, serverless, standalone tables, items, attributes).
*   Be able to create a simple DynamoDB table and understand its use case for the employee directory app.
