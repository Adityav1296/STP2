# DOM XSS in jQuery anchor href attribute sink using location.search source

## Description
This lab contains a DOM-based cross-site scripting vulnerability in the submit feedback page. It uses the jQuery library's $ selector function to find an anchor element, and changes its href attribute using data from location.search.

To solve this lab, make the "back" link alert document.cookie. 

## Solve 

- After launching the lab, I had to look around for a while to find the correct back button which could execute the script. As per the description, it uses jQuery's selector function so I assumed that it might have a script written before or after it.
- The correct back button was finally found in the submit feedback link. On inspecting, it looked like this:

    <img width="963" height="218" alt="Screenshot 2026-01-03 170000" src="https://github.com/user-attachments/assets/e44767ac-bf01-47e6-97ce-af9544b223a9" />

- So from the script it is clear how the `href` attribute of the `anchor` tag is working. The script first looks for the id `backLink` and then it takes the value written in the `returnPath` parameter in the URL and puts that value in the `href` attribute.
- So, now I had to put a javascript in the `returnPath` parameter of the URL. Final payload: `javascript:alert(document.cookie)`

    <img width="1067" height="52" alt="Screenshot 2026-01-03 170036" src="https://github.com/user-attachments/assets/5d68fd87-9d9c-4baf-b8eb-022348b2d283" />

    <img width="962" height="177" alt="Screenshot 2026-01-03 170107" src="https://github.com/user-attachments/assets/ee0353a5-765e-40f7-a1d7-40e4340fce6d" />
