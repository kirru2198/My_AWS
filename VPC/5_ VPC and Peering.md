Problem Statement:
You work for XYZ Corporation and based on the expansion requirements of your corporation you have been asked to create and set up a distinct Amazon VPC for the production and development team. You are expected to perform the following tasks for the respective VPCs.
 
 Production Network:
 1. Design and build a 4-tier architecture.
 2. Create 5 subnets out of which 4 should be private named app1, app2, dbcache and db and one should be public, named web.
 3. Launch instances in all subnets and name them as per the subnet that they have been launched in.
 4. Allow dbcache instance and app1 subnet to send internet requests.
 5. Manage security groups and NACLs.
 
 Development Network:
 1. Design and build 2-tier architecture with two subnets named web and db and launch instances in both subnets and name them as per the subnet names.
 2. Make sure only the web subnet can send internet requests.
 3. Create peering connection between production network and development network.
 4. Setup connection between db subnets of both production network and development network respectively.

---
Here's a **complete guide** to implement the required **VPC architecture** and networking components for **XYZ Corporation**, covering **Production** and **Development networks**, instance setup, internet access control, and secure communication.

---

## 🧭 OVERVIEW OF TASKS

### 🏭 Production Network
- Create a VPC with **5 subnets**:  
  - **1 Public**: `web`  
  - **4 Private**: `app1`, `app2`, `dbcache`, `db`
- Create a **4-tier architecture**
- **Allow only app1 and dbcache** to send internet traffic
- Create **EC2 instances** in each subnet
- Manage **security groups** and **NACLs**

---

### 🛠 Development Network
- Create a VPC with **2 subnets**:  
  - `web` (public or with egress)  
  - `db` (private)
- Only **web subnet** can send traffic to the internet
- Create **EC2 instances** and name them based on the subnet
- Create **VPC peering** between production and development
- Setup **connectivity between db subnets** securely

---

## 🧱 STEP-BY-STEP IMPLEMENTATION

---

### ✅ **1. Create Production VPC**

- VPC CIDR: `10.0.0.0/16`
- Create 5 subnets:
  - `web` (Public) – `10.0.0.0/24`
  - `app1` – `10.0.1.0/24`
  - `app2` – `10.0.2.0/24`
  - `dbcache` – `10.0.3.0/24`
  - `db` – `10.0.4.0/24`

- Attach **Internet Gateway** to the VPC

- Route table configuration:
  - **Public Route Table**:
    - Associate with `web` subnet
    - Route to `0.0.0.0/0` via IGW
  - **Private Route Table (with NAT)**:
    - Create a NAT Gateway in `web` subnet
    - Associate with `app1` and `dbcache` subnets
    - Route `0.0.0.0/0` → NAT Gateway
  - `app2` and `db` subnets → **No internet access**

---

### ✅ **2. Launch EC2 Instances in Production VPC**

Launch EC2 instances (e.g., Amazon Linux) in each subnet and name them:

| Subnet   | EC2 Instance Name |
|----------|-------------------|
| web      | web               |
| app1     | app1              |
| app2     | app2              |
| dbcache  | dbcache           |
| db       | db                |

- Use **key pair** for SSH
- Allocate Elastic IP for the NAT Gateway (not for private instances)

---

### ✅ **3. Security Groups for Production**

- **web-sg**:
  - Inbound: SSH, HTTP, HTTPS from the internet
- **app1/app2/dbcache/db-sg**:
  - Inbound: Allow from specific subnets or instances as needed
  - Outbound: Allow all (by default)

### ✅ **NACLs (Optional Advanced Control)**

- Default NACL is usually enough; if more granular control is needed:
  - Allow traffic on necessary ports per subnet
  - Block unnecessary public access to `db`, `app2`

---

### ✅ **4. Create Development VPC**

- CIDR: `10.1.0.0/16`
- Create 2 subnets:
  - `web` – `10.1.0.0/24`
  - `db` – `10.1.1.0/24`

- Create an **Internet Gateway** and attach to this VPC
- Public Route Table:
  - Associate with `web` subnet
  - Add `0.0.0.0/0` route to IGW
- `db` subnet:
  - No internet route

---

### ✅ **5. Launch EC2 Instances in Development**

| Subnet | EC2 Instance Name |
|--------|-------------------|
| web    | web               |
| db     | db                |

---

### ✅ **6. VPC Peering Between Production and Development**

- Go to **VPC Dashboard** → **Peering Connections** → **Create Peering**
  - Requester: **Production VPC (10.0.0.0/16)**
  - Accepter: **Development VPC (10.1.0.0/16)**

- Accept peering request from Development side

- Update Route Tables in both VPCs:
  - In Production VPC:
    - Add `10.1.0.0/16` → Peering connection
  - In Development VPC:
    - Add `10.0.0.0/16` → Peering connection

---

### ✅ **7. Allow Connectivity Between DB Subnets**

- Production DB subnet: `10.0.4.0/24`
- Development DB subnet: `10.1.1.0/24`

#### Security Group Rules:
- In **db (Production) EC2** security group:
  - Allow inbound TCP (e.g., port 3306 for MySQL) from `10.1.1.0/24`
- In **db (Development) EC2** security group:
  - Allow inbound TCP from `10.0.4.0/24`

---

## 🔒 Security Recap

- Only `web` subnets have direct internet access
- NAT Gateway is used for secure **outbound internet** for selected subnets (`app1`, `dbcache`)
- No public access to `db` or `app2`
- Peering allows **private** communication between production and development
- DB subnets are tightly controlled using **security groups**

---

