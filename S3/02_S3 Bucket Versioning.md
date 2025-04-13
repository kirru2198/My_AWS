 Problem Statement:
 You work for XYZ Corporation. Their application requires a storage service that can store files and publicly share them if required. Implement S3 for the same.
 
 Tasks To Be Performed:
 1. Enable versioning for the bucket created in task 1.
 2. Re-upload any 2 files already uploaded to verify if versioning works

---

## ✅ Task: Enable Versioning and Verify with Re-Upload

---

### **Step 1: Enable Versioning on the S3 Bucket**

#### 📌 Using AWS Console:
1. Go to the **S3** service in the AWS console.
2. Click on the bucket you created earlier (e.g., `xyzcorp-file-storage`).
3. Click on the **Properties** tab.
4. Scroll down to **Bucket Versioning**.
5. Click **Edit**, then choose **Enable**.
6. Click **Save changes**.

> ✅ Versioning is now turned on, which means every time you upload a file with the same name, a **new version** is stored instead of replacing the old one.

---

### **Step 2: Re-upload Any 2 Files**

#### 📌 Using AWS Console:
1. Go back to the **Objects** tab of your bucket.
2. Choose two files (e.g., `document.pdf` and `image.jpg`) that you already uploaded.
3. Upload new versions of those two files **with the same file name**.
4. AWS will automatically save both versions (old and new).

---

### ✅ How to Verify Versioning:

1. Click on the file name (e.g., `document.pdf`) in the bucket.
2. Click on **Versions** (you'll see a list of all versions with timestamps and version IDs).
3. You should see at least **two versions** of the file now.

---

### ✅ Summary

- ✅ Enabled versioning on the bucket.
- ✅ Re-uploaded 2 files with the same names to create new versions.
- ✅ Verified versioning is working by checking the version history.
