 ## Problem Statement:
 You work for XYZ Corporation. Their application requires a storage service that can store files and publicly share them if required. Implement S3 for the same.
 
 Tasks To Be Performed:
 1. Create an S3 Bucket for file storage.
 2. Upload 5 objects with different file extensions.

---
To implement S3 for XYZ Corporation's application and meet the outlined requirements, here's how you can proceed using AWS S3 via the AWS Management Console or programmatically with the AWS CLI or SDKs (like Python with `boto3`).

---

## ✅ Step-by-Step Solution

---

### **Step 1: Create an S3 Bucket**

#### 📌 Using AWS Console:
1. Log in to [AWS Console](https://console.aws.amazon.com/).
2. Go to **S3** service.
3. Click on **Create bucket**.
4. Enter a unique bucket name (e.g., `xyzcorp-file-storage`).
5. Choose a region (e.g., `us-east-1`).
6. Uncheck **Block all public access** if public sharing is needed (can be configured per object later too).
7. Click **Create Bucket**.

---

### **Step 2: Upload 5 Objects with Different File Extensions**

You can upload files via the console or programmatically.

#### 📂 Example File Types:
- `document.pdf`
- `image.jpg`
- `spreadsheet.xlsx`
- `presentation.pptx`
- `script.js`

#### 📌 Using AWS Console:
1. Go to the bucket you just created.
2. Click **Upload**.
3. Add the 5 different files.
4. Optionally set individual object permissions for public access.
5. Click **Upload**.

---

### ✅ What’s Done
- ✅ Created an S3 bucket for file storage.
- ✅ Uploaded 5 objects with different file extensions.
- ✅ Enabled optional public sharing per object.
