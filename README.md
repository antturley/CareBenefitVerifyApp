```markdown
# Care Benefit Verification App

A Salesforce Health Cloud extension that connects to the Benefits Verification Service, enabling streamlined eligibility workflows directly within Salesforce. This managed package includes the full data model, trigger handlers, Apex classes, permission sets, queue-driven task generation, and external credential support.

---

## 🚀 Setup and Deployment Guide

This guide walks you from environment setup to deploying and customizing the Care Benefit Verification App in a scratch org.

---

### 🛠 Prerequisites

- [Salesforce CLI](https://developer.salesforce.com/tools/sfdxcli)
- [Visual Studio Code](https://code.visualstudio.com/)
- Salesforce Extensions for VS Code
- A [Developer Edition org](https://developer.salesforce.com/signup) with Dev Hub enabled

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
    Package Id │ 0HogL0000000FOrSAM
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

### 📝 Notes

- CoverageBenefit record is created via Apex, no need to create it manually
- Ensure all required fields are visible and marked on the layouts
- The MemberPlan object was extended with provider info for proper routing
- Queues and task routing logic are included in `CareBenefitVerifyReqTriggerHandler`

---

## 👨‍🔬 Contributing

Feel free to fork, create issues, or submit pull requests for enhancements. For questions or deeper documentation, open a GitHub issue or start a discussion.

---

## 🏁 Final Setup Checklist

- [x] Dev Hub authorized  
- [x] Scratch org created  
- [x] Metadata pushed  
- [x] External Credentials configured  
- [x] Permission sets assigned  
- [x] Queue manually created  

---

> _Inspired and structured in accordance with Salesforce best practices for health care integrations._
```

---

Let me know if you'd like a second section with test coverage steps or a diagram of the data model/task flow. This README is clean, developer-focused, and will scale well with contributions.