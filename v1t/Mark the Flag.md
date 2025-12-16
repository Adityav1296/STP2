# Mark The Flag

## Description
My friend make a website for his favourite, but the lyrics seem a little bit odd

http://tommytheduck.github.io/mckey

## Solve
**Flag:** `V1T{MCK-pap-cool-ooh-yeah}`

- On viewing the site for the first time, certain words in the given lyrics were in bold while the others were written normally.

    <img width="363" height="85" alt="Screenshot 2025-11-07 164553" src="https://github.com/user-attachments/assets/58088ab4-6a81-4ff4-8bf1-d34f7b8395f8" />

- This seemed strange to me and I checked the code for those sections. There, I found that all these bold words were written separately in the `<mark>` tag. So I extracted all the words written in the tag and found the flag.

    <img width="596" height="213" alt="Screenshot 2025-11-07 164642" src="https://github.com/user-attachments/assets/678f4a88-3cfa-46b1-abb6-de277cd41ef1" />
