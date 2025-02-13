# **Module 4: AWS Migration: Lecture Notes**

## **1. Introduction & Overview**
- **Scope**: These notes cover key AWS services, strategies, and best practices for migrating workloads from on-premises environments to AWS.
- **Main Phases Discussed**:  
  1. *Evaluate & Readiness* (previously covered in detail)  
  2. **Migrate & Modernize** (primary focus of these notes)  
  3. *Operate* (post-migration phase)

> **Key Point**: AWS provides a suite of tools and services to help automate, optimize, and secure migrations at every stage.

---

## **2. The Migrate & Modernize Phase**
- **Objective**: Move applications and their components to AWS, with minimal downtime and maximum efficiency.
- **Key Activities**:
  - Finalize each application’s design for AWS.
  - Prepare for migration and validate the process.
  - Continue using **AWS Migration Hub** to track and coordinate progress.
  - Explore and utilize specialized services like **AWS Application Migration Service (MGN)**.

> **Optional Visual Aid**: A *Migration Flow Diagram* showing each application’s path from on-prem to AWS using AWS Migration Hub and AWS MGN.

---

## **3. AWS Migration & Modernization Tools**

### **3.1 AWS Migration Hub**
- **Purpose**: Central dashboard to **plan, run, and track** application migrations.  
- **Benefits**: 
  - Single-pane-of-glass for multiple migration tools.  
  - Streamlines migration status monitoring.

### **3.2 AWS Application Migration Service (MGN)**
- **Purpose**: *Lift and shift* (rehost) servers from on-premises or other clouds to AWS.
- **How It Works**:
  1. **Install the agent** on the source server or use agentless replication (e.g., if using VMware).
  2. **MGN replicates** entire server (OS, apps, data, and configurations) to AWS.
  3. **Encrypted data transfer** in transit, with optional *encrypted EBS volumes* in AWS for storage.
- **Key Feature**: **Test Environments**  
  - Spin up a smaller environment for safe testing prior to moving production traffic.  
  - **Cut-over strategy**: once tests pass, redirect production traffic to the new AWS environment.

> **Note**: Under the AWS Free Tier (as of the script’s recording), AWS MGN offers *90 days* (2,160 hours) of free replication time.

