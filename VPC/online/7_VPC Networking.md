# 📘 AWS VPC Networking Notes – Detailed Breakdown

---

## 🏗 VPC Topology and Routing: Basic Concepts

### Example Network Setup:

* VPC CIDR: `10.0.0.0/16`
* Subnet A: `10.0.0.0/24` (256 IPs)
* Subnet B: `10.0.1.0/24` (256 IPs)
* EC2 Instances:

  * EC2-A in Subnet A: IP `10.0.0.125`
  * EC2-B in Subnet B: IP `10.0.1.150`

### 🔄 Inter-Subnet Communication:

* All EC2s in same VPC **can communicate** with each other.
* This is enabled by a **default route** in the **main route table**:

  ```
  Destination      Target
  10.0.0.0/16      local
  ```
* Traffic between EC2-A and EC2-B uses the **router** inside the VPC.

---

## 🛣 Route Tables in AWS VPC

* A **route table** defines how traffic is routed within the VPC.
* **Each subnet must be associated with exactly one route table.**
* By default, a **main route table** is created and associated with all subnets.

### 🧠 Important Points:

* Route table entries:

  * `Destination`: IP range (CIDR)
  * `Target`: Where to send traffic (e.g., `local`, `internet gateway`)

#### Example Route Table:

| Destination | Target           |
| ----------- | ---------------- |
| 10.0.0.0/16 | local            |
| 0.0.0.0/0   | internet gateway |

---

## 🌍 Accessing the Internet

### Scenario: Installing Software via `yum install httpd`

* EC2 instance needs to reach the **public internet**.
* Destination IP (e.g., `151.101.0.81`) **does not start with 10.x.x.x**, `172.16.x.x`, or `192.168.x.x`.
* Router checks the **route table**:

  * If there's **no matching rule for external IP**, request is **denied**.
  * To enable internet access, you need to add:

    ```
    Destination: 0.0.0.0/0
    Target: Internet Gateway
    ```

### 🧱 Default Behavior

* Without `0.0.0.0/0` pointing to an Internet Gateway (IGW), subnets are **private**.
* Cannot reach or be reached from the internet.

---

## 🔐 Public vs Private Subnets

### ✅ Public Subnet:

* Has a route table entry:

  ```
  0.0.0.0/0 → Internet Gateway
  ```
* Instances **can access and be accessed** from the internet (assuming public IP and security groups allow it).

### ❌ Private Subnet:

* No route to Internet Gateway.
* Instances **cannot access** internet directly.
* Still can communicate **internally** within the VPC.

---

## 🧭 Making Subnets Public or Private

### By Default:

* All subnets share the **main route table**.
* Modifying the main route table affects **all subnets**.

### To Separate:

1. **Create a new route table** with only the `local` route:

   ```
   10.0.0.0/16 → local
   ```
2. **Associate** this new route table to the subnet you want as **private**.
3. Keep or modify the main route table to add the internet gateway for **public** subnets.

### Important Rule:

* A **subnet can be associated with only one route table**.
* Associating a new table automatically **disassociates** the previous one.

---

## 🔄 Internal vs External Communication

| Source Subnet | Destination IP     | Routed? | Conditions                 |
| ------------- | ------------------ | ------- | -------------------------- |
| Subnet A      | 10.0.1.150 (B)     | ✅ Yes   | Matches `10.0.0.0/16`      |
| Subnet B      | 8.8.8.8 (Internet) | ❌ No    | Unless IGW route exists    |
| Internet      | EC2 in Subnet A    | ❌ No    | Needs IGW + security rules |

---

## 🗂 Summary: Key Takeaways

* AWS **reserves 5 IPs** per subnet (first 4 + last 1).
* Route tables control **internal and external routing**.
* All subnets are **private by default**.
* Use **Internet Gateway** + **route entry** (`0.0.0.0/0`) to enable internet access.
* To isolate access:

  * Use **separate route tables** for private/public subnets.
* Internal VPC communication is always allowed with the `local` route.

---

## 🔧 Coming Up (Lab Preview)

In the next hands-on lab:

* Inspect the default VPC and its configuration.
* Create a **custom VPC**:

  * Define CIDR range
  * Create public & private subnets
  * Set up route tables
  * Add Internet Gateway
* Launch EC2s and test connectivity:

  * Internal communication
  * Internet access

---
