
# T-Jungle - Cyber Talent

Okay, here’s the matter: if you are familiar with PHP language, you will get to the point in no time. As you know, to compare between **two string values**, you have to use `===` instead of `==`. The former enforces a specific type, while the latter will convert the string to a number if possible. 

In this scenario, **it is possible**, because `0e...` is scientific notation meaning \(0 * 10^{...}\). And, luckily for you, the password after hashing is `0e...`.

So, if you can use a value that ends up hashed like this, you can bypass the challenge and get the flag. After searching, you find `QNKCDZO`. 

To solve it:
1. Edit the URL to:
   ```
   http://wcamxwl32pue3e6m4nzy586unzkme435p0l7s835-web.cybertalentslabs.com/?passwd=QNKCDZO
   ```

2. And voilà, you got your flag! 🎉

![Flag Image](https://github.com/user-attachments/assets/e1c21aa2-8f5f-4f12-9d55-37a7e0a4a3ff)
