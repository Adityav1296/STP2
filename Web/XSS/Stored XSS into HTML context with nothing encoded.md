# Stored XSS into HTML context with nothing encoded

## Description
This lab contains a stored cross-site scripting vulnerability in the comment functionality.

To solve this lab, submit a comment that calls the alert function when the blog post is viewed. 

## Solve
- After launching the lab, I opened one of the posts shown on the web page. In there, I quickly filled in a comment and submitted it to intercept the outgoing request using burp.
- In the description, it is mentioned that the vulnerability is in the comment functionality. So, I edited the original comment to my payload: `<script>alert(9)</script>` and solved the lab.

    <img width="878" height="611" alt="Screenshot 2026-01-01 185032" src="https://github.com/user-attachments/assets/68cc6598-f3a4-48e9-967e-9ce1b337d1ee" />
