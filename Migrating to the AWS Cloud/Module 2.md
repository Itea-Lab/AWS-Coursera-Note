**Module 2 - Lecture Notes: Migration Assessing Phase and Supporting AWS Tools**

---

## 1. Overview of the Assessing Phase

### 1.1 Definition and Purpose
- The *Assessing Phase* is part of the overall cloud-migration process.
- Focus is on determining **where an organization is** in its cloud journey, identifying **strengths and weaknesses**, and creating an **action plan** to address gaps before a large-scale migration.

### 1.2 Goals of the Assessing Phase
1. **Understand Current State**  
   - Are you already using the cloud, partially or extensively?  
   - Are you fully on-premises?
2. **Identify Strengths and Weaknesses**  
   - Gauge *cloud-readiness* of existing applications and infrastructure.  
   - Determine ease or difficulty of moving to the cloud.
3. **Create an Action Plan**  
   - Resolve foundational issues in advance.  
   - Avoid halting the migration midway to fix problems.

---

## 2. Understanding the Organization’s Cloud Journey

1. **Experimentation Phase**  
   - Many start small (low-impact application) to gain experience.  
   - Look for what worked well and what didn’t.  
   - *Tip:* Investigate any departmental or individual efforts—others may have valuable lessons learned.
2. **Roadblocks**  
   - Identify reasons why you *haven’t* started or expanded cloud usage.  
   - Understand possible constraints (skills, security concerns, tooling, etc.).

---

## 3. Laying the Foundation

1. **Infrastructure Setup**  
   - **VPN** to on-premises or **Direct Connect** for larger, dedicated bandwidth.  
   - Migrate **Active Directory** if necessary.  
   - Implement **auditing/security** tools to meet organizational standards.
2. **Importance**  
   - Proper foundation ensures smoother migration for production or legacy apps.  
   - Ensures that network, security, and identity are ready for the transition.

---

## 4. Migration Steps and Strategies

1. **Migrating the First Applications**  
   - After experimentation and foundation building, move simpler or less business-critical applications.  
   - Demonstrates success to the organization, encouraging broader adoption.
2. **Reinventing Architecture**  
   - Once established in the cloud, consider serverless options (e.g., AWS Lambda), container platforms (e.g., Amazon EKS or ECS), and more managed services.  
   - Aim to reduce reliance on manually managed infrastructure where possible.

---

## 5. Strengths and Weaknesses Review

### 5.1 Stakeholder Involvement
- Involve executives and senior technical leaders (CEO, CTO, CIO, Security, Networking, etc.).  
- Use this to foster *broad alignment* and secure continued buy-in.

### 5.2 AWS’s 70-Question Assessment
- *AWS provides a robust set of questions* to guide this review.  
- Focuses on understanding gaps in tooling, security, governance, and skills.
- **Note:** The “favorite ice cream flavor” question was humor, not official!

### 5.3 Outcome
- Identify *strengths* (e.g., well-documented infrastructure) and *opportunities* (e.g., insufficient automation).  
- *Avoid blame;* the goal is to create a plan for improvement, not assign fault.

---

## 6. Presenting the Findings

1. **Transparency**  
   - Make results visible to all IT stakeholders.  
   - Clearly assign each gap to a responsible owner or team.  
   - Regular check-ins to track progress.
2. **Action Items**  
   - Must be specific and time-bound.  
   - Example: “Migrate legacy code to new frameworks by Q2.”

---

## 7. AWS Migration Evaluator

### 7.1 Purpose
- *Helps build a business case* for AWS migration by analyzing on-premises usage and projecting AWS costs.
- **No additional cost** to use.

### 7.2 Process
1. **Contact AWS** via a form to engage the Migration Evaluator team.  
2. **Data Collection**  
   - Install an agent on servers or at vSphere level (if using VMware).  
   - Optionally, upload CSVs if you already have data.  
   - Collects usage, licensing, and resource information.
3. **Report Generation**  
   - Provides an executive summary (e.g., possible 15% resource shutdown).  
   - Estimates current on-prem cost and projects possible savings in AWS.  
   - Offers *directional guidance* on resource sizing and cost analysis.

### 7.3 Key Takeaways
- Great tool for the *Assessing Phase* to build leadership buy-in.  
- Focuses on *cost optimization* and *accurate inventory* of current infrastructure.

---

## 8. AWS Migration Hub

### 8.1 Purpose
- A **single console** for discovering resources, assessing readiness, planning migrations, orchestrating steps, and tracking status.

