# 🚀 End-to-End Salesforce DevOps CI/CD Pipeline

An end-to-end Salesforce DevOps CI/CD pipeline using **Salesforce DX (SFDX)**, **JWT-based OAuth Authentication**, and **GitHub Actions** for fully automated deployment to a UAT Org — no manual login required.

---

## 🏗️ Architecture

```
![Salesforce DevOps Architecture](architecture/Pasted%20image.png)
```

---

## 🔐 Authentication (JWT Flow)

- Connected App created in UAT
- Digital certificate uploaded
- Admin pre-authorized user access
- GitHub Secrets configured:

| Secret | Description |
|---|---|
| `SF_CLIENT_ID` | Connected App Consumer Key |
| `SF_USERNAME` | Salesforce admin username |
| `SF_JWT_KEY` | Private key for JWT authentication |

---

## ⚙️ GitHub Actions Workflow

```yaml
name: Salesforce CI/CD

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Install SFDX CLI
        run: npm install -g sfdx-cli

      - name: Authenticate via JWT
        run: |
          echo "${{ secrets.SF_JWT_KEY }}" > server.key
          sfdx force:auth:jwt:grant \
            --clientid ${{ secrets.SF_CLIENT_ID }} \
            --jwtkeyfile server.key \
            --username ${{ secrets.SF_USERNAME }} \
            --instanceurl https://login.salesforce.com

      - name: Deploy Metadata to UAT
        run: sfdx force:source:deploy -p force-app -u ${{ secrets.SF_USERNAME }}
```

---

## 📦 Metadata Included

- Custom Objects
- Custom Fields
- Record-Triggered Flows
- Page Layouts

### Example Automation Logic

```
IF Credit Score > 700 AND Loan Amount < 5,000,000
  → Set Status = "Approved"
```

---

## 🎯 Outcome

- ✅ Secure, token-based CI/CD for Salesforce
- ✅ Automated UAT deployment on every push to `main`
- ✅ Git-based metadata version control
- ✅ Enterprise-style DevOps workflow

---

## 👨‍💻 Author

**Sarfraj Khan**

> Open to **DevOps Internship** Opportunities  
> Feel free to connect or reach out!