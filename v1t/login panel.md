# Login Panel

## Descriptioin
login

https://tommytheduck.github.io/login

## Solve 
**Flag:** `v1t{p4ssw0rd}`

- For this challenge, I first checked the source code for the login page. There I found this info:

   <img width="919" height="389" alt="Screenshot 2025-11-07 172818" src="https://github.com/user-attachments/assets/82183f5c-740e-4b13-a908-f52b2420a25a" />

- Here, two ciphers are given and a function is given in the source code that was converting the username and password to SHA256 encodings. In the image above, two encodings are given and the login page accepts only them as the correct username and password. So I decoded the two of them and found the flag and login credintials:

   <img width="949" height="327" alt="Screenshot 2025-11-07 162255" src="https://github.com/user-attachments/assets/b2ab9fba-3083-44ee-b550-cf38d1148303" />

    <img width="932" height="340" alt="Screenshot 2025-11-07 162349" src="https://github.com/user-attachments/assets/98b42ecc-8d2e-43ad-b20f-9d8bfbac7d0f" />
