
# 🛡️ Encrypted Database Lab Walkthrough  - Cyber Talents

## 🔍 Step 1: Identifying the Issue  
The lab hints suggested the error might be in the database, which led me to attempt SQL injection. However, no injection points were found.  

## 📝 Step 2: Checking the dev tool tags


![image](https://github.com/user-attachments/assets/1cd38bfd-0097-4768-be6a-7b246b6ab3af)

I traversed the following path to access the source code:
```
http://wcamxwl32pue3e6mxmdvww5c1v58e435p0l7s835-web.cybertalentslabs.com/admin/assets/app.js
```  
While I could display the source code, I didn’t find anything useful.  

## 📂 Step 3: Directory Traversal Attempt  
I took two steps back in the URL structure to explore:  
```
http://wcamxwl32pue3e6mxmdvww5c1v58e435p0l7s835-web.cybertalentslabs.com/admin/
```  
This revealed the following page:  

![Admin Page](https://github.com/user-attachments/assets/70f6cded-54cd-4a83-9c48-a2e020f89a10)  

## 🛠️ Step 4: Inspecting the Page Source  
Thinking this might be the place for an injection attempt, I checked the developer tools for keywords like `username`, `password`, `input`, and `hidden`. However, nothing significant was found.  

![image](https://github.com/user-attachments/assets/c8eebba0-7f02-4150-b991-5de70b962b4f)

## 🔑 Step 5: Discovering the Database File  
I attempted accessing:  
```
http://wcamxwl32pue3e6mxmdvww5c1v58e435p0l7s835-web.cybertalentslabs.com/admin/secret-database/db.json
```  
This led to an exposed JSON file containing credentials:  

![Database JSON](https://github.com/user-attachments/assets/65a81118-40d7-4db9-bfc2-62448d6375b4)  

## 🔓 Step 6: Cracking the MD5 Hash  
The JSON file contained an MD5-hashed password:  
```
ab003765f3424bf8e2c8d1d69762d72c
```  
Using an online hash cracker, I found the plaintext password:  
```
badboy
```  

## ⚠️ Conclusion  
- The database file was publicly accessible, revealing sensitive credentials.  
- Always ensure proper access control and avoid exposing critical files in public directories.  
- Hashing alone is not enough—use stronger hashing algorithms (e.g., bcrypt, Argon2) with proper salting.  

**Lesson Learned:** Proper security practices can prevent such data leaks and unauthorized access. 🔐   

