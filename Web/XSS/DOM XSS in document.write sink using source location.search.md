# DOM XSS in document.write sink using source location.search

## Description
This lab contains a DOM-based cross-site scripting vulnerability in the search query tracking functionality. It uses the JavaScript document.write function, which writes data out to the page. The document.write function is called with data from location.search, which you can control using the website URL.

To solve this lab, perform a cross-site scripting attack that calls the alert function. 

## Solve

- After lauching the challenge, I first inspected the code of the page but didn't find anything. So, I just randomly typed a name in the search box shown on the page. After the search results were loaded, I again inspected the page's code and searched for the name specifically.
- This time it showed me the name appearing in two locations:

    pic

- In the second location, where the name appears in the `src` attribute of the `img` tag, a script is also present above it. That script takes the value written in the `search` parameter of the URL and then calls the `trackSearch(query)` function which uses document.write to write the search term in the `src` attribute.
- So, after finding where the solution might appear for the lab, I had to find a way to execute the `alert()` function. For that, I included double quotes in my search query to break out of the `src` attribute before adding the `onload` attribut to it. Final payload: `carlos"onload="alert(9)"`

    pic