# 📘 **AWS VPC Lecture Notes**

## 🗓️ **Session Overview**

This session covered the AWS **Virtual Private Cloud (VPC)** in detail, focusing on default VPCs, their components, CIDR blocks, subnets, routing, and how to create and configure a **custom VPC**. It also addressed platform limitations due to webinar restrictions.

---

## 🧑‍🏫 **Key Administrative Notes**

* The webinar platform is in **listen-only mode** — participants cannot unmute or share screens.
* The instructor recommended **Zoom**, but the platform switch was an **organizational decision**.
* Students are encouraged to **report issues** (e.g., chat disabled, recordings not working).

---

## 🌐 **What is a VPC?**

> A **Virtual Private Cloud (VPC)** is a private, isolated network in the AWS cloud where you can launch AWS resources securely.

### 🔒 **VPC Use Cases**

* Launch secure resources like:

  * EC2 Instances
  * Databases
  * Auto Scaling Groups
* Control access to AWS resources through:

  * Security Groups
  * Network ACLs
  * Custom routing

---

## 🔧 **Core Components of a VPC**

### 1. **CIDR Block**

* CIDR = *Classless Inter-Domain Routing*
* Used to define IP range of the VPC.
* Example ranges for **private IPs**:

  * `10.0.0.0/8`
  * `172.16.0.0/12`
  * `192.168.0.0/16`
* Common setup:

  * VPC: `10.0.0.0/16` (up to 65,536 IPs)
  * Subnets: Smaller blocks like `/24`, `/28` as needed

### 2. **Subnets**

* Logical division of the VPC IP range.
* Can be in **different Availability Zones (AZs)**.
* Two types:

  * **Public Subnet**: Accessible from the internet
  * **Private Subnet**: Internal communication only
* Example Subnet CIDRs from a `/16` VPC:

  * `172.31.0.0/20`
  * `172.31.16.0/20`
  * `172.31.32.0/20`

### 3. **Route Tables**

* Determine how traffic is directed.
* Each VPC has a **main route table** by default.
* A **subnet can be associated with only one route table**, but a route table can be shared across subnets.

#### 🔁 Default Route Table

* Contains routes like:

  * `172.31.0.0/16` → Local
  * `0.0.0.0/0` → Internet Gateway (if public)

### 4. **Internet Gateway (IGW)**

* Allows communication between VPC and internet.
* Must be explicitly attached to a VPC.
* **One-to-one** relationship with VPC.

### 5. **Network ACLs**

* Optional layer of security for subnets.
* Controls **inbound/outbound traffic** at the subnet level.

---

## 🧰 **Default VPC Behavior**

* Created automatically in each region.
* Comes with:

  * Default subnets (one per AZ)
  * Default route table
  * Internet gateway
* Uses CIDR: `172.31.0.0/16`
* **Public subnets** with `Auto-assign Public IP = Yes`

---

## 📌 **Best Practices**

* **Do not delete the default VPC** – if deleted, must contact AWS support to restore.
* **Avoid modifying** default VPC configurations (used for quick testing).
* **Always name your resources** for clarity and organization.
* For production environments: **Use Custom VPCs** for better control and security.

---

## 🧪 **Lab Exercise Preview: Creating a Custom VPC**

### Objective:

* Create a custom VPC and configure it with:

  * Two subnets (one in AZ-a, one in AZ-b)
  * One **public**, one **private** subnet
  * Custom CIDR block (e.g., `10.0.0.0/16`)
  * Internet Gateway
  * Custom route tables
  * Two EC2 instances (one in each subnet)

---

## 💡 **Important Concepts Recap**

| Term             | Description                                      |
| ---------------- | ------------------------------------------------ |
| CIDR             | Defines IP range (e.g., `/16`, `/24`)            |
| VPC              | Virtual private network in AWS                   |
| Subnet           | Subdivision of VPC; defines a smaller IP range   |
| Route Table      | Defines how traffic flows inside/outside the VPC |
| Internet Gateway | Allows traffic from VPC to the internet          |
| Public Subnet    | Has route to IGW and assigns public IPs          |
| Private Subnet   | No direct internet access                        |
| Default VPC      | AWS-provided; convenient for quick testing       |

---

## ❓ **Q\&A Highlights**

* **Can we modify the default VPC?**

  * Technically yes, but not recommended.
* **How many IPs in a `/20` subnet?**

  * 4,096 IPs, but 5 are reserved, so **4,091 usable**.

---
