# Stylish Flag

## Description
Are you a front end dev ?

https://tommytheduck.github.io/stylish_flag/

## Solve
**Flag:** `V1Y{H1D3OUT_CSS}`

- Here, when I checked the code for the challenge site, I found a hidden class flag. And on checking the css file, there were separate settings given for that flag class. 

    hidden
    css
- So, first I removed the hidden keyword from the code. Then I deleted all the other elements of the css file except the flag one. In the flag element, I set the opacity to 1 and removed the rotation to get the flag.

    flag