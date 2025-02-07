# **Bitbucket to GitHub Sync via Bitbucket Pipelines**  

## **Overview**  
This project automates the synchronization of a Bitbucket repository to GitHub using Bitbucket Pipelines. Any change in the Bitbucket repository will automatically trigger an update in the corresponding GitHub repository without manual intervention.  

## **Prerequisites**  
Before starting, ensure you have:  
- A Bitbucket repository with admin access  
- A GitHub account with repository creation permissions  
- SSH and Git installed on your local machine  
- Basic knowledge of CI/CD pipelines  

---

## Project Visualization:

![image](https://github.com/user-attachments/assets/eac00f6b-b4c9-45ab-8af0-c9c99c7589e1)

---


## **Step-by-Step Guide**  

### **1. Configure Bitbucket**  

#### **1.1 Create an Access Token**  
1. Navigate to **Repository settings** > **Security** > **Access Tokens**  
2. Click on **Create Access Token**  
3. Grant **Read** permissions to:  
   - Repository  
   - Webhooks  
4. Copy and securely store the generated token.  

#### **1.2 Enable Bitbucket Pipelines**  
1. Go to **Repository settings** > **Pipelines**  
2. Under **Settings**, toggle the button to enable Pipelines.  

#### **1.3 Generate SSH Keys and Configure Known Hosts**  
1. On your local machine, generate an SSH key pair:  

2. Copy the public key from the console.

---

## 2. Add SSH Public
2.1 Add SSH Public Key to Bitbucket
Go to Repository settings > Security > Access Keys
Click Add Key
Paste the SSH public key
Save the changes.

2.2 1.5 Configure Repository Variables
Navigate to Repository settings > Repository Variables

Add the following variables:

| Name	             | Value                         |
| ------------------ | ----------------------------  |
| BITBUCKET_VARIABLE |	(Access Token from Step 1.1) |
| GITHUB_VARIABLE	   |  (GitHub Personal Access Token)|

---

## 3. Configure GitHub

## 3.1 Create a New Empty Repository
1. Go to GitHub
2. Click on New Repository
3. Do not initialize with a README file
4. Copy the repository URL

## 3.2 Add SSH Deploy Key
1. Go to Settings > Security > Deploy Keys
2. Click Add Key
3. Paste the public SSH key generated in Bitbucket Step 1.3
4. Check Allow write access
5. Save the changes.
   
## 3.3 Generate a GitHub Personal Access Token (PAT)
1. Go to GitHub Developer Settings
2. Navigate to Personal Access Tokens > Generate New Token
3. Grant the following permissions:
- `repo` (Full control of repositories)
- `workflow` (Trigger workflows)
- `write:packages` (Push access for packages)
4. Generate and securely store the PAT.

---

# 4. Configure Bitbucket Pipeline

## 4.1 Create `bitbucket-pipelines.yml`
In your Bitbucket repository, create a `.yml` file in the root directory:

```
yaml

pipelines:
    default:
      - step:
          name: Sync GitHub Mirror
          image: alpine/git:latest
          clone:
            enabled: false
          script:
            - git clone --mirror https://x-token-auth:"$BITBUCKET_VARIABLE"@bitbucket.org/succpinnsolutions/ecommerce_app.git 
            - cd ecommerce_app.git ## cd followed by your Github repository Name
            - git push --mirror https://x-token-auth:"$GITHUB_VARIABLE"@github.com/felix-momodebe-official/bitbucket_github-sync.git 
```
Replace:

- `bitbucket.org/succpinnsolutions/ecommerce_app.git` with your actual Bitbucket repository name
- `github.com/felix-momodebe-official/bitbucket_github-sync.git` with your actual GitHub repository name

---

# 5. Test the Pipeline
1. Commit and push the bitbucket-pipelines.yml file to Bitbucket.
2. Navigate to Bitbucket Repository > Pipelines
3. Manually trigger the pipeline or push a change to test.
4. Verify that the repository is correctly mirrored to GitHub.

---

# Troubleshooting

## Issue: Pipeline Fails Due to SSH Key Permission Issues
- ✅ Ensure the **public SSH key** is added to **GitHub Deploy Keys** with write access.
- ✅ Check that **Bitbucket Access Keys** contains the correct key.

## Issue: Git Push Fails Due to Authentication
- ✅ Verify the GitHub Personal Access Token (PAT) has the correct permissions (`repo`, `workflow`, `write:packages`).
- ✅ Ensure `GITHUB_VARIABLE` in Bitbucket contains the correct PAT value.

## Issue: Pipeline Does Not Trigger on Changes
- ✅ Confirm that **Bitbucket Pipelines** is enabled.
- ✅ Ensure `.yml` syntax is correct in `bitbucket-pipelines.yml`.

---

# Conclusion
By following this setup, any change in your **Bitbucket repository** will be automatically synced to **GitHub**, eliminating manual intervention. This process ensures your repositories remain consistent across platforms.

## 🚀 Happy DevOps! 🚀

---




