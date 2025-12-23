# OS command injection, simple case

## Description
 This lab contains an OS command injection vulnerability in the product stock checker.

The application executes a shell command containing user-supplied product and store IDs, and returns the raw output from the command in its response.

To solve the lab, execute the whoami command to determine the name of the current user. 

## Solve

- After launching the challenge lab, I first went to one of the product's pages. There, at the bottom, I found the stock checker option. I just clicked on the button and intercepted the request using burp suite.

  <img width="706" height="94" alt="Screenshot 2025-12-23 092155" src="https://github.com/user-attachments/assets/2ae41856-91e4-4a79-9b00-ca53babbea57" />
  
- After looking at the GET request, at the bottom, the productId and the storeId were being sent to retrieve the data from the server. So, I added the `whoami` commands after the storeId and got the current user's name: 

    <img width="927" height="650" alt="Screenshot 2025-12-23 091745" src="https://github.com/user-attachments/assets/dadf5935-1e31-49b0-ae3f-2e70189b31eb" />
