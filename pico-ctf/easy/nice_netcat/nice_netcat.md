## Introduction

This is write-up for the challenge `picoCTF - Nice netcat...`.
The goal is to obtain flag from the netcat connection.
**Tools used:** Netcat, Python.

---

## Challenge Description

We are given the following host and port:
`mercury.picoctf.net 22342`

---

## Solution

Firstly, we connect to this host using netcat:
``` bash
nc mercury.picoctf.net 22342
```

Which gives the following output:
``` bash
⟩ nc mercury.picoctf.net 22342                                                                                 11.2s
112
105
99
111
67
84
70
123
103
48
48
100
95
107
49
116
116
121
33
95
110
49
99
51
95
107
49
116
116
121
33
95
53
102
98
53
101
53
49
100
125
10
```

So then we should store this output to use it later:
``` bash
nc mercury.picoctf.net 22342 > letters
```

To convert these numbers into characters, we will use a Python script:
``` python
with open("letters", "r") as file:
     list_letters = file.readlines()
     for i in list_letters:
         print(chr(int(i)), end="")
```

Result:
``` bash
python translate.py 
picoCTF{g00d_k1tty!_n1c3_k1tty!_5fb5e51d}
```
