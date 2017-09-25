---
layout: default
title:  "go get failure"
excerpt: Loops tripping me up
categories: code
---

## go get failure

Over the weekend I was trying to solve one of the Cryptopals challenges in Go. I 
wrote this `for` loop to make a dictionary of the last byte of a block (see the details of 
Challenge 12 [here](http://cryptopals.com/sets/2/challenges/12) if you're interested):

```go
var j byte = 0
for ; j <= 255; j++ {
    dicPayload := append(bytes.Repeat([]byte("A"), blockSize-i), j)
    dicCiphertext := EncryptWithSecret(dicPayload)[:blockSize]
    dic[j] = dicCiphertext
}
fmt.Println("Done with loop")
```

I ended up doing it a little differently in the end but here's the thing, my code got stuck
here and never printed "Done with loop". Ever. Man I was adding `fmt.Println`'s everywhere. Just seemed
like something was resetting `j` within the loop so it would never progress. 

So I go about carefully analyzing those three inner lines. Like a hawk looking for some munchies. You
know the feeling. Crazed. But those three lines don't do jack to mess with `j`.

1. Make a byte array that's `blocksize - i` length (`i` is an outer loop not in the snippet) and append
`j` to it
2. Encrypt it
3. Put that in my map (`dic` also not in the snippet)

Nothing messes with `j`. So frustrating. 

Then out of nowhere I realize the problem. `j++` on the last iteration (when `j = 255`) will set `j`
to 256, but `j` is a byte which is just an alias for uint8 and of course 256 doesn't fit in a byte so 
it sets `j` to 0 and the loop starts over. And over. And over.

So I changed the evalutaor to `j < 255` and that fixed it.