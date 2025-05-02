# 📘 AWS VPC and Subnet CIDR Block Allocation - Detailed Notes

## 🧠 Understanding VPC and Subnets

### 🔹 What is a VPC?

A **Virtual Private Cloud (VPC)** is a logically isolated section of the AWS cloud where you can launch AWS resources in a virtual network you define.

* You assign a **CIDR block** (e.g., `10.0.0.0/16`) to your VPC to define the private IP address space.
* The maximum size of a VPC is a `/16` block (i.e., 65,536 IP addresses).

### 🔹 What is a Subnet?

Subnets are subdivisions within a VPC.

* Each **subnet resides in a single Availability Zone**.
* Resources such as EC2 instances are launched within a subnet, not directly into the VPC.

## 📏 CIDR Blocks and IP Addressing

### 🧩 CIDR (Classless Inter-Domain Routing)

CIDR notation defines IP address ranges:

* Format: `<IP>/<prefix-length>`
* Example: `10.0.0.0/16`

  * The first 16 bits (2 bytes) are the network part.
  * Remaining 16 bits (2 bytes) are used for host addresses.

| Prefix | Hosts (IPs) | Notes                                    |
| ------ | ----------- | ---------------------------------------- |
| `/16`  | 65,536      | Max VPC range                            |
| `/24`  | 256         | Common subnet size                       |
| `/28`  | 16          | Smallest allowed subnet                  |
| `/20`  | 4,096       | Used when more IPs are needed per subnet |

> **AWS reserves 5 IPs per subnet** (first 4 and last), so usable IPs = total - 5.

---

## 🧮 Subnet Planning: Examples

### Example 1: VPC with `/16` CIDR block

* VPC: `10.0.0.0/16` (Range: `10.0.0.0 - 10.0.255.255`)
* Total IPs: 65,536

#### Subnets:

* Subnet A: `10.0.0.0/28` (For the CIDR block 10.0.0.0/28, AWS allocates IP addresses from the start of the range.)

  * IPs: `10.0.0.0 - 10.0.0.15`
  * Usable: 11 IPs
* Subnet B: `10.0.0.16/28`

  * IPs: `10.0.0.16 - 10.0.0.31`
  * No overlap allowed
* Subnet C: `10.0.0.32/28`

  * IPs: `10.0.0.32 - 10.0.0.47`
  * And so on...

> Avoid overlapping CIDR blocks between subnets — each private IP must be unique within the VPC.

---

### Example 2: Subnets with More IPs

#### Subnet A: `10.0.0.0/24`

* IPs: `10.0.0.0 - 10.0.0.255`
* Usable: 251 IPs

#### Subnet B: `10.0.1.0/24`

* IPs: `10.0.1.0 - 10.0.1.255`
* Unique and non-overlapping

> If `10.0.0.0/24` is used, don't assign another subnet that overlaps (`10.0.0.128/25` would overlap).

---

## 🧱 Rules for CIDR Blocks in AWS

### ✅ Validations and Best Practices:

* A **VPC CIDR block** must not overlap with other VPCs if they are connected (e.g., via peering).
* A **subnet CIDR block** must be a subset of the VPC CIDR block.
* You **cannot** assign the same CIDR to multiple subnets in the same VPC.
* IP addresses **must be unique within a VPC**, even though they are private and not globally unique.

---

## 📌 CIDR Math Quick Reference

| CIDR | Usable IPs | Bit Mask        | Total IPs | Reserved by AWS |
| ---- | ---------- | --------------- | --------- | --------------- |
| /28  | 11         | 255.255.255.240 | 16        | 5               |
| /24  | 251        | 255.255.255.0   | 256       | 5               |
| /20  | 4,091      | 255.255.240.0   | 4,096     | 5               |

---

## 📦 Planning IP Allocation in Subnets

If you need:

* **More instances per subnet**: Use a smaller subnet mask (e.g., `/24` or `/20`).
* **More subnets**: Use a larger subnet mask (e.g., `/28`, `/27`), at the cost of fewer IPs per subnet.

---

## 🔁 Summary

* Always start with a VPC CIDR (e.g., `/16`).
* Break it into **non-overlapping** smaller CIDR blocks for subnets.
* Plan subnet sizing according to the **number of IPs you need**.
* Be aware of AWS reserving 5 IPs per subnet.
* You cannot edit CIDR blocks for subnets after creation — plan carefully.

---
