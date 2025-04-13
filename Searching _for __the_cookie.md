

## Searching for the Cookie - CyberTalents

For this challenge, as the description indicates, we should look for a cookie. You might think it's an XSS vulnerability—and you're absolutely right! Here's how I tackled it:

### Steps Taken:

1. After checking the **Elements** tab in the developer tools, I decided to mess with the search function.
   - Tried closing the single quote with something like: `'}; alert(1);`
   - Unfortunately, it failed, and I couldn't bypass it.

2. So, I attempted to close the `<script>` element and write my own script element below it:
   ```html
   </script><script>alert(document.cookie)</script>
   ```

    And guess what? It worked!

3. **Unexpected Result**:
   - However, the alert appeared with the word `undefined`. Where is the cookie?! 

### Finding the Cookie:
If you search for cookies in the **Memory** element, you'll find them easily. Here's a screenshot for reference:

![Memory screenshot](https://github.com/user-attachments/assets/0a05478e-0fe2-47c6-975e-79e3ad3de51c)

### Why Didn't the Cookie Appear in `document.cookie`?
This is because of the `httpOnly` property. When creating or reading cookies with `httpOnly` enabled:
   - It isolates the cookie from the `document.cookie` object in JavaScript.
   - Such cookies **cannot appear** on the web page but can still be found in **Application > Cookies**.

### Alternative Approach:
You can also replace the `alert` function with a `console.log` function to log cookies to the browser console:
```html
</script><script>console.log(document.cookie)</script>
```
This will display the cookies in the console.

