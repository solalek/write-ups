## Introduction

This is write-up for the challenge `picoCTF convertme.py`.
The goal is to convert a decimal number into a binary number using a Python script.
**Tools used:** python.

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


flag_enc = chr(0x15) + chr(0x07) + chr(0x08) + chr(0x06) + chr(0x27) + chr(0x21) + chr(0x23) + chr(0x15) + chr(0x5f) + chr(0x05) + chr(0x08) + chr(0x2a) + chr(0x1c) + chr(0x5e) + chr(0x1e) + chr(0x1b) + chr(0x3b) + chr(0x17) + chr(0x51) + chr(0x5b) + chr(0x58) + chr(0x5c) + chr(0x3b) + chr(0x42) + chr(0x57) + chr(0x5c) + chr(0x0d) + chr(0x5f) + chr(0x06) + chr(0x46) + chr(0x5c) + chr(0x13)


num = random.choice(range(10,101))

print('If ' + str(num) + ' is in decimal base, what is it in binary base?')

ans = input('Answer: ')

try:
  ans_num = int(ans, base=2)
  
  if ans_num == num:
    flag = str_xor(flag_enc, 'enkidu')
    print('That is correct! Here\'s your flag: ' + flag)
  else:
    print(str(ans_num) + ' and ' + str(num) + ' are not equal.')
  
except ValueError:
  print('That isn\'t a binary number. Binary numbers contain only 1\'s and 0\'s')
```
## Analysis
1. The script randomly selects a number between 10 and 100.
2. It asks the user to convert a number to binary.
3. If the conversion is correct. the script XOR-decrypts flag_end using the key "enkidu" and prints the flag.

---

## Solution

To gain the flag we should convert a decimal number into a binary number. 
We can do it manually or using Python.

``` bash
⟩ python
>>> print(bin(67))
0b1000011
```

So now we can pass the number.

``` bash
⟩ python convertme.py
If 67 is in decimal base, what is it in binary base?
Answer: 1000011
That is correct! Here's your flag: picoCTF{4ll_y0ur_b4535_722f6b39}
```
