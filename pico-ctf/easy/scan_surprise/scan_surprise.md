## Introduction

This is write-up for challenge `picoCTF - scan surprise`.
The goal is to obtain the flag from image.
**Tools used:** zbar.

---

## Challenge Description

We are given the `challenge.zip` archive.

---

## Solution

To extract content from the zip archive, we will use `unzip` tool:
``` bash
unzip challenge.zip
Archive:  challenge.zip
   creating: home/ctf-player/drop-in/
 extracting: home/ctf-player/drop-in/flag.png
```

Next we scrutinize the `flag.png` and notice that this is `QR` code.
So we can just scan this.
``` bash
zbarimg flag.png
QR-Code:picoCTF{p33k_@_b00_3f7cf1ae}
scanned 1 barcode symbols from 1 images in 0 seconds
```
