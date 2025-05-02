### 🧠 Example: NAT Gateway – “Outbound Only”

Imagine you have a computer in a **private room** with **no door for visitors** — no one from outside can come in.
But inside, there's a **window** you can use to **look out and send messages** (like browsing the web).

That’s what a **NAT Gateway** does:

* Your **private EC2 instances** (in private subnets) can **go out to the internet** to:

  * Download software updates
  * Call external APIs
  * Access S3 or other services

✅ But… people **on the internet can’t reach them directly** — your resources stay **safe and hidden**.

---

