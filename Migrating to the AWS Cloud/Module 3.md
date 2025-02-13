# Module 3: The mobilize phase of the migration

## 1. **Introduction to the Mobilize Phase**

### 1.1 Purpose of the Mobilize Phase
- **Refine Business Case**: Validate and refine cost estimates, identify needed resources, and confirm the ROI (Return on Investment) for migrating to AWS.
- **Create a Migration Plan**: Develop a detailed migration roadmap and address any organizational readiness gaps discovered in the *Assess* phase.
- **Build Baseline Environment (Landing Zone)**: Establish foundational AWS accounts, security controls, and best practices before large-scale migration.
- **Develop Cloud Skills and Operational Readiness**: Train teams on cloud services, governance models, and operational processes.

### 1.2 Key Activities
- **Deeper Application Understanding**: Identify interdependencies among applications to choose the optimal migration approach.
- **Rationalize Applications**: Use the seven common migration strategies: *relocate, rehost, replatform, refactor, repurchase, retire, or retain*.
- **Tooling and Automation**: Employ AWS and partner tools to streamline decision-making and migration.

> *Key Takeaway:* The Mobilize phase focuses on planning, refining business objectives, and setting up a secure environment to ensure success in the upcoming migrations.

---

## 2. **Migration Strategies and Planning**

### 2.1 Application Portfolio Data Collection
- **AWS Application Discovery Service**:
  - Collects details about servers, dependencies, and resource utilization.
  - Helps in right-sizing and mapping application interdependencies.
- **AWS Migration Hub**:
  - A central tracking service for migrations, integrating with AWS tools and AWS Partner tools.
  - Streamlines planning and monitoring across multiple resources.

### 2.2 Seven Migration Strategies
1. **Relocate**: Move VMware environments to AWS without extensive changes (e.g., VMware Cloud on AWS).
2. **Rehost** (Lift-and-Shift): Migrate applications to AWS with minimal modifications.
3. **Replatform** (Lift, Tinker, and Shift): Make minor optimizations during migration (e.g., changing the database engine).
4. **Refactor**: Redesign parts of an application for a cloud-native architecture.
5. **Repurchase**: Move to a new product (e.g., replace a self-managed solution with a SaaS product).
6. **Retire**: Decommission applications no longer needed.
7. **Retain**: Keep some applications on-premises if migration is not beneficial or practical.

> *Key Takeaway:* Choosing the right migration strategy depends on the application’s business value, complexity, and technical requirements.

---

## 3. **Governance and Controls: AWS Management and Governance Services**

### 3.1 Overview
- **Objective**: Maintain agility and innovation while enforcing security, compliance, and cost controls.
- **Importance**: Prevent runaway costs, inconsistent standards, or misconfigurations that can introduce security risks.

### 3.2 Enable Category (Environment Setup)
- **AWS Control Tower**:
  - Automates multi-account setup with prescriptive best-practice guardrails.
  - Sets up logging, auditing, and security in a standardized manner.
  - Ideal for organizations wanting a quick, guided setup.
- **AWS Landing Zone**:
  - Customizable solution for creating a secure multi-account environment.
  - Deployed via AWS Solutions Architects, Professional Services, or AWS Partner Network.
  - Allows deeper customization of account baselines and pipelines.

> *Visual Aid (Optional)*:  
> A diagram comparing **Control Tower** (predefined guardrails, quick start) vs. **Landing Zone** (extensive customization) could illustrate which solution fits different organizational needs.

### 3.3 Provision Category (Building Architecture)
- **AWS CloudFormation**:
  - *Infrastructure as Code* service to define and provision AWS resources predictably.
  - Ensures consistent, repeatable deployments across environments.
- **AWS Service Catalog**:
  - Simplifies provisioning by offering a catalog of vetted, preconfigured product templates.
  - Users pick from approved templates (“buffet approach”) to maintain compliance.

### 3.4 Operate Category (Maintaining Applications)
- **AWS Systems Manager**:
  - Automates configuration, patching, and deployment tasks across large fleets of EC2 instances.
  - Provides a centralized interface for operational workflows.
- **Amazon CloudWatch**:
  - Monitors resources in real time (CPU usage, memory, logs).
  - Critical for performance metrics and alerting on potential issues.

> *Key Takeaway:* These services work together to standardize deployment, enforce security, and provide operational visibility—essential for large-scale, well-governed migrations.

---

## 4. **Application Discovery Service in Action**

### 4.1 Purpose
- Gathers in-depth information (e.g., OS details, CPU, RAM usage, network dependencies) to inform migration decisions.
  
### 4.2 Agent-Based vs. Agentless
- **Agent-Based**:
  - Install the AWS Application Discovery Agent on each server.
  - Ideal for detailed metrics and dependency mapping.
- **Agentless**:
  - Use the AWS Agentless Discovery Connector for VMware vCenter.
  - Less granular but easier to deploy at scale.

