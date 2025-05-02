# 🛠️ VPC Components and Routing – Notes

## 🌍 Availability Zones and Subnets

* A VPC spans **multiple Availability Zones (AZs)**.
* Each AZ typically contains **at least one subnet**.
* You can create **multiple subnets per AZ**, based on your design.
* **Subnets** are parts of your network — they divide your VPC into smaller segments.

---

## 🔁 Implicit Router

* AWS **automatically creates** a **router** (called an **implicit router**) when a VPC is created.
* You **cannot modify or delete** it — it’s managed by AWS.
* This router is the **central traffic controller** within your VPC.

### 🚦 Router Responsibilities:

* Routes traffic **between subnets** within a VPC.
* Forwards traffic **to/from the internet** via the Internet Gateway.
* Acts **like your home Wi-Fi router**, directing traffic in/out.

---

## 🌐 Internet Gateway (IGW)

* A **gateway that allows your VPC to connect to the internet**.
* Must be **explicitly attached** to your VPC.
* Without an IGW, **internet-bound traffic cannot leave** the VPC.

### 🧭 Internet Access Flow:

```text
EC2 Instance → Subnet → Implicit Router → Route Table → Internet Gateway → Internet
```

---

## 🗺️ Route Tables

* A **route table** contains a set of rules (routes) that tell the router where to send network traffic.
* Every **subnet must be associated** with a route table.
* You can associate:

  * **One route table per subnet**
  * **Multiple subnets with the same route table**

### 🧾 Route Table Details:

* Contains **routes** like:

  * Local traffic (`10.0.0.0/16`) stays within the VPC.
  * Internet-bound traffic (`0.0.0.0/0`) goes to IGW.
* You can **edit** or **create custom** route tables as needed.
* A **default route table** is automatically created with every VPC.

---

## 🔄 Internal Traffic Flow

* **Within the same subnet**: traffic flows directly (no router needed).
* **Across subnets in the same VPC**: traffic is routed via the **implicit router**.
* **Across AZs**: still internal to the VPC, routed via the router.

---

## 🔐 Security Groups and NACLs

* **Security Groups**:

  * Act like a virtual firewall at the **instance level**.
  * Define inbound/outbound rules per instance.
* **Network ACLs**:

  * Work at the **subnet level**.
  * Allow or deny traffic in/out of the subnet.
  * Useful for **DDoS protection**, **blocking IPs**, etc.

---

## 🧠 Summary of Key Concepts

| Component         | Description                                 |
| ----------------- | ------------------------------------------- |
| VPC               | Isolated network in AWS                     |
| Availability Zone | AWS Data Center within a region             |
| Subnet            | Segment within a VPC                        |
| Implicit Router   | AWS-managed router, always created with VPC |
| Internet Gateway  | Gateway to the public internet              |
| Route Table       | Routing rules for a subnet                  |
| Security Group    | Controls traffic at the instance level      |
| NACL              | Controls traffic at the subnet level        |

---

## 📌 Final Notes

* **One router per VPC** (implicit and managed by AWS).
* **Multiple route tables** can exist, but only **one can be associated per subnet** at a time.
* Traffic within the VPC uses the router to move between subnets.
* Route tables **guide the router** on where to send traffic, but **routing is physically done** by the router.

---
