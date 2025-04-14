 Problem Statement:
 Working for an organization, you are required to provide them a safe and secure environment for the deployment of their resources. They might require different types of connectivity. Implement the following to fulfill the requirements of the company.
 
 Tasks To Be Performed:
 1. Create a VPC endpoint for a S3 bucket of your choice for secure access to the files.

---

To **securely access an S3 bucket from within your VPC without traversing the internet**, you can create a **VPC Endpoint for S3**. This ensures traffic between your VPC and Amazon S3 **stays within the AWS network**, providing enhanced **security, performance, and cost-efficiency**.

---

## ✅ **Goal**

- Create a **VPC endpoint for S3** to securely access a specific S3 bucket from your VPC
- Prevent public internet traffic

---

## 🧱 **Step-by-Step Guide (AWS Console)**

### ✅ 1. **Choose or Create Your VPC**
Make sure you have a VPC where your resources (like EC2) are running. Note the **VPC ID** and **subnet IDs**.

---

### ✅ 2. **Create a Gateway VPC Endpoint for S3**

1. Go to **VPC Dashboard** → **Endpoints** → **Create Endpoint**
2. Fill in:
   - **Service category:** AWS services
   - **Service name:** Find `com.amazonaws.<region>.s3`
     - Example: `com.amazonaws.us-east-1.s3`
   - **VPC:** Select your VPC
   - **Route Tables:** Select the route table(s) associated with your VPC’s subnets
     - This adds a route to S3 using the endpoint
   - **Policy:** Choose **Full access** or customize if needed
3. Click **Create endpoint**

✅ Done — your VPC can now access **all S3 buckets** securely.

---

### ✅ 3. (Optional but Recommended) **Restrict Access to Specific S3 Bucket**

To limit access to **only one S3 bucket**, do the following:

#### a. Edit the **S3 bucket policy**:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAccessFromVPCE",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::your-bucket-name",
        "arn:aws:s3:::your-bucket-name/*"
      ],
      "Condition": {
        "StringNotEquals": {
          "aws:SourceVpce": "vpce-xxxxxxxxxxxxxxxxx"
        }
      }
    }
  ]
}
```

> Replace `your-bucket-name` and `vpce-xxxxxxxxxxxxx` with your actual values.

✅ Now your bucket can **only be accessed via the VPC endpoint**, and **not via the internet**.

---

## 🔒 Final Outcome

- Your EC2 in private subnets can access S3
- No need for internet/NAT Gateway
- S3 access is **secure**, **fast**, and **cost-efficient**

---
