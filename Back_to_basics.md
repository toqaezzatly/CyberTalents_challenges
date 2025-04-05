# Back to Basics - Cyber Talents Challenge

This challenge lives up to its name, offering a fundamental lesson in investigating source code and elements using the developer tools.

## Steps to Solve:
1. **Access Developer Tools**:
    - Right-clicking doesn't work here, so use the keyboard shortcut:
      - Press **Fn + F12** to open Developer Tools.

2. **Inspect the `script` Element**:
    - Upon viewing the `script` element, you will find JavaScript code that blocks specific key combinations:
        - **Ctrl+C (67)**: Copy action.
        - **Ctrl+V (86)**: Paste action.
        - **Ctrl+U (85)**: View page source.
        - **Ctrl+F6 (117)**: Developer tools or focus change.

3. **Dig Deeper**:
    - Continue surfing through the developer tools until you reach the following highlighted section:
    
    ![Image of deeper inspection](https://github.com/user-attachments/assets/b207852e-5dee-415a-9b9e-a1c2ffa1e067)

4. **Find the Flag**:
    - In the inspected code, the flag is commented as:
    ```
// FLAG{OkaY_I_FailEd_tO_kEEp_yOu_awAy_hEre_iS_yOur_fLAg}
    ```

---

## What You Can Learn:
- **Using Developer Tools**:
  - Gain proficiency with browser DevTools, particularly for inspecting elements and debugging.
  
- **JavaScript Basics**:
  - Understand how JavaScript can be used to control user actions and interact with the page.

- **Bypassing Content Restrictions**:
  - Recognize common tricks to prevent interactions (e.g., blocking right-click, `Ctrl+U`).
  - Learn how to bypass these restrictions to access the underlying code.

- **Problem-Solving Skills**:
  - Approach challenges methodically and explore all available tools and shortcuts.

**Conclusion**: This challenge reinforces the importance of persistence and thorough inspection when working with web applications. Keep exploring, and happy flag hunting!
