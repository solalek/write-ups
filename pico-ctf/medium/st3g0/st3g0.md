## Introduction

This is write-up for the challenge `picoCTF - st3g0`.
The goal is to obtain the flag from a PNG image file.
**Tools used:** zsteg.

--

## Solution

First, we look at image details.
``` bash
⟩ file pico.flag.png
pico.flag.png: PNG image data, 585 x 172, 8-bit/color RGBA, non-interlaced

⟩ exiftool pico.flag.png
ExifTool Version Number         : 13.21
File Name                       : pico.flag.png
Directory                       : .
File Size                       : 13 kB
File Modification Date/Time     : 2023:08:05 02:36:13+06:00
File Access Date/Time           : 2025:03:06 15:54:21+05:00
File Inode Change Date/Time     : 2025:03:06 15:54:21+05:00
File Permissions                : -rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 585
Image Height                    : 172
Bit Depth                       : 8
Color Type                      : RGB with Alpha
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Image Size                      : 585x172
Megapixels                      : 0.101
```

As we can see, there is nothing, even in the hex code.
``` bash
⟩ strings pico.flag.png | grep picoCTF
```

Next we can look at hiden files.
``` bash
⟩ binwalk -e pico.flag.png

                             /home/solalek/ctf/pico-ctf/st3g0/extractions/pico.flag.png
---------------------------------------------------------------------------------------------------------------------
DECIMAL                            HEXADECIMAL                        DESCRIPTION
---------------------------------------------------------------------------------------------------------------------
0                                  0x0                                PNG image, total size: 13438 bytes
---------------------------------------------------------------------------------------------------------------------
[#] Extraction of png data at offset 0x0 declined
---------------------------------------------------------------------------------------------------------------------

Analyzed 1 file for 85 file signatures (187 magic patterns) in 3.0 milliseconds
```

``` bash
⟩ ls extractions/
 pico.flag.png
```

As result, we again have found nothing.
Then we will check LSB-steganography.
``` bash
⟩ zsteg pico.flag.png
b1,r,lsb,xy         .. text: "~__B>wV_G@"
b1,rgb,lsb,xy       .. text: "picoCTF{7h3r3_15_n0_5p00n_96ae0ac1}$t3g0"
b1,abgr,lsb,xy      .. text: "E2A5q4E%uSA"
b2,b,lsb,xy         .. text: "AAPAAQTAAA"
b2,b,msb,xy         .. text: "HWUUUUUU"
b4,r,lsb,xy         .. file: Targa image data (16-273) 65536 x 4097 x 1 +4352 +4369 - 1-bit alpha - right "\021\020\001\001\021\021\001\001\021\021\001"
b4,g,lsb,xy         .. file: 0420 Alliant virtual executable not stripped
b4,b,lsb,xy         .. file: Targa image data - Map 272 x 17 x 16 +257 +272 - 1-bit alpha "\020\001\021\001\021\020\020\001\020\001\020\001"
b4,bgr,lsb,xy       .. file: Targa image data - Map 273 x 272 x 16 +1 +4113 - 1-bit alpha "\020\001\001\001"
b4,rgba,msb,xy      .. file: Applesoft BASIC program data, first line number 8
```

