
# Cracking "This is Sparta" on CyberTalents

## A Quick Web CTF Guide Using Dev Tools

The **"This is Sparta"** challenge on [CyberTalents](https://cybertalents.com/) is a beginner web CTF that highlights the power of dev tools. I’ll show you how I solved it by focusing on the source code and the `check()` function—ignoring the useless hint and SQL injection.

---

## The Challenge

You get a login page asking for a username and password, with a hint: "Easier than Ableton." Spoiler: **the hint is useless**, and SQL injection won’t work. The key is in the source code, and dev tools (F12) are your best friend.

---

## Step 1: Find the `check()` Function

I opened dev tools (F12) and searched the source code for keywords like `user`, `password`, or `check`. I found this JavaScript:

```javascript
var _0xae5b=["\x76\x61\x6C\x75\x65","\x75\x73\x65\x72","\x67\x65\x74\x45\x6C\x65\x6D\x65\x6E\x74\x42\x79\x49\x64","\x70\x61\x73\x73","\x43\x79\x62\x65\x72\x2d\x54\x61\x6C\x65\x6E\x74","\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x43\x6F\x6E\x67\x72\x61\x74\x7a\x20\x0A\x0A","\x77\x72\x6F\x6E\x67\x20\x50\x61\x73\x73\x77\x6F\x72\x64"];

function check() {
    var _0xeb80x2 = document[_0xae5b[2]](_0xae5b[1])[_0xae5b[0]];
    var _0xeb80x3 = document[_0xae5b[2]](_0xae5b[3])[_0xae5b[0]];
    if (_0xeb80x2 == _0xae5b[4] && _0xeb80x3 == _0xae5b[4]) {
        alert(_0xae5b[5]);
    } else {
        alert(_0xae5b[6]);
    }
}
```

The `check()` function is our heroine, but the array `_0xae5b` is hex-encoded.

---

## Step 2: Decode the Hex

I decoded the `_0xae5b` array:

- `_0xae5b[0]` → `"value"`
- `_0xae5b[1]` → `"user"`
- `_0xae5b[2]` → `"getElementById"`
- `_0xae5b[3]` → `"pass"`
- `_0xae5b[4]` → `"Cyber-Talent"`
- `_0xae5b[5]` → `"                    Congratz \n\n"`
- `_0xae5b[6]` → `"wrong Password"`

---

## Step 3: Solve with `check()`

The decoded `check()` function shows:

```javascript
function check() {
    var _0xeb80x2 = document["getElementById"]("user")["value"];
    var _0xeb80x3 = document["getElementById"]("pass")["value"];
    if (_0xeb80x2 == "Cyber-Talent" && _0xeb80x3 == "Cyber-Talent") {
        alert("                    Congratz \n\n");
    } else {
        alert("wrong Password");
    }
}
```

It checks if both username and password are `"Cyber-Talent"`.

---

## Step 4: Get the Flag

I entered:

- **Username**: `Cyber-Talent`
- **Password**: `Cyber-Talent`

An alert revealed the flag:

```plaintext
FLAG: {J4V4_Scr1Pt_1S_Aw3s0me}
```

Submitted and done!

---

## Takeaways

- **Dev Tools Rule**: Use F12 to find `check()` in the source.
- **Skip Distractions**: The hint and SQL injection are useless.
- **`check()` is Key**: It holds the solution in client-side challenges.
- **Decode Hex**: Unobfuscate strings to reveal the logic.

---

## Tips

- Use dev tools (F12) to inspect source code.
- Search for `user`, `password`, or `check`.
- Decode hex with online tools.


---

## Final Thoughts

"This is Sparta" proves dev tools and the `check()` function can crack web CTFs. Ignore distractions and dig into the code. What’s your favorite CTF? Comment below!

Happy hacking! 🚀

