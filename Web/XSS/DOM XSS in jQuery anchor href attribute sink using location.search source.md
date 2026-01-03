# DOM XSS in jQuery anchor href attribute sink using location.search source

## Description
This lab contains a DOM-based cross-site scripting vulnerability in the submit feedback page. It uses the jQuery library's $ selector function to find an anchor element, and changes its href attribute using data from location.search.

To solve this lab, make the "back" link alert document.cookie. 

## Solve 

- After launching the lab, I had to look around for a while to find the correct back button which could execute the script. As per the description, it uses jQuery's selector function so I assumed that it might have a script written before or after it.
- The correct back button was finally found in the submit feedback link. On inspecting, it looked like this:

    pic

- So from the script it is clear how the `href` attribute of the `anchor` tag is working. The script first looks for the id `backLink` and then it takes the value written in the `returnPath` parameter in the URL and puts that value in the `href` attribute.
- So, now I had to put a javascript in the `returnPath` parameter of the URL. Final payload: `javascript:alert(document.cookie)`

    pic
    
    pic