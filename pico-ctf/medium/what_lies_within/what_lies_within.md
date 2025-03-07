## Introduction

This is write-up for the challenge `picoCTF - What Lies Within`.
The goal is to obtain the flag from a PNG file.
**Used tools:** file, exiftool, binwalk, strings.

---

## Solution

First, we look at the file details:
``` bash
⟩ file buildings.png
buildings.png: PNG image data, 657 x 438, 8-bit/color RGBA, non-interlaced

⟩ exiftool buildings.png
ExifTool Version Number         : 13.21
File Name                       : buildings.png
Directory                       : .
File Size                       : 625 kB
File Modification Date/Time     : 2020:10:27 00:30:20+06:00
File Access Date/Time           : 2025:03:07 15:51:08+05:00
File Inode Change Date/Time     : 2025:03:07 15:51:08+05:00
File Permissions                : -rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 657
Image Height                    : 438
Bit Depth                       : 8
Color Type                      : RGB with Alpha
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Image Size                      : 657x438
Megapixels                      : 0.288
```

Then, we check hex and hiden files.
``` bash
⟩ strings buildings.png | grep "pico"

⟩ binwalk -e buildings.png

                        /home/solalek/ctf/pico-ctf/what_lies_within/extractions/buildings.png
---------------------------------------------------------------------------------------------------------------------
DECIMAL                            HEXADECIMAL                        DESCRIPTION
---------------------------------------------------------------------------------------------------------------------
0                                  0x0                                PNG image, total size: 625219 bytes
---------------------------------------------------------------------------------------------------------------------
[#] Extraction of png data at offset 0x0 declined
---------------------------------------------------------------------------------------------------------------------

Analyzed 1 file for 85 file signatures (187 magic patterns) in 3.0 milliseconds
```
``` bash
⟩ ls extractions/
 buildings.png
```

Next, we look at LSB-steganography:
``` bash
⟩ zsteg buildings.png
b1,r,lsb,xy         .. text: "^5>R5YZrG"
b1,rgb,lsb,xy       .. text: "picoCTF{h1d1ng_1n_th3_b1t5}"
b1,abgr,msb,xy      .. file: OpenPGP Secret Key
b2,b,lsb,xy         .. text: "XuH}p#8Iy="
b3,abgr,msb,xy      .. text: "t@Wp-_tH_v\r"
b4,r,lsb,xy         .. text: "fdD\"\"\"\" "
b4,r,msb,xy         .. text: "%Q#gpSv0c05"
b4,g,lsb,xy         .. text: "fDfffDD\"\""
b4,g,msb,xy         .. text: "f\"fff\"\"DD"
b4,b,lsb,xy         .. text: "\"$BDDDDf"
b4,b,msb,xy         .. text: "wwBDDDfUU53w"
b4,rgb,msb,xy       .. text: "dUcv%F#A`"
b4,bgr,msb,xy       .. text: " V\"c7Ga4"
b4,abgr,msb,xy      .. text: "gOC_$_@o"
```
