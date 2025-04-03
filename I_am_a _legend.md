
# **I Am a Legend Challenge - CyberTalents**  

At first glance, SQL injection seems like a valid approach. However, in this case, it's unnecessary and won’t yield any results.  

### **Step 1: Checking the Source Code**  
Since I had no credentials and there was nothing to exploit via the UI, I examined the **source code**. From a previous challenge with a similar UI, I suspected the vulnerability lay in **client-side credential validation**, specifically in the `check()` function.  

After searching for `check()`, I found this:  
![image](https://github.com/user-attachments/assets/b2f32837-a669-48fd-b5a9-904c5805696a)  

### **Step 2: Identifying JSFuck Obfuscation**  
Upon inspecting the `<script>` tag, I noticed that the JavaScript was heavily **obfuscated** using **JSFuck**, an extreme obfuscation technique that only uses six characters:  

```
[] + ! ( ) 
```
JSFuck works by exploiting JavaScript’s type coercion and implicit conversions, making the code unreadable.  

Here’s an example of JSFuck obfuscation:  
![image](https://github.com/user-attachments/assets/0db378e4-02c5-4a47-a964-17a7699d80e8)  

### **Step 3: Extracting the Flag**  
By decoding the JSFuck obfuscation, I was able to retrieve the flag:  

> **Flag{J4V4_Scr1Pt_1S_S0_D4MN_FUN}**  

---

