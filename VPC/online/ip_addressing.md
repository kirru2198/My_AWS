<img width="343" alt="image" src="https://github.com/user-attachments/assets/98016c83-2d3e-4971-8076-991deeb9a5ed" />

---

## ✅ What is an IPv4 Address?

An **IPv4 address** is just a **unique number** assigned to every device connected to a network (like the internet or a home Wi-Fi). Think of it like the **home address** of your device — it tells other devices where to send data.

---

## 🧩 Structure: What Does It Look Like?

An IPv4 address looks like this:
**192.168.1.20**

It’s made up of **four numbers**, separated by **dots**.

Each of these four numbers:

* Is called an **octet**
* Can be **anything from 0 to 255**

So you might see IPs like:

* `10.0.0.1`
* `172.16.254.5`
* `192.168.1.20`

That’s all IPv4 — just combinations of four numbers from 0 to 255.

---

## 🧠 Why 0 to 255?

That’s where computers think differently from us.

Computers don't use regular numbers (like 1, 2, 3). They use **binary** — just 1s and 0s. Everything in a computer is stored using **bits** (short for binary digits).

### Example:

Each of the four numbers in an IPv4 address is stored using **8 bits**.

And with 8 bits, you can represent **256 different values** — from **0 to 255**.

Here's why:

* If you had 1 bit, you can make: 0, 1 → 2 values
* If you had 2 bits: 00, 01, 10, 11 → 4 values
* If you had 8 bits: you can make `2⁸ = 256` values

So, **each number** (like the 192 in `192.168.1.20`) is one byte or **8 bits** long.

---

## 🧮 What Does “32 Bits” Mean?

Since there are **4 numbers**, and each one uses **8 bits**, the total size of an IP address is:

```
4 numbers × 8 bits = 32 bits
```

That’s why IPv4 is called a **32-bit address system**.

---

## 🌎 How Many IPv4 Addresses Exist?

Since we have 32 bits, and each bit can be 0 or 1, the total number of unique combinations is:

```
2^32 = 4,294,967,296
```

So IPv4 can support **about 4.3 billion unique addresses**.

This sounds like a lot — but with billions of devices today (phones, laptops, routers, TVs, etc.), we're running out of them! That’s one reason IPv6 was created.

---

## 🔄 Let’s Translate One IP (Example)

Let's take **192.168.1.20**

Each number is converted to binary (1s and 0s):

| Decimal | Binary   |
| ------- | -------- |
| 192     | 11000000 |
| 168     | 10101000 |
| 1       | 00000001 |
| 20      | 00010100 |

So, behind the scenes, the computer stores:

```
11000000.10101000.00000001.00010100
```

But as humans, we find it easier to read:

```
192.168.1.20
```

---

## 🏠 Real-Life Analogy

Think of an IPv4 address like a **home mailing address**:

* Each part of the address (house number, street, city, ZIP code) helps the post office know **where to deliver** the letter.
* An IP address tells the internet where to deliver **data** — like websites, emails, or videos — to your device.

---
