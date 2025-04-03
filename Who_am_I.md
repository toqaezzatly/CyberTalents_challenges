

## Cybertalents: "Who am I" Challenge Walkthrough

This walkthrough details the steps taken to solve the "Who am I" challenge on Cybertalents, focusing on reconnaissance and cookie manipulation.

### Initial Analysis & Reconnaissance

1.  **Initial Thoughts & Attempts:** The first instinct was to test for common web vulnerabilities like SQL injection or command injection by manipulating input fields. However, these attempts yielded no useful results or error messages, suggesting the vulnerability might lie elsewhere.

2.  **Developer Tools Investigation:** The next step involved inspecting the web page's source code using the browser's Developer Tools. The focus was on searching for potentially revealing keywords such as `username`, `user`, `password`, `login`, `admin`, `check()`, etc.

3.  **Key Finding: Credentials in Source Code:** This approach proved successful. **Crucially, username (`root`) and password (`root`) credentials were found commented out directly within the HTML source code.** This is a significant security misconfiguration.

    ![Credentials found in HTML source comments](https://github.com/user-attachments/assets/06669056-2b1a-40d7-8f59-095c41fad679)

4.  **Login Verification:** Using the discovered credentials (`root` / `root`), a successful login was achieved. However, this granted access as the user `root`, not the target user `admin`.

    ![Successful login as 'root' user](https://github.com/user-attachments/assets/a4996fd8-b874-4e8c-9267-5ec45e4bc8aa)

### Exploitation via Cookie Manipulation

5.  **Objective & Failed Attempt:** The goal was to gain access as the `admin` user. A simple attempt to log in with `admin`/`admin` or similar default credentials failed. This indicated that direct login as admin wasn't straightforward or perhaps disabled.

6.  **Cookie Inspection:** Attention then shifted to how the application managed sessions, specifically the cookies. Using the Developer Tools (Application -> Cookies tab), a session cookie was identified.

    ![Inspecting the session cookie in Developer Tools](https://github.com/user-attachments/assets/4a1de0a5-f601-4c32-b25c-3a7eaf2ba973)

7.  **Cookie Decoding:** The value of the identified cookie (`bG9naW49R3Vlc3Q=`) appeared to be Base64 encoded. Decoding this value revealed its plain text content:
    ```
    login=Guest
    ```
    *(Self-correction: The screenshot shows logging in as `root` but the cookie shows `Guest`. This might imply the cookie wasn't updated post-login or the screenshot of the cookie was taken before the `root` login. However, the process remains the same: identify the cookie format).*
    **This revealed that the application uses a simple, unencrypted, and easily predictable cookie structure to identify the logged-in user.**

8.  **Crafting the Target Cookie:** The hypothesis was that by modifying this cookie value to represent the `admin` user, it might be possible to impersonate them. The target plain text value was formulated:
    ```
    login=admin
    ```

9.  **Encoding the Malicious Cookie:** This target string was then Base64 encoded:
    ```
    login=admin  -->  bG9naW49YWRtaW4=
    ```

10. **Cookie Replacement:** The original cookie value (`bG9naW49R3Vlc3Q=`) was replaced with the newly crafted Base64 string (`bG9naW49YWRtaW4=`) directly in the browser's Developer Tools.

11. **Accessing Admin Privileges:** After saving the modified cookie, the page was refreshed.

### Result: Flag Obtained

Upon refreshing the page with the modified cookie, the application accepted the forged identity. **The server trusted the client-provided cookie value without sufficient validation**, granting access as the `admin` user and revealing the flag.

![Flag revealed after cookie manipulation](https://github.com/user-attachments/assets/e6b559b2-c482-4ac7-9e1f-5151e32c53d1)

### Key Takeaways & Vulnerabilities

*   **Credentials in Source Code:** Hardcoding or commenting out sensitive information like passwords in client-side code is a critical vulnerability.
*   **Insecure Session Management:** Relying on easily manipulable, unencrypted, and unvalidated cookie values (like Base64 encoded usernames) for authorization is insecure. The application improperly trusted client-side data.
*   **Importance of Developer Tools:** Browser developer tools are indispensable for web security testing, allowing inspection of source code, network requests, and client-side storage like cookies.

---
