## Introduction

This is write-up for the challenge `picoCTF - plumbing`.
The goal is to obtain the flag from output of connection.
**Tools used:** Netcat, grep.

---

## Challenge Description

We are given the following host and port:
`jupiter.challenges.picoctf.org 4427`

---

## Solution

Firstly, we connect to this host:
``` bash
nc jupiter.challenges.picoctf.org 4427
```

And get the following output:
``` bash
nc jupiter.challenges.picoctf.org 4427 | more

Not a flag either
This is defintely not a flag
This is defintely not a flag
Not a flag either
This is defintely not a flag
I don't think this is a flag either
This is defintely not a flag
Again, I really don't think this is a flag
Again, I really don't think this is a flag
Not a flag either
This is defintely not a flag
I don't think this is a flag either
Not a flag either
Again, I really don't think this is a flag
This is defintely not a flag
Not a flag either
This is defintely not a flag
Not a flag either
Again, I really don't think this is a flag
Not a flag either
Again, I really don't think this is a flag
Again, I really don't think this is a flag
Again, I really don't think this is a flag
Not a flag either
--More--
```

The output is huge, it will be time-consuming to read this all. So we will use grep:
``` bash
nc jupiter.challenges.picoctf.org 4427 | grep "picoCTF"
picoCTF{digital_plumb3r_5ea1fbd7}
```
