## Problem Statement:
 You work for XYZ Corporation. Their application requires a storage service that can store files and publicly share them if required. Implement S3 for the same.
 
 Tasks To Be Performed:
 1. Use the created bucket in the previous task to host static websites, upload an index.html file and error.html page.
 2. Add a lifecycle rule for the bucket:
 - a. Transition from Standard to Standard-IA in 60 days
 - b. Expiration in 200 days
---
Let's complete the remaining S3 tasks for XYZ Corporation — **hosting a static website** and **adding a lifecycle rule**.

---

## ✅ Task 1: Host a Static Website Using the Existing S3 Bucket

### 📝 Steps (Using AWS Console):

1. **Go to S3** and open the bucket you created earlier (e.g., `xyzcorp-file-storage`).
2. Click on the **Properties** tab.
3. Scroll down to **Static website hosting**.
4. Click **Edit**, then:
   - Choose **Enable**.
   - **Index document**: `index.html`
   - **Error document**: `error.html`
5. Click **Save changes**.

---

### 📁 Upload `index.html` and `error.html`

1. Go to the **Objects** tab in your bucket.
2. Click **Upload**.
3. Add `index.html` and `error.html` from your local machine.
4. **Set permissions** if you want them to be publicly accessible:
   - Under **Permissions**, uncheck "Block all public access".
   - Set **public-read** for these two files.

---

### 🌐 Make Files Public (optional for website to work):

1. Go to each uploaded file (`index.html`, `error.html`).
2. Click **Actions > Make public** (or set proper bucket policy).

---

### 🔗 Get Website URL

1. Go back to the **Properties > Static website hosting** section.
2. Copy the **Bucket website endpoint**.
3. Open it in your browser — it should show the content of `index.html`.

---

## ✅ Task 2: Add a Lifecycle Rule

### 🧾 Lifecycle Rule Requirements:
- Transition to **Standard-IA** (Infrequent Access) after **60 days**.
- **Delete** (expire) objects after **200 days**.

---

### 📝 Steps (Using AWS Console):

1. Go to your S3 bucket > **Management** tab.
2. Click **Create lifecycle rule**.
3. Rule name: `TransitionAndExpire`
4. Choose: **This rule applies to all objects in the bucket**
5. Click **Next**.
6. Under **Transitions**:
   - Add transition to **Standard-IA** after **60 days**.
7. Under **Expiration**:
   - Set **Expire current versions** after **200 days**.
8. Click **Next**, then **Create rule**.

---

### ✅ Summary

| Task                         | Status       |
|------------------------------|--------------|
| Static website hosting setup | ✅ Completed |
| `index.html` and `error.html` uploaded | ✅ Completed |
| Lifecycle rule (60 days to IA, 200 days expiration) | ✅ Completed |

---
