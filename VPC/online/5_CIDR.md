# 📘 **VPC and IP Addressing – Detailed Notes**

## 🧠 **Overview**

In this session, we revisited **IP addressing** concepts with a focus on how **CIDR (Classless Inter-Domain Routing) blocks** define ranges of private IPs in a **Virtual Private Cloud (VPC)** in AWS.

---

## 📌 **1. Private IP Address Ranges**

AWS allows use of private IPs from the following ranges as per RFC 1918:

| Range            | Size           | Notes        |
| ---------------- | -------------- | ------------ |
| `10.0.0.0/8`     | 16,777,216 IPs | Large range  |
| `172.16.0.0/12`  | 1,048,576 IPs  | Medium range |
| `192.168.0.0/16` | 65,536 IPs     | Small range  |

These are **non-routable** on the internet and are commonly used inside private networks (e.g., within VPCs).

---

## 🧮 **2. IP Address Structure**

IPv4 addresses consist of **4 bytes (32 bits)**:

```
10.0.0.0 ➝ [8 bits].[8 bits].[8 bits].[8 bits] = 32 bits
```

Each byte can have values from **0 to 255** (2⁸ possibilities), so:

* `10.0.0.0/8` means **first 8 bits (first byte)** are fixed ➝ 10
* Remaining 24 bits can vary ➝ huge number of combinations

---

## 📘 **3. CIDR Block Notation Explained**

CIDR notation format: `IP_Address/Prefix_Length`

* The **prefix length** indicates how many bits are fixed.
* The **remaining bits** determine how many IP addresses are possible.

| CIDR  | Fixed Bits | Free Bits | Total IPs (2ⁿ) |
| ----- | ---------- | --------- | -------------- |
| `/16` | 16         | 16        | 65,536         |
| `/24` | 24         | 8         | 256            |
| `/28` | 28         | 4         | 16             |
| `/30` | 30         | 2         | 4              |
| `/32` | 32         | 0         | 1              |

> For example: `10.0.0.0/24` means:
>
> * IPs range from `10.0.0.0` to `10.0.0.255`
> * Total of `256` IPs

---

## 🛠️ **4. Calculating Number of IPs in a CIDR Block**

**Formula**:

```
Total IPs = 2^(32 - Prefix_Length)
```

**Examples**:

* `/16`: 2^(32 - 16) = **65,536**
* `/24`: 2^(32 - 24) = **256**
* `/28`: 2^(32 - 28) = **16**
* `/32`: 2^(32 - 32) = **1** ➝ Only one IP address

---

## 🚨 **5. VPC CIDR Block Constraints in AWS**

* **Minimum CIDR block for a VPC**: `/28` ➝ 16 IPs
* **Maximum CIDR block for a VPC**: `/16` ➝ 65,536 IPs

### 📝 Best Practice:

Use `/16` when creating a VPC to ensure flexibility and future scalability (subnets can be created later from this larger block).

---

## 🔁 **6. CIDR Block Usage Within VPC**

* CIDR blocks define **available private IPs** for resources like:

  * EC2 instances
  * Load Balancers (ELBs)
  * Auto Scaling Groups (ASGs)
* Subnets created within a VPC are further sub-divided CIDR blocks of the parent VPC.

### 🧪 Example:

If VPC = `10.0.0.0/16`, you can create subnets like:

* `10.0.0.0/24`
* `10.0.1.0/24`
* `10.0.2.0/28` … etc.

---

## 🧰 **7. Amazon's Reserved IPs in a Subnet**

From any subnet range, AWS reserves **5 IP addresses**:

* Network Address (e.g., `10.0.0.0`)
* VPC Router (e.g., `10.0.0.1`)
* DNS (e.g., `10.0.0.2`)
* Future use
* Broadcast address (e.g., `10.0.0.255`)

So **if you create a `/28` subnet (16 IPs)**, **only 11 are usable**.

---

## 📊 **8. Summary of Key CIDR Blocks**

| CIDR  | Usable IPs (approx.) | Common Usage            |
| ----- | -------------------- | ----------------------- |
| `/16` | 65,531               | VPC                     |
| `/24` | 251                  | Subnet (medium)         |
| `/28` | 11                   | Subnet (small)          |
| `/32` | 1                    | Single host route or IP |

---

## ❓ **FAQs Addressed in Session**

* **Q: What happens if I choose a small range like `/30`?**
  A: AWS doesn’t allow you to use CIDR blocks smaller than `/28` for a VPC.

* **Q: What does `/32` represent?**
  A: A single IP address, typically used in security rules or routing for a single host.

* **Q: Why do we start IP count from zero?**
  A: IP addressing begins at zero in binary counting. So `0` to `255` equals 256 total IPs.

---

## 📌 **Recommendation When Creating VPCs**

* Always use a **/16 block** for flexibility unless you have strict limitations.
* Plan subnetting ahead: define your **subnets**, **availability zones**, and **routing tables**.

---
