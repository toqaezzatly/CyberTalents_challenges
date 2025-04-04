# Newsletter - Cybertalents

In this challenge, there is nothing related to UI, and it is very strict about the input.

![Image](https://github.com/user-attachments/assets/d043fc8e-066d-439b-83ca-faeb1c72aa4e)

After checking the dev tools, nothing seems interesting to test. 
So, injecting through the UI is not valid, and the dev tools are useless. 

Thus, my next step is to use Burp Suite to intercept the traffic and perform the injection in transit.

![Image](https://github.com/user-attachments/assets/c7fea713-bbf0-4d2a-910c-78d3c8a6f451)

And it worked! This indicates that there is only client-side validation here, so we could make OS injection using `; ls -a ||` aftet adding the email.
