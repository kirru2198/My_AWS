 Problem Statement:
 You work for XYZ Corporation. Their application requires a storage service that can store files and publicly share them if required. Implement S3 for the same.
 
 While migrating, you are asked to perform the following tasks:
 1. Ensure that any amount of data can be stored on the cloud and can be retrieved at any time from anywhere on the web.
 2. Manage the lifecycle of data that is being stored on the cloud so that it gets automatically deleted after 75 days.
 3. Retrieve the old version of a file if the content of the current version of that file is compromised accidently.
 4. Host your static website on the AWS cloud using the domain name created in Module 3 assignment.
 5. Display an error page if proper domain name is not used while attempting to access the company’s website.

---
Let's break down the tasks step by step for XYZ Corporation's application in AWS. The tasks involve S3 storage, data lifecycle management, versioning, static website hosting, and handling errors in a web application.

---

## **Task 1: Ensure that any amount of data can be stored on the cloud and can be retrieved at any time from anywhere on the web.**

### Solution:
To meet this requirement, AWS S3 (Simple Storage Service) is ideal. It offers scalable, secure, and highly available storage for any amount of data.

#### Steps:
1. **Create an S3 Bucket** to store data:
   - Go to the **S3** service in the AWS Management Console.
   - Click **Create bucket**.
   - Choose a globally unique bucket name (e.g., `xyzcorp-file-storage`).
   - Select a region.
   - Leave other settings as default unless customization is needed (e.g., public access settings for public files).
   - Click **Create bucket**.

2. **Upload data** to the bucket:
   - Go to the **Objects** tab within the bucket.
   - Click **Upload**, select files from your local machine, and upload them to the bucket.
   - You can store unlimited data, and S3 will handle the scaling.

3. **Access the data from anywhere**:
   - By default, S3 allows data to be accessed over the web. You can use the S3 bucket URL or set permissions to make the data publicly accessible.

> 📌 **Note**: S3 provides 99.999999999% (11 9's) durability and is accessible via HTTP(S) endpoints globally.

---

## **Task 2: Manage the lifecycle of data that is being stored on the cloud so that it gets automatically deleted after 75 days.**

### Solution:
AWS S3 provides **Lifecycle Policies** to manage your objects automatically.

#### Steps:
1. Go to the **Management** tab of the bucket.
2. Click **Create lifecycle rule**.
3. Provide a **Rule name** (e.g., `DeleteAfter75Days`).
4. Under **Lifecycle rule scope**, select **This rule applies to all objects**.
5. Click **Next**.
6. Under **Expiration**, set:
   - **Expire current versions of objects** after **75 days**.
7. Click **Create rule**.

> 📌 This rule will automatically delete objects after 75 days, reducing manual management and ensuring that old data is removed in a timely manner.

---

## **Task 3: Retrieve the old version of a file if the content of the current version of that file is compromised accidentally.**

### Solution:
This requires **Versioning** to be enabled on the S3 bucket. When versioning is enabled, S3 will store all versions of a file.

#### Steps to Enable Versioning:
1. Go to the **Properties** tab of the bucket.
2. Scroll down to the **Bucket Versioning** section.
3. Click **Edit**, then choose **Enable**.
4. Click **Save changes**.

#### Retrieve Previous Versions:
1. In the **Objects** tab of your bucket, click on the file you wish to retrieve the old version of.
2. Click **Versions** to see a list of all versions.
3. Click on the version you need and download it.

> 📌 **Versioning** helps to prevent accidental data loss and allows retrieval of earlier versions of your files.

---

## **Task 4: Host your static website on the AWS cloud using the domain name created in Module 3 assignment.**

### Solution:
To host a static website, you'll need to use S3 for web hosting and configure Route 53 for the custom domain.

#### Steps:
1. **Enable Static Website Hosting** on S3:
   - Go to the **Properties** tab of your S3 bucket.
   - Scroll down to **Static website hosting**.
   - Choose **Enable**.
   - Set the **Index document** (e.g., `index.html`).
   - Set the **Error document** (e.g., `error.html`).
   - Click **Save changes**.

2. **Create a Route 53 hosted zone**:
   - If you already have a domain name (e.g., `xyzcorp.com`) from Module 3, create a **hosted zone** in AWS Route 53.
   - Go to Route 53 in the AWS console.
   - Click **Create hosted zone** and add your domain name.
   
3. **Update DNS Records**:
   - Create a **Record Set** for your domain (e.g., `www.xyzcorp.com`).
   - Set the **Type** to **A – IPv4 address**.
   - Choose **Alias** as **Yes**.
   - For **Alias Target**, select the S3 bucket’s static website endpoint.
   - Save the record.

4. **Access your website**:
   - Your website should now be accessible using your custom domain (e.g., `www.xyzcorp.com`).

---

## **Task 5: Display an error page if a proper domain name is not used while attempting to access the company’s website.**

### Solution:
You can configure your S3 static website to serve a custom error page when the domain is not used correctly.

#### Steps:
1. **Create a Custom Error Page**:
   - Create an HTML file, `error.html`, that contains the error message (e.g., "Please use the correct domain name: www.xyzcorp.com").
   
2. **Upload the Error Page**:
   - Upload this `error.html` file to your S3 bucket in the **Objects** tab.

3. **Set the Error Page in S3 Static Hosting**:
   - Go to the **Properties** tab of the S3 bucket.
   - Under **Static website hosting**, make sure that the **Error document** is set to `error.html`.

4. **Redirect Users to Error Page**:
   - If someone tries to access the website using an incorrect domain (e.g., `xyzcorp.com` instead of `www.xyzcorp.com`), they will be redirected to the `error.html` page.

> 📌 **Tip**: To ensure your website only works with the correct domain (e.g., `www.xyzcorp.com`), configure a redirect in your DNS or S3 bucket settings to redirect users from the non-www domain to the www domain.

---

## ✅ Summary of Tasks Completed:

| Task                                                                 | Status       |
|----------------------------------------------------------------------|--------------|
| Ensure that data can be stored and retrieved globally                | ✅ Completed |
| Manage data lifecycle and automatically delete after 75 days         | ✅ Completed |
| Retrieve old version of files if compromised                         | ✅ Completed |
| Host static website using a custom domain from Route 53              | ✅ Completed |
| Display an error page if the correct domain is not used              | ✅ Completed |

---