### 4.3 Practical Example
- By installing an agent on each server, you see:
  - *Resource utilization* to help with capacity planning.
  - *Network traffic* and *port usage* to clarify dependencies.
- This data is consolidated in AWS Migration Hub, enabling better right-sizing decisions.

> *Key Takeaway:* The Application Discovery Service automates much of the “detective work,” ensuring accurate data to shape both migration strategies and future AWS resource provisioning.

---

## 5. **Common Considerations and Pitfalls**

### 5.1 Organizational Alignment
- **Everyone on the Same Page**:
  - *Training*, *documentation review*, and *experimentation* are crucial.
  - Use the *AWS Cloud Adoption Framework (CAF)* for structured guidance:
    - Six perspectives: **Business**, **People**, **Governance**, **Platform**, **Security**, **Operations**.

### 5.2 Functional Requirements vs. Existing Components
- Don’t automatically mirror on-premises environments in AWS.
- Ask questions like:
  - Do we really need continuously running servers, or can we use AWS Lambda?
  - Managed databases (RDS) vs. self-managed on EC2?
  - Do we need multi-Region replication, or is a CDN sufficient?

### 5.3 Security and Compliance Risks
- Neglecting security can lead to data leaks and breaches.
- **AWS Management and Governance** services (Control Tower, Organizations, Budgets) help define strict rules.
- Tools like AWS CloudFormation, Service Catalog, and Marketplace keep resources aligned with policies.

### 5.4 Resistance to Change
- A strict “lift-and-shift” might ignore opportunities to modernize.
- Encourage *experimentation and innovation*, but do it gradually.
- Continuous improvement: Periodically review to optimize and modernize workloads.

### 5.5 Lack of Clear Business Objectives
- A migration with no clear plan can result in wasted effort and minimal ROI.
- Develop a *detailed, goal-aligned migration strategy* that meets specific requirements.

> *Key Takeaway:* Planning, readiness, clear objectives, security measures, and openness to modernizing are critical to a smooth and successful migration.

---

## 6. **Partnering and Additional Resources**

### 6.1 AWS Migration Competency Partners
- Third-party organizations with proven AWS migration expertise.
- Can assist in:
  - *Discovery and Planning*
  - *Business-Case Analysis*
  - *Migration Execution*
  - *Ongoing Modernizations*

### 6.2 Resources and Documentation
- **AWS Prescriptive Guidance**: Detailed migration strategies and best practices.
- **AWS Partner Network**: Case studies, whitepapers, and eBooks.
- Partner involvement can speed up migrations, especially for complex or specialized scenarios.

> *Key Takeaway:* Leverage the AWS Partner Network to supplement internal knowledge and accelerate migrations while ensuring best-practice alignment.

---

## 7. **Real-Life Scenarios and Illustrations**

- **Nikki and Alex’s Conversation**: Demonstrates using the Application Discovery Service to map dependencies and gather performance data.
- **Nikki’s Business Case Development**: Emphasizes the importance of cost estimation, buy-in from leadership, and connecting migration activities to business outcomes.
- **Walter’s Governance Setup**: Highlights the use of CloudFormation, Control Tower, and other governance tools to establish a secure, compliant multi-account environment.

---

## 8. **Key Takeaways and Summary**

1. **Mobilize Phase Focus**: Develop a thorough migration plan, address readiness gaps, and establish a baseline cloud environment.
2. **Tools & Services**:  
   - *Application Discovery Service* and *Migration Hub* for data-driven planning.  
   - *AWS Control Tower*, *Landing Zone*, *CloudFormation*, and *Systems Manager* for governance and secure provisioning.
3. **Security & Compliance**: Enforce best practices with guardrails, logging, and auditing to avoid compliance pitfalls.
4. **Migration Strategies**: Choose from rehost, replatform, refactor, etc., based on application needs.
5. **Continual Improvement**: Recognize that *modernization* is an ongoing process; re-evaluate architecture post-migration.
6. **AWS CAF and Training**: Align stakeholders via the Cloud Adoption Framework, and upskill teams for cloud proficiency.
7. **Leverage AWS Partners**: Access specialized knowledge, tools, and hands-on guidance to streamline complex migrations.

---

## 9. **Potential Gaps or Points for Further Clarification**
- **Licensing Considerations**: How to handle third-party licenses (e.g., Microsoft, Oracle) during cloud migration.
- **Hybrid Environments**: Detailed strategies for coexisting on-premises and cloud architectures during gradual migration.
- **Cost Management**: Deeper exploration of AWS cost-optimization tools (e.g., AWS Cost Explorer, Savings Plans) beyond high-level references.

---

### Final Note
By following these best practices—planning thoroughly, leveraging the right AWS services, maintaining strict governance, and constantly revisiting modernization opportunities—you can navigate the mobilize phase effectively and set the foundation for a successful and efficient migration to AWS.