
# Bean - Cyber Talents

As you can see, there is nothing to do with the HTML page, and the developer tools have nothing interesting.  
So in situations like this, I will check if there is any hint for subdirectories, as the description suggests:

![image](https://github.com/user-attachments/assets/905f6283-2f79-4363-845a-63ad1d4b4f01)

So this lab is about **path traversal**, and to find the subdirectories, I used **dirsearch** after the trials failed.

- `http://wcamxwl32pue3e6mr4gvmkxal3dme435p0l7s835-web.cybertalentslabs.com/../home`
- `http://wcamxwl32pue3e6mr4gvmkxal3dme435p0l7s835-web.cybertalentslabs.com/home`

At this point, we need to find another starting point.

![image](https://github.com/user-attachments/assets/1766e4a9-56cd-481a-b6f5-9c7ce82dee18)

So, our new starting point is:  
`http://wcamxwl32pue3e6mr4gvmkxal3dme435p0l7s835-web.cybertalentslabs.com/files/`

From the hint or the description (whichever you want to call it), we should go back with:  
`http://wcamxwl32pue3e6mr4gvmkxal3dme435p0l7s835-web.cybertalentslabs.com/files/../home`

But guess what? It will fail as well.

But as a workaround, try:  
`http://wcamxwl32pue3e6mr4gvmkxal3dme435p0l7s835-web.cybertalentslabs.com/files../home`

And guess again? It worked! 🎉

![image](https://github.com/user-attachments/assets/4834c35a-57c5-42db-8eae-9dc78690f7c8)

Here is your flag:  
**`FLAG{Nginx_nOt_aLWays_sEcUre_bY_The_waY}`**

### Explanation of the Vulnerability

The vulnerability was caused by **bad sanitization**. The server only relies on splitting the path according to `/` and blocking `..` if it comes after removing the slashes, not forwarding them to the OS. However, `files..` will not be filtered because it is considered a valid file name (`files..`). But the OS resolves it as:

- Go to `files/`
- Then move up (`..`) to the parent directory
- Finally, access `/home`

This results in a successful path traversal.

