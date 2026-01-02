# Reflected XSS into HTML context with nothing encoded

## Description
This lab contains a simple reflected cross-site scripting vulnerability in the search functionality.

To solve the lab, perform a cross-site scripting attack that calls the alert function. 

## Solve 

- After launching the challenge lab, I straight away put my javascript payload in the search fucntionality. Payload: `<script>alert(9)</script>`
- Running this payload gave the following output which indicated that I solved the lab:

    <img width="878" height="611" alt="Screenshot 2026-01-01 185032" src="https://github.com/user-attachments/assets/3b4f28ea-e38a-4436-9f85-5f2bf90d760e" />

    
