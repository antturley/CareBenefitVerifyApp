```markdown
# Care Benefit Verification App

A Salesforce Health Cloud extension that connects to the Benefits Verification Service, enabling streamlined eligibility workflows directly within Salesforce. This managed package includes the full data model, trigger handlers, Apex classes, permission sets, queue-driven task generation, and external credential support.

---


## 🚀 Setup and Deployment Guide

This guide walks you from environment setup to deploying and customizing the Care Benefit Verification App in a scratch org.

⚠️ Challenges I Faced
Working on this project was exciting, but it came with some tricky parts. Here are a few of the biggest challenges I ran into along the way:

Hard-to-Find Help When I tried to set up Care Benefit Verification, there wasn’t much clear help or instructions online. The official Salesforce setup didn’t even explain how to make the parts work together. I had to guess, test, and keep fixing things until I found the right way to make it work. It took a lot of time and patience.

Setting Up the Package Namespace Making the 2nd-generation package (2GP) itself was easy. But adding a namespace took extra work. I learned that I couldn’t do everything in just one Salesforce org. I needed two different Developer Edition orgs—and one of them couldn’t even be a Dev Hub. That part was pretty confusing until I figured it out.

Learning Something Totally New I had never worked with Health Cloud’s benefit verification tools before. So I spent a lot of time reading, researching, and testing things to understand how it all fit together. Even though it was tough at times, I really enjoyed learning and building something new from scratch.

💭 Assumptions & Early Decisions
At the start of this project, I made a few assumptions that shaped my first approach:

Queueable Approach for External Requests I assumed I could build a Queueable Apex class to send benefit verification requests in bulk. This would’ve made retry logic easier and allowed me to batch multiple CareBenefitVerifyRequest__c (CBVR) records in a single outbound call.

Trigger-Based Population I originally thought I could simply fire a trigger on CBVR records and let users populate them manually via a custom button or Lightning Web Component (LWC).

🧭 How My Approach Evolved
After reading the Health Cloud documentation more closely, I realized the native Benefit Verification component automatically creates CBVR and CoverageBenefit records. That meant:

My trigger-based approach needed to shift.

I had to write logic that responded to these records after they were created, not before.

I transitioned to using object-level triggers for CareBenefitVerifyRequest__c and CoverageBenefit__c instead of relying on custom flows.

⌛ Quick Decision: Custom Task vs. Case
Looking back, I realize that I could have used Salesforce Cases to track benefit verification tasks. That would’ve saved time and aligned better with Salesforce’s built-in automation. But with time running short, I created a custom object (CareBenefitVerificationTask__c) to track the work instead along with a few other HealthCloud Native Objects. 

---

### 🛠 Prerequisites

- [Salesforce CLI](https://developer.salesforce.com/tools/sfdxcli)
- [Visual Studio Code](https://code.visualstudio.com/)
- Salesforce Extensions for VS Code
- A [Developer Edition org](https://developer.salesforce.com/signup) with Dev Hub enabled

🎥 Code & App Walkthroughs
Take a guided look into the architecture, workflows, and integration design of the Care Benefit Verification App:

🔧 Code Walkthrough — Apex logic, trigger flows, metadata structure, and queue-based task automation 📺 Watch Code Walkthrough - https://drive.google.com/file/d/11TSv7TGhUfo50LdTrF1PJ8c9siAKjsNj/view?usp=sharing

💡 Benefit Verification App Walkthrough — Full app demonstration, including UI configuration and user flow 📺 Watch Benefit Verification App Walkthrough - https://drive.google.com/file/d/1AGJ3DqTPiRXz5CYtCTZcBoFFdu6o84A6/view?usp=sharing

---

### 🔧 Local Environment Setup

1. **Install Salesforce CLI**  
   > _Note: If CLI isn't added to your system path, manually add: `/Program Files/SF/bin`_

2. **Verify CLI Installation**
   ```bash
   sf -v
   ```

3. **Install Salesforce Extensions Pack** in VS Code from the Extensions Marketplace.

4. **Create a New SFDX Project**
   ```bash
   sf force:project:create -n care-benefit-verification-app
   ```

5. **Initialize Git**
   ```bash
   git init
   ```
   Push to GitHub or Bitbucket after first commit.

---

### ☁️ Dev Hub & Org Configuration

6. **Enable Dev Hub** in your Developer Edition org (via Setup UI).

7. **Authorize Dev Hub**
   ```bash
   sf org login web --set-default-dev-hub --alias careBenefitOrgDevHub
   ```

8. **Authorize Target Org**
   > _Typically the same as your Dev Hub for development purposes._

---

### 📦 Package Setup

9. **Create a Managed Package**
   ```bash
   sf package create --name "Care Benefit Verification App" --path force-app --package-type Managed
   ```

10. **Store your package ID** — e.g.:
    ```
    Package Id │ <Your-PackageId>
    ```

---

### 🧪 Scratch Org Setup

11. **Create `project-scratch-def.json`** in `config/`, customized to include Health Cloud settings and Person Account.

12. **Generate Scratch Org**
   ```bash
   sf org create scratch -f config/project-scratch-def.json -a HealthCloudDev
   ```

13. **Push Source**
   ```bash
   sf project deploy start --target-org HealthCloudDev
   ```

---

### 🧱 Data Model & Configuration

14. **Create Business Record Type** on `Account`

15. **Enable Person Account** in scratch org and assign access via permission set

16. **Set Up Standard Benefit Verification Process**

17. **Create & Assign Permission Sets**
   - Custom: `Care_Benefit_Verification`
   - Standard: `HealthCloudFoundation` or `HealthCloudStarter`

18. **Set Up Test User** with Sales Cloud access and Health Cloud Platform license

19. **Follow Salesforce Setup Guide**  
   [Admin Guide](https://help.salesforce.com/s/articleView?id=ind.admin_benefit_verification_data.htm&type=5)

---

### 🔐 External Credential Configuration

25. **Create External Credential**
   - Authentication Protocol: Custom
   - Add Principal: Username + Password

---

### 🛠 Development Highlights

- Custom Apex classes:  
  `healthcloudext.BenefitsVerificationRequest`, `CareBenefitVerifyHandler`, etc.

- Custom objects include:
  - `CareBenefitVerifyRequest`
  - Verification Task Object (`CareBenefitVerificationTask__c`)
  
- Custom layouts, record types, and permission sets

- Programmatic metadata fetch:
  ```bash
  sf project retrieve start --metadata "<type>:<name>" --target-org HealthCloudDev
  ```

- Queue-based task routing (queue must be created manually post-deploy)

---

## 🔧 Project Setup Instructions

Follow the steps below to download the Salesforce DX project from GitHub, authorize your org, and deploy metadata using the Salesforce CLI. This process ensures a smooth and reproducible setup experience across development environments.

🧰 Step 1: Clone the GitHub Repo
First, open your terminal and run:

bash
git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>
> Replace <your-username> and <your-repo-name> with your GitHub account name and the project repo name.

🔑 Step 2: Authorize Your Salesforce Org
Use the Salesforce CLI to log in to the org where you want to deploy the metadata:

bash
sf org login web --alias MyTargetOrg
This opens a browser window for you to authenticate.

After logging in, the alias MyTargetOrg can be used in all future commands.

🏗️ Step 3: Set Default Org (Optional but helpful)
If you want to make this your default org for the project:

bash
sf config set target-org MyTargetOrg
📦 Step 4: Deploy the Project Metadata
From your project root directory, deploy all source files:

bash
sf project deploy start --target-org MyTargetOrg
You can also preview the changes first with:

bash
sf project deploy preview --target-org MyTargetOrg
✅ Step 5: Verify Deployment
You can now open the org and verify everything deployed correctly:

bash
sf org open --target-org MyTargetOrg
Navigate to App Launcher and check for custom objects, Lightning components, or other deployed metadata.

### 📝 Notes

- CoverageBenefit record is created via Apex, no need to create it manually
- Ensure all required fields are visible and marked on the layouts
- The MemberPlan object was extended with provider info for proper routing
- Queues and task routing logic are included in `CareBenefitVerifyReqTriggerHandler`

Navigate to App Launcher and check for custom objects, Lightning components, or other deployed metadata.

---

## 🏁 Final Setup Checklist

- [x] Dev Hub authorized  
- [x] Scratch org created  
- [x] Metadata pushed  
- [x] External Credentials configured  
- [x] Permission sets assigned  
- [x] Queue manually created  

---
