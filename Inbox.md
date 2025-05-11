# 📬 Inbox

This challenge is pretty easy, so let's get straight to the point.

---

## 🔍 First Look

This is what you see when you click on the challenge link:

![interface](https://github.com/user-attachments/assets/479bad9b-9350-4879-aa4c-4e57a089f4d6)

You try crawling the website by clicking on everything in front of you, but nothing is clickable — **except for the messages**.

---

## 🕵️ Monitoring with Burp

Clicking on any message and monitoring the requests in **Burp Suite**, you’ll see a file called `show.php`:

![burp-showphp](https://github.com/user-attachments/assets/939310d0-b304-49a8-99f3-f6ed19859153)

Notice the `id` field — let's try injecting:

```
id='
```

And here’s what we get:

![sqlite-error](https://github.com/user-attachments/assets/3cf8c5a3-1494-4bf7-9547-2db82a4580ff)

Good news! 🎉 The `HY000 1` error means this is **SQLite**.

---

## 🧪 Testing SQLite Injection

Let’s try to fetch all tables using:

```
id=(select * from sqlite_master)
```

![image](https://github.com/user-attachments/assets/63706fd9-abc9-43ed-98f3-6123d284ae9e)

But as you can see, **only a single result is allowed**.

---

## 🧠 Refining the Injection

So we try to get a single result using a table filter. Example:

```
id=(select id from sqlite_master where Title="Hola")
```

![hola-table](https://github.com/user-attachments/assets/f65b36f5-a12a-4c33-bd7b-74f55e206ba6)

And guess what? **It works!**

![success](https://github.com/user-attachments/assets/9ab1ddbc-f913-4403-8120-e5516b181227)

Now, let’s try a different value:

```
id=(select id from sqlite_master where Title="flag")
```

And here it is:

![flag](https://github.com/user-attachments/assets/311ce4a9-d07e-4954-9d63-ae57384714a2)

```json
{"title":"flag","msg":"KHDFI3#fi4@Flag"}
```

Boom 💥! There’s the flag.

---

## ✅ Final Flag

```
KHDFI3#fi4@Flag
```

---

## 🔎 Vulnerability Assessment

* **Vulnerability Type:** SQL Injection (SQLite)
* **Vector:** Unsanitized `id` parameter in `show.php`
* **Database:** SQLite, confirmed via error message
* **Impact:** Full access to internal schema; ability to extract sensitive data
* **Limitation:** Only one row is returned — multi-row results are blocked (probably due to how the backend handles subqueries)

---

## 🧠 Lessons Learned

* Always monitor traffic via **Burp Suite** or similar tools — they reveal hidden API endpoints like `show.php`.
* **Error messages** can leak database technology (e.g., `HY000 1` reveals SQLite).
* In SQLite, `sqlite_master` is your best friend when injecting — it holds all schema data.
* Even limited-response systems can be abused using **targeted subqueries**.
* **Never trust user input** — this entire issue stems from poor input validation and lack of query parameterization.
