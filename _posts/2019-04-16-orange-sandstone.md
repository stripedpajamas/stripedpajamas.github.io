---
layout: default
title:  "orange sandstone"
date:   2018-04-19 21:30:00 -0400
excerpt: making communication difficult
categories: nonsense
---
# orange sandstone

### part i
sometimes i like to type to my friends without vowels (unless the vowel starts the word) because i think it's fun and mostly readable. instead of typing `that looks dope`, i would type `tht lks dp`. 

last week my friend said we should write a library that reverses the process. that's kind of tricky because there's unavoidable ambiguity when you remove (most of) the vowels from a word. cliche example: `wrd` could with equal certainty represent `word`, `weird`, `wired`, and `ward`. maybe others. 

the implementation idea (which doesn't necessarily make loads of sense) was to filter a wordlist down to words that have the same voweless representation, and then take the word whose double metaphone has the lowest averaged levenshtein distance to the input word's double metaphone. something like that. implementation in javascript [here](https://github.com/stripedpajamas/voweler).

originally the wordlist was an english dictionary (around 500k words i think), but that performed horribly. later i swapped it out with the 20k version of [this wordlist](https://github.com/first20hours/google-10000-english). although this wordlist is much smaller, since it's sorted by frequency of use it performs much better.

```bash
$ npx voweler th nw wrdlst prfrms mch bttr
the nw ? performs much better
```

words that have no match in the dictionary are replaced with `?`. 

### part ii
i was at a team outing and a teammate started talking to me about video games. i said i don't really play but he wanted to let me know he was struggling with "a particularly difficult game". cool dude. he said it was called "dark souls". this was shortly after i found a $20 bill on the ground. idk if there's a connection.

i looked up dark souls [on wikipedia](https://en.wikipedia.org/wiki/Dark_Souls) and read through a little of the article. i wanted to be impressed. the introductory paragraph was lackluster. no matter. there's a part where the article says:

> Communication between players is deliberately restricted. Outside of character gestures, the only other communication players have with one another comes by way of orange soapstones, which allow players to write limited messages that can be read by others in the same area.

that got me going. i was thinking how about a chat app that deliberately restricts communication called 'orange soapstone'.

### part iii
the incorrigible conclusion:
- alice sends a message to bob in orange soapstone client
- server removes vowels from message, and then sends it through `voweler` to repopulate the vowels
- bob receives message from server and is maybe confused maybe not

i forked a socket.io demo on glitch and added one line of translation logic to the server code:

https://orange-sandstone.glitch.me/

fin.
