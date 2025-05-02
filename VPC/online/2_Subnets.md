# 🌐 Understanding AWS VPC (Virtual Private Cloud)

## 🌍 VPC and Availability Zones

* A **Region** consists of multiple **Availability Zones (AZs)**.
* Each **AZ** is a group of **one or more data centers** with redundant power, networking, and connectivity.
* A **VPC spans across all AZs** in a region.
* However, a VPC only takes a **portion** of each AZ (not the entire AZ) — like slicing a big pie into small virtual slices.

---

## 🚀 Why VPC is Important?

Before VPC, AWS had **EC2-Classic**, where all instances were in a **shared network**.

* This posed serious security issues, as instances could potentially talk to each other if not secured.
* VPC allows **network isolation**, **segmentation**, and **fine-grained control**.

---

## 🧱 Structure of a VPC

```
Region
 └── VPC (Virtual Private Cloud)
      └── Availability Zones (A, B, C...)
           └── Subnets (Public / Private)
                └── EC2 Instances / Resources
```

* **Subnets** are smaller subdivisions of an AZ.
* You create resources (like EC2s) **inside subnets**, **not directly in an AZ**.
* **Default VPCs** are automatically created in every region for new AWS accounts.

---

## 🔁 Subnets: Public vs Private

### 📤 Public Subnet

* Resources (e.g., EC2) **can access the internet**.
* Requires:

  * Internet Gateway
  * Route Table entry to the internet

### 🔒 Private Subnet

* Resources **cannot access the internet directly**.
* Used for:

  * Databases
  * Internal services
  * App backend

### Use Case Example: Tiered Application

| Tier      | Subnet Type | Access         |
| --------- | ----------- | -------------- |
| Web Layer | Public      | Internet       |
| App Layer | Private     | Web Layer only |
| DB Layer  | Private     | App Layer only |

---

## 🛡️ Security in VPC

### 🔐 Security Group

* Acts like a **firewall at the instance level**.
* Controls **inbound/outbound traffic**.

### 🚧 Network ACL (Access Control List)

* Acts like a **firewall at the subnet level**.
* Controls **inbound/outbound traffic for subnets**.
* Useful for **DDoS protection** or **blocking specific IPs**.

### 🔄 Communication Rules

* Instances within a VPC **can communicate** with each other **by default**.
* Resources **cannot communicate across VPCs** unless:

  * **VPC Peering**
  * **Transit Gateway**
  * **VPN or Direct Connect**

---

## 🏛️ Classic EC2 vs VPC

| Feature       | EC2-Classic (Deprecated) | VPC                   |
| ------------- | ------------------------ | --------------------- |
| Network Scope | Shared across accounts   | Isolated per account  |
| IP Control    | No                       | Yes                   |
| Security      | Low                      | High (Subnets, NACLs) |
| Availability  | Limited                  | Multi-AZ support      |

---

## ✅ Best Practices

* Always have **at least one public subnet** to allow admin access (e.g., SSH).
* Place sensitive resources (DBs, backend services) in **private subnets**.
* Use **security groups for fine-grained control** at instance level.
* Use **NACLs for subnet-level restrictions** (e.g., block bad IPs).
* Distribute instances across **multiple AZs** for high availability.
* Use **VPC Peering** or **Transit Gateway** for cross-VPC communication.
* Log and monitor with **VPC Flow Logs**.

---
