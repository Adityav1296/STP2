# Stylish Flag

## Description
Are you a front end dev ?

https://tommytheduck.github.io/stylish_flag/

## Solve
**Flag:** `V1Y{H1D3OUT_CSS}`

- Here, when I checked the code for the challenge site, I found a hidden class flag. And on checking the css file, there were separate settings given for that flag class. 

    <img width="781" height="148" alt="Screenshot 2025-11-07 163310" src="https://github.com/user-attachments/assets/33a0de5d-960f-4c6c-bf9a-289600bd4ddc" />

    <img width="758" height="277" alt="Screenshot 2025-11-07 163321" src="https://github.com/user-attachments/assets/85b13323-9f88-4f0e-9c7f-1f57df0f4bec" />

- So, first I removed the hidden keyword from the code. Then I deleted all the other elements of the css file except the flag one. In the flag element, I set the opacity to 1 and removed the rotation to get the flag.

    <img width="1592" height="273" alt="Screenshot 2025-11-07 163242" src="https://github.com/user-attachments/assets/2a91d153-7d29-45a4-ac82-455dbb367e1e" />
