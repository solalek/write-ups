## Introduction

This is a write-up for the challenge `picoCTF - extensions`.
The goal is to obtain the flag from the given file.
**Tools used:** chafa, file.

---

## Challenge Description

'This is a really weird text file TXT? Can you find the flag?'

We are given a file: flag.txt.

---

## Solution

First, we look at details of file.
``` bash
file flag.txt
flag.txt: PNG image data, 1697 x 608, 8-bit/color RGB, non-interlaced
```

And can notice that this is acually the PNG image.
To see the image you can use any image viewer.
``` bash
chafa flag.txt
```

Flag: picoCTF{now_you_know_about_extensions}
