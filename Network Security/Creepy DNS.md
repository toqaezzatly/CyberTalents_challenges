
# 🕵️ Creepy DNS 

## 🧩 Scenario

Your **Network Monitoring System (NMS)** detected suspicious DNS traffic. You were handed a capture file `dns.pcapng` and tasked with investigating it to uncover any anomalies or hidden data.

---

## 🧰 Tools Used (and why)

### 1. **Tshark**

> A terminal-based version of **Wireshark**, used for packet analysis from the command line.

* `-r dns.pcapng`
  → Tells `tshark` to **read from a capture file** named `dns.pcapng`.

* `-Y "dns.qry.name contains \"cybertalents.com\""`
  → This is a **display filter** (like in Wireshark). It tells tshark to show only packets where the **DNS query name** (i.e., the domain being requested) **includes** the string `cybertalents.com`.

* `-T fields`
  → This tells tshark to **output in field format** (i.e., don't show the full packet, just specific fields).

* `-e dns.qry.name`
  → This tells tshark to **extract the field** called `dns.qry.name`, which is the actual domain name requested in each DNS query.

### 🧪 Example:

```bash
tshark -r dns.pcapng -Y "dns.qry.name contains \"cybertalents.com\"" -T fields -e dns.qry.name
```

📤 Output:

```
ZZZZ.cybertalents.com
mmmm.cybertalents.com
xxxx.cybertalents.com
...
```

---

### 2. **awk**

> A powerful command-line tool for **text manipulation**, especially with columns and delimiters.

* `-F'.'`
  → Tells `awk` to use the **dot `.`** as a **field separator** (DNS names are dot-separated).

* `'$2=="cybertalents"{print $1}'`
  → This logic says:

  * If the **second field is `cybertalents`**, then **print the first field**, which is the **subdomain** (e.g., `ZZZZ`, `mmmm`, etc.)

### 🧪 Example:

```bash
... | awk -F'.' '$2=="cybertalents"{print $1}'
```

📤 Output:

```
ZZZZ
mmmm
xxxx
...
```

---

### 3. **tr**

> Used for **translating or deleting characters**.

* `tr -d '\n'`
  → Removes **newline characters**, effectively putting all the words on **one line** to form a long, continuous string.

### 🧪 Example:

```bash
... | tr -d '\n'
```

📤 Output:

```
ZZZZmmmmxxxxhhhhZZZZ...
```

---

### 4. **sed**

> A **stream editor** used to **find and replace text** using patterns.

* `-E`
  → Enables **extended regex** syntax (makes the pattern syntax easier to read).

* `'s/(.)\1+/\1/g'`
  → Regex explanation:

  * `(.)` captures a single character.
  * `\1+` means “one or more repetitions of that same character”.
  * So this replaces **repeated letters** with just **one instance** of the letter.

🧠 Purpose: Clean up the string to remove extra obfuscation.

### 🧪 Example:

```bash
... | sed -E 's/(.)\1+/\1/g'
```

📤 Input:

```
ZZZZmmmmttttcccc
```

📤 Output:

```
Zmtc
```

---

### 5. **base64**

> A tool for **encoding or decoding Base64** strings (a common method for hiding data in DNS, HTTP, etc.).

* `base64 -d`
  → Decodes a Base64 string back to its **original text**.

### 🧪 Example:

```bash
... | base64 -d
```

📤 Input:

```
ZmxhZ3t0c2hBcmtfSXNfQXdlczBtZV9OZXR3MHJraW5nX3RvMGx9
```

📤 Output:

```
flag{tshArk_Is_Awes0me_Netw0rking_to0l}
```

---

## 🏁 Final Command (All-in-One):

```bash
tshark -r dns.pcapng -Y "dns.qry.name contains \"cybertalents.com\"" -T fields -e dns.qry.name \
| awk -F'.' '$2=="cybertalents"{print $1}' \
| tr -d '\n' \
| sed -E 's/(.)\1+/\1/g' \
| base64 -d
```

📥 **Final Output**:

```
flag{tshArk_Is_Awes0me_Netw0rking_to0l}
```

---

## 🧠 What You Learned

* DNS queries can be used to **exfiltrate or hide data** by splitting a message into the subdomains.
* You learned to **reconstruct a hidden message** from PCAP data using command-line tools.
* This is a typical **CTF-style DNS covert channel** lab.
