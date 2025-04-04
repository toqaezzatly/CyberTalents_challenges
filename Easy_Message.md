
# Easy Message - Cyber Talents 🛠️

oKay, this challenge is not about injection and there is nothing in the elements of the developer tools. In this challenge, we are **literally** finding our path, not by guessing (unfortunately), but using the `dirsreach` tool as follows:

```bash
dirsearch -u http://wcamxwl32pue3e6mxwl322yue3e6e435p0l7s835-web.cybertalentslabs.com/ -x 403,404
```

### Output 🌐
![image](https://github.com/user-attachments/assets/724d75b6-7542-4056-91a6-63e3f9f10439)

You can traverse other files, but you will eventually find out that `robots.txt` is the one of interest. 🕵️‍♀️

### Accessing robots.txt 🔍
```bash
curl http://wcamxwl32pue3e6mxwl322yue3e6e435p0l7s835-web.cybertalentslabs.com/robots.txt
```

![image](https://github.com/user-attachments/assets/7210e7e8-cb37-4bce-ac80-184fcfc5c442)

### Moving Forward 🚀
Open this link in the web browser:  
[http://wcamxwl32pue3e6mxwl322yue3e6e435p0l7s835-web.cybertalentslabs.com/?source](http://wcamxwl32pue3e6mxwl322yue3e6e435p0l7s835-web.cybertalentslabs.com/?source)

![image](https://github.com/user-attachments/assets/1ca5e869-312f-484b-8776-8d35547dc337)

This simple PHP code snippet indicates that this is a credential check decoded in **Base64**.

### Base64 Decoding 🔓
- **Original string**: `Q3liZXItVGFsZW50`  
- **Base64 Decoded string**: `Cyber-Talent`

### Credentials Entry 📝
After adding `Cyber-Talent` in the username and password fields, you will get this:

![image](https://github.com/user-attachments/assets/9fabdfa0-724d-49c1-939f-d3ce759a8cd7)

### Flag 🎉
After decoding -> **FLAG(IT-KN0W-Y0U-AR3-M0RS3)**  
