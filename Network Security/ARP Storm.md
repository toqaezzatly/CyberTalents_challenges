
# 🧪 ARP Strom

---

## 🎯 **Goal of the Lab:**

Analyze a `.pcap` file that contains strange ARP traffic, figure out why the ARP packets have invalid opcodes, and extract a hidden flag by decoding the manipulated opcode values.

---

## 🧰 Tools Used:

* `Wireshark` – GUI network protocol analyzer
* `tshark` – CLI version of Wireshark
* `cut` – command to cut/select specific columns of text
* `xxd` – convert between hex and binary
* `base64` – encode/decode base64 text
* Optional: `cat`, `awk`, `grep` for file manipulation

---

## 🔍 Step 1: Open the `.pcap` file in Wireshark

```bash
wireshark ARP+Storm.pcap
```

### What to look for:

* All packets are **ARP**.
* But the **Opcode field** (which should normally be `0x0001` for request or `0x0002` for reply) contains **weird values**, such as:

  ```
  Unknown ARP opcode 0x006e
  ```

💡 That’s suspicious. It suggests the attacker is sending **malformed ARP packets** on purpose, possibly to hide data in them.

---

## 🧾 Step 2: Extract packet data using `tshark`

```bash
tshark -r ARP+Storm.pcap
```

* `-r` means "read from file"
* This outputs all packets in a text form to your terminal

💡 But this is too much. We only care about the **Opcode field** – the one that contains the strange hex values.

---

## ✂️ Step 3: Extract only the column containing the opcodes

```bash
tshark -r ARP+Storm.pcap | cut -d " " -f 21 > encoded_flag.txt
```

### Breakdown:

* `cut -d " " -f 21`: This cuts column number 21, using space `" "` as the delimiter.
* That column (in this specific file) contains something like:

  ```
  Opcode 0x006e
  Opcode 0x0061
  ...
  ```

💡 Now we have a list of "Opcode 0x\*\*\*\*" values saved into a file.

---

## 🧹 Step 4: Clean the data to get **only the hex values**

```bash
cat encoded_flag.txt | cut -d "x" -f 2 > encoded_opcode.txt
```

### What this does:

* Splits each line at the letter `"x"` (in `0x006e`)
* Takes the second part, which is the hex value (e.g., `006e`)
* Resulting file (`encoded_opcode.txt`) contains clean hex values, one per line:

  ```
  006e
  0061
  0067
  ...
  ```

---

## 🔁 Step 5: Turn the hex values into raw binary text

```bash
cat encoded_opcode.txt | xxd -r -p
```

### Breakdown:

* `xxd`: A tool that normally converts files to hex.
* `-r`: Reverse mode (hex → binary)
* `-p`: Plain hex format

💡 Now you get an output like:

```
ZmxhZ3tnckB0dWl0MHVzXz...
```

This is a **Base64 encoded string**.

---

## 🔓 Step 6: Decode the base64 string

```bash
echo -n "ZmxhZ3tnckB0dWl0MHVzXzBwY09kZV8xc19BbHdAeXNfQTZ1U2VkX3QwX3AwMXMwbn0=" | base64 -d
```

### Output:

```
flag{gr@tuit0us_0pcOde_1s_Alw@ys_A6uSed_t0_p01s0n}
```

🎉 You’ve got the flag!

---

## 🧠 What did you really learn?

### Networking concepts:

* **ARP Protocol**: Usually helps devices map IP addresses to MAC addresses.
* **Gratuitous ARP**: ARP packets sent without being requested — often used in attacks.
* **ARP Poisoning**: Sending fake ARP packets to poison the target’s ARP cache.

### Tools & Skills:

| Tool      | What you used it for                          |
| --------- | --------------------------------------------- |
| Wireshark | Inspect ARP packets                           |
| tshark    | Extract data from `.pcap` on the command line |
| cut       | Slice specific columns of text                |
| xxd       | Convert hex to binary text                    |
| base64    | Decode base64-encoded string                  |

---

## 🚀 Summary

This lab teaches you:

* How attackers may **hide payloads** in network protocol fields.
* How to **analyze traffic manually** using `tshark` instead of GUI.
* How to use **basic Linux CLI tools** to clean, transform, and decode data.
