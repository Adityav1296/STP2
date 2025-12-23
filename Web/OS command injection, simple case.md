# OS command injection, simple case

## Description
 This lab contains an OS command injection vulnerability in the product stock checker.

The application executes a shell command containing user-supplied product and store IDs, and returns the raw output from the command in its response.

To solve the lab, execute the whoami command to determine the name of the current user. 

## Solve

- After launching the challenge lab, I first went to one of the product's pages. There, at the bottom, I found the stock checker option. I just clicked on the button and intercepted the request using burp suite.
- After looking at the GET request, at the bottom, the productId and the storeId were being sent to retrieve the data from the server. So, I added the `whoami` commands after the storeId and got the current user's name: 

    ss