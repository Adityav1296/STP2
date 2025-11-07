# Mark The Flag

## Description
My friend make a website for his favourite, but the lyrics seem a little bit odd

http://tommytheduck.github.io/mckey

## Solve
**Flag:** `V1T{MCK-pap-cool-ooh-yeah}`

- On viewing the site for the first time, certain words in the given lyrics were in bold while the others were written normally.

    bold
- This seemed strange to me and I checked the code for those sections. There, I found that all these bold words were written separately in the <mark> tag. So I extracted all the words written in the tag and found the flag.

    mark