### 8.2 Core Functions
1. **Discover**  
   - Collect inventory of on-premises servers/apps via *AWS Application Discovery Service* or CSV import.  
2. **Assess**  
   - Receive recommendations on AWS instance types and cost estimates.  
3. **Strategy**  
   - Get modernization recommendations (e.g., refactor to microservices).  
4. **Orchestrate**  
   - Plan who does what, track progress through each phase.  
5. **Migrate**  
   - Integrates with AWS tools like *Application Migration Service (MGN)* or *Database Migration Service (DMS)*.

### 8.3 Discovery Tools
- **AWS Application Discovery Agent**: Installed directly on servers (collects config, processes, performance).  
- **AWS Agentless Discovery Connector**: Deployed as a VM in VMware environments, collects data at the hypervisor level.  
- **Migration Hub Import**: Manual CSV-based import for orgs with existing resource data.

---

## 9. Translating On-Premises Services to AWS

### 9.1 Servers and Compute
- On-prem servers typically map to **Amazon EC2 instances**.
- Consider rehosting in EC2 or replatforming to container services (Amazon EKS, ECS) or serverless (AWS Lambda).

### 9.2 Load Balancers
- Traditional load balancers map to **Elastic Load Balancing**:
  - **Application Load Balancer (ALB)** – Layer 7, for HTTP/HTTPS with advanced routing.  
  - **Network Load Balancer (NLB)** – Layer 4, handles millions of requests per second at low latency.  
  - **Gateway Load Balancer (GLB)** – For deploying and scaling third-party virtual appliances.

### 9.3 Databases
- Many on-prem databases can be migrated to **Amazon RDS** (managed relational database) or **EC2** (self-managed).  
- RDS supports multiple engines (e.g., Amazon Aurora, MySQL, PostgreSQL, Oracle, etc.), providing easier scaling and maintenance.

---

## 10. Translating Networking

### 10.1 Building a VPC
- **Amazon VPC**: Define IP ranges, subnets (public and private), security groups, NAT gateways, and route tables.  
- Public subnets allow inbound/outbound internet access.  
- Private subnets have no direct inbound from the internet.

### 10.2 Connectivity to On-Prem
- **VPN**: Encrypted site-to-site connection for smaller traffic volumes.  
- **AWS Direct Connect**: Dedicated fiber link for higher throughput (50 Mbps to 10 Gbps). Ideal for hybrid or large-scale, ongoing migrations.

---

## 11. Mainframe Modernization and Special Workloads

### 11.1 Mainframes
- **AWS Mainframe Modernization**: Tools and services to refactor or re-platform mainframe COBOL apps.  
- Options to *transform code to Java* or *migrate “as is”* to a mainframe-compatible runtime in AWS.

### 11.2 SAP
- Over **130 AWS instance types** are SAP-certified.  
- **AWS Launch Wizard** can set up new SAP environments in hours.  
- Existing workloads can use *lift-and-shift* automation tools or **AWS Backup** for SAP HANA.

---

## 12. Example Use Cases and Conversations

1. **Walter & Nikki’s MRA Meeting**  
   - Identified old servers to retire.  
   - Found the need to retool DevOps for correct deployment targets.  
   - Decided on a VPN for initial connectivity and a VPC design for staging migrated apps.
2. **Frank from AWS**  
   - Demonstrated **Migration Evaluator** to produce cost-saving analyses and on-prem comparisons.  
3. **Closet Mainframe Discovery**  
   - Highlighted how legacy systems can appear unexpectedly.  
   - Emphasized **AWS Mainframe Modernization** as an option.

---

## 13. Key Takeaways / Summary

1. **Assess Early and Thoroughly**  
   - Identify organizational cloud maturity and existing resources.
   - Determine strengths and areas needing improvement before deep migrations.
2. **Leverage AWS Tools**  
   - **Migration Evaluator** for cost and resource analysis.  
   - **Migration Hub** to unify discovery, planning, and tracking.
3. **Map On-Prem to AWS Services**  
   - EC2 for servers, ELB for load balancing, RDS for databases, VPC for networking.  
   - Keep an eye out for serverless or container-based optimizations.
4. **Networking Foundation is Critical**  
   - Align with the networking team.  
   - Decide on VPN or Direct Connect early to support hybrid usage.
5. **Don’t Forget Specialized Workloads**  
   - Mainframes can be addressed with AWS Mainframe Modernization.  
   - SAP solutions have certified instance types and automation support.

---
