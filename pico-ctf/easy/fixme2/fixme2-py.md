## Introduction

This is write-up for the challenge `picoCFT - fixme2.py`
The goal is to fix syntax error in the Python script to print the flag.
**Tools used**: Code editor, Python.

---

## Challenge Description

We are given the following Python script:

``` python
import random

def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)        
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])

flag_enc = chr(0x15) + chr(0x07) + chr(0x08) + chr(0x06) + chr(0x27) + chr(0x21) + chr(0x23) + chr(0x15) + chr(0x58) + chr(0x18) + chr(0x11) + chr(0x41) + chr(0x09) + chr(0x5f) + chr(0x1f) + chr(0x10) + chr(0x3b) + chr(0x1b) + chr(0x55) + chr(0x1a) + chr(0x34) + chr(0x5d) + chr(0x51) + chr(0x40) + chr(0x54) + chr(0x09) + chr(0x05) + chr(0x04) + chr(0x57) + chr(0x1b) + chr(0x11) + chr(0x31) + chr(0x0d) + chr(0x5f) + chr(0x05) + chr(0x40) + chr(0x04) + chr(0x0b) + chr(0x0d) + chr(0x0a) + chr(0x19)
  
flag = str_xor(flag_enc, 'enkidu')

# Check that flag is not empty
if flag = "":
  print('String XOR encountered a problem, quitting.')
else:
  print('That is correct! Here\'s your flag: ' + flag)
```

---

## Solution

The issue in the given code is a syntax error in the if statement:
``` python
# Check that flag is not empty
if flag = "":     # here only one '=' which is incorrect syntax.
  print('String XOR encountered a problem, quitting.')
```

In Python, '=' is used for assignment, while == is used for comparison.
So correct syntax is:
``` python
# Check that flag is not empty
if flag == "":    # here is correct syntax.
  print('String XOR encountered a problem, quitting.')
```

