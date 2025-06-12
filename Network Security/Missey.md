

# 🕵️  Missey

## 🧩 Challenge Overview

Our Network Monitoring System (NMS) flagged suspicious traffic coming from IP `192.168.1.7`. You’re tasked with analyzing the captured network traffic (`Missey.pcap`) and identifying any anomalies—possibly brute-force activity—and uncovering a hidden flag.

---



## 🔍 Step-by-Step Analysis

### 🔹 Step 1: Filter the Suspect Traffic

We start by isolating only the packets where the suspected attacker (`192.168.1.7`) is the source, and the destination is in the suspicious external IP range (`192.126.117.0/24`):

```bash
tshark -r Missey.pcap -Y "ip.src==192.168.1.7 && ip.dst >= 192.126.117.0 && ip.dst <= 192.126.117.255" -T fields -e tcp.payload > bruteforce.txt
```

**Breakdown:**

* `-r Missey.pcap`: Read the packet capture.
* `-Y`: Apply a **display filter** (as used in Wireshark GUI).
* `ip.src == ... && ip.dst == ...`: Focus only on brute-force attempts to external IPs.
* `-T fields -e tcp.payload`: Extract just the TCP payload (likely to include brute-force attempts).
* `> bruteforce.txt`: Save it for post-analysis.

---

### 🔹 Step 2: Decode the Extracted Payload (Double Decoding)

The extracted data in `bruteforce.txt` is **hex-encoded**. To convert it into readable ASCII, we used a **two-step decoding** using `xxd`:

```bash
cat bruteforce.txt | xxd -r -p | xxd -r -p
```

**Why decode twice?**

* The first `xxd -r -p` turns hex into **raw bytes**.
* But those raw bytes themselves are hex strings again (e.g., `"464c4147..."` → `"FLAG..."`).
* So we apply `xxd -r -p` **again** to finally decode the actual message.

---

### 🔹 Step 3: Finding the Flag

After decoding, we found a readable string:

```
FLAG{M15SED_INBY73$3}
```

But here’s the catch: the last `3` was **outside the flag format** and caused submission errors.

---

### 🔹 Step 4: Fix the Flag Format

To fix it:

* Manually remove the stray character (`3`), likely caused by trailing junk bytes or TCP fragmentation.
* This gives us the correct flag:

```
FLAG{M15SED_INBY73$}
```

---

## 🧠 Analysis – Why the “3” Was There?

* It’s **not part of the actual flag**.
* Likely an **extra byte** in the payload or leftover from TCP segmentation.
* Always visually verify the string and **clean up any artifacts** before submitting.

---

## ✅ Final Flag

```
FLAG{M15SED_INBY73$}
```

---

## 🗂 Summary Table

| Step | What We Did                        | Command                                                                  |
| ---- | ---------------------------------- | ------------------------------------------------------------------------ |
| 1️⃣  | Extract traffic from attacker      | `tshark -r Missey.pcap -Y "ip.src==..." -e tcp.payload > bruteforce.txt` |
| 2️⃣  | Decode hex payload (1st pass)      | `xxd -r -p`                                                              |
| 3️⃣  | Decode nested hex again (2nd pass) | `xxd -r -p` again                                                        |
| 4️⃣  | Extract readable flag              | `FLAG{M15SED_INBY73$3}` → remove extra `3`                               |

---

## 🧪 Bonus Tip

If you ever see this kind of **double-encoded hex**, try a nested decoding strategy. You can automate this by checking if the decoded string is still valid hex before re-decoding.
