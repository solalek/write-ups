## Introduction

This is a write-up for the challenge `picoCTF - Disk, disk, sleuth!`.
The goal is to obtain the flag from the disk image.
**Tools used:** gunzip, srch_strings.

---

## Challenge Description

Use `srch_strings` from the sleuthkit and some terminal-fu to find a flag in this disk image: dds1-alpine.flag.img.gz

---

## Solution

First, we extract files using `gunzip`.
``` bash
gunzip dds1-alpine.flag.img.gz
```

Then, we search for strings with the flag using `srch_strings`, as it's said in the description.
``` bash
srch_strings -f dds1-alpine.flag.img | grep "pico"                                                            5.2s
dds1-alpine.flag.img: ffffffff81399ccf t pirq_pico_get
dds1-alpine.flag.img: ffffffff81399cee t pirq_pico_set
dds1-alpine.flag.img: ffffffff820adb46 t pico_router_probe
dds1-alpine.flag.img:   SAY picoCTF{f0r3ns1c4t0r_n30phyt3_dcbf5942}
```