### **3.3 Other Migration Tools**
- **AWS Database Migration Service (DMS)**: For database migrations and ongoing replication (covered more in [Section 4](#4-database-migration-services-aws-dms-and-related-tools)).
- **AWS DataSync**: For *file-based data transfers* to and from AWS (discussed in [Section 5](#5-data-transfer-services-aws-datasync-storage-gateway-and-direct-connect)).
- **AWS Transfer Family**: *Secure file transfer* (SFTP/FTPS/FTP).
- **AWS Snow Family**: *Physical devices* for offline data transfer (detailed in [Section 6](#6-aws-snow-family-offline-data-migration)).

---

## **4. Database Migration Services (AWS DMS and Related Tools)**

### **4.1 AWS Database Migration Service (DMS)**
- **Purpose**: Migrate and transform databases quickly with minimal downtime.
- **Supports**: Oracle, Microsoft SQL, MySQL, MariaDB, PostgreSQL, MongoDB, and more.
- **Ongoing Replication**: *Continuously syncs* source and target databases with minimal latency.
- **Use Cases**:
  1. **Homogeneous Migrations** (same or compatible engines, e.g., Oracle → Amazon RDS for Oracle).
  2. **Heterogeneous Migrations** (different engines, e.g., Oracle → Amazon Aurora).
  3. **Database Consolidation** (multiple sources into one target).
  4. **Development & Testing** (migrate data to/from AWS for staging environments).

### **4.2 AWS Schema Conversion Tool (SCT)**
- **Role**: Used for *heterogeneous migrations* to automatically convert schema/code (views, stored procedures, functions) to the target database format.
- **Cloud-Native Optimization**: Can convert legacy DB functions to equivalent AWS services.
- **Manual Adjustments**: Any unconvertible object is flagged for manual edits.

### **4.3 AWS DMS Fleet Advisor**
- **Purpose**: Automates migration planning for *database and analytics fleets*.
- **Capabilities**:
  - Inventories on-prem databases and analytics systems.
  - Assesses them for potential migration paths.
  - Helps create a customized plan in hours instead of weeks/months.

> **Key Takeaway**: **DMS + SCT + DMS Fleet Advisor** together streamline database migration, from planning and schema conversion to final data replication.

---

## **5. Data Transfer Services: AWS DataSync, Storage Gateway, and Direct Connect**

### **5.1 AWS DataSync**
- **Purpose**: Securely and automatically **copy data** between on-premise storage systems and AWS (e.g., NFS, SMB, HDFS).
- **Mechanism**: Uses a lightweight *agent* (VM) that reads/writes data to/from your source and synchronizes to AWS storage services.

### **5.2 AWS Storage Gateway**
- **Purpose**: Low-latency, on-premises data access **backed by AWS**.
- **Appliance**: Deployed as a VM or hardware appliance in your data center.
- **Storage Gateway Types**:
  1. **File Gateway** (NFS or SMB → Amazon S3).
  2. **Volume Gateway** (store data locally; replicate to S3; EBS snapshot integration).
  3. **Tape Gateway** (virtual tapes archived to Amazon S3 Glacier).

### **5.3 AWS Direct Connect**
- **Purpose**: Establish a dedicated network connection from your data center to AWS (bypassing the public internet).
- **Benefits**: Reduced latency, more consistent network performance, and secure connectivity.

> **Key Tip**: These services can be combined to meet complex or large-scale data transfer needs. For instance, pair DataSync with **Direct Connect** for faster, more stable transfer speeds.

---

## **6. AWS Snow Family: Offline Data Migration**

### **6.1 Overview & Purpose**
- **Problem Addressed**: Large-scale data transfers that can be time-consuming or impractical over standard internet connections.
- **Main Devices**:
  1. **AWS Snowcone** – *8 TB* capacity + built-in compute.
  2. **AWS Snowball Edge** – *42–80 TB* capacity, two variants:
     - *Storage Optimized* (80 TB).
     - *Compute Optimized* (42 TB + more powerful processors).
  3. **AWS Snowmobile** – *100 PB* containerized in a semi-truck.

### **6.2 General Workflow**
1. **Order** the device(s) from the AWS Console.
2. **Copy / Encrypt** data onto the device.
3. **Ship** the device to AWS.
4. AWS **ingests** data into the specified S3 bucket or other storage service.

### **6.3 Example Use Cases**
- **Remote or bandwidth-limited sites** (ships at sea, research stations).
- **Mass data center migrations** (Snowmobile for extreme volumes).
- **Edge compute**: Snow devices can run local compute jobs (e.g., transform or filter data) before final transfer to AWS.

---

## **7. Post-Migration & Operating Phase**

### **7.1 What Happens After Migration?**
- **Optimize Workloads**: 
  - Consider adopting serverless (e.g., AWS Lambda) or containers (e.g., Amazon ECS/EKS).
  - Evaluate further *modernization* steps based on application needs.
- **Continuous Cycle**: 
  - Once an app is migrated, repeat for new workloads.
  - Or move to full *operational maintenance* in AWS.

### **7.2 Key Tools for Operations**
1. **AWS CloudFormation**:
   - *Infrastructure as Code* (IaC).
   - Easily replicate environments across Regions within minutes.
   - Reduces manual setup errors and speeds deployments.
2. **AWS Service Catalog**:
   - Delivers *self-service* resource provisioning (built atop CloudFormation templates).
   - Enforces organizational best practices and compliance.
3. **AWS Managed Services (AMS)**:
   - For enterprises wanting AWS to handle day-to-day ops, security, and scaling.
   - AWS acts as the Managed Service Provider (MSP), handling infrastructure setup, patching, and more.

### **7.3 Continual Learning**
- **AWS Skill Builder**: Official AWS training content with deep dives into services.
- **AWS Well-Architected Framework**: Six pillars for best-practice guidance (e.g., Operational Excellence, Security, Reliability, etc.).
- **Documentation & Whitepapers**: Keep reading official AWS docs for up-to-date features.

---

## **8. The Seven Rs of Migration**
1. **Relocate**: Move entire VMware-based environments to AWS without re-architecting.
2. **Rehost** (Lift & Shift): Move applications to AWS as-is (e.g., using MGN).
3. **Re-platform** (Lift, Tinker, & Shift): Minimal changes, like swapping out the database engine.
4. **Repurchase**: Move to a new product or licensing model (e.g., SaaS).
5. **Refactor**: Rearchitect to adopt cloud-native patterns (microservices, serverless).
6. **Retain**: Keep certain components on-prem (e.g., compliance or legacy constraints).
7. **Retire**: Decommission obsolete apps or workloads.

> **Tip**: Use these Rs to categorize each workload or application early in your migration plan.

---

## **9. Key Takeaways & Summary**
- **Holistic Planning**: Assess readiness, define objectives, and map your on-prem architecture to AWS services.
- **Data & Database Tools**: AWS DMS, SCT, DataSync, Storage Gateway, Transfer Family for robust data migration.
- **Offline Data Transfer**: Snow Family for large-scale or bandwidth-limited scenarios.
- **Testing & Validation**: Always spin up staging or test environments before cutting over production traffic.
- **Post-Migration**: Embrace IaC (CloudFormation), consider modernization, or use AWS Managed Services for ongoing support.
- **Iterative Approach**: Migration can be cyclical; once one application is in the cloud, move to the next or optimize further.
- **7 Rs**: A comprehensive framework to decide the fate of each workload in your environment.

> **Connections**:  
> - AWS Migration Hub ties together multiple tools (e.g., MGN, DMS).  
> - DataSync, Storage Gateway, and the Snow Family can complement or replace each other depending on data size and network constraints.  
> - DMS + SCT addresses the complexities of database migrations, both homogeneous and heterogeneous.

---

## **10. (Optional) Gaps or Ambiguities**
- **Specialty Scenarios**: Mainframe or specific third-party system migrations might need specialized guidance.
- **Edge Case Management**: Handling extremely large multi-region migrations or complex hybrid architectures can add additional steps.
- **Licensing & Compliance**: Some software licenses or compliance rules (HIPAA, PCI-DSS) may require extra steps or specific AWS configurations.

> If encountered, consult AWS documentation or AWS Professional Services for tailored solutions.

---

# **Conclusion**
These notes summarize the *migrate-and-modernize* phase of moving workloads to AWS, along with essential tools and strategies for **data transfer**, **database migration**, and **operational best practices**. By leveraging AWS’s specialized services and following a thorough plan (including the 7 Rs approach), organizations can accelerate their cloud adoption while minimizing risks and downtime.