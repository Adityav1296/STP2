# Login Panel

## Descriptioin
login

https://tommytheduck.github.io/login

## Solve 
**Flag:** `v1t{p4ssw0rd}`

- For this challenge, I first checked the source code for the login page. There I found this info:

   info
- Here, two ciphers are given and a function is given in the source code that was converting the username and password to SHA256 encodings. In the image above, two encodings are given and the login page accepts only them as the correct username and password. So I decoded the two of them and found the flag and login credintials:

   flag
   flag

