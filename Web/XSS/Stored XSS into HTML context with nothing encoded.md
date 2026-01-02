# Stored XSS into HTML context with nothing encoded

## Description
This lab contains a stored cross-site scripting vulnerability in the comment functionality.

To solve this lab, submit a comment that calls the alert function when the blog post is viewed. 

## Solve
- After launching the lab, I opened one of the posts shown on the web page. In there, I quickly filled in a comment and submitted it to intercept the outgoing request using burp.
- In the description, it is mentioned that the vulnerability is in the comment functionality. So, I edited the original comment to my payload: `<script>alert(9)</script>` and solved the lab.

    pic