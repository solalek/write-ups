## Introduction

This is a write-up for the challenge `picoCTF - information`.
The goal is to obtain the flag from image metadata.
**Tools used:** hexdump, exiftool, base64.

---

## Challenge Description

We are given a jpeg image file: cat.jpg.

---

## Solution

Firstly, we check the file information:
``` bash
file cat.jpg
cat.jpg: JPEG image data, JFIF standard 1.02, aspect ratio, density 1x1, segment length 16, baseline, precision 8, 2560x1598, components 3
```

As we can see, it is a valid jpeg image. But when we have to work with the image file. It's good idea to open it and look at its metadata:
``` bash
exiftool cat.jpg                                                                                                2m
ExifTool Version Number         : 13.21
File Name                       : cat.jpg
Directory                       : .
File Size                       : 878 kB
File Modification Date/Time     : 2021:03:16 00:24:46+06:00
File Access Date/Time           : 2025:03:06 12:13:31+05:00
File Inode Change Date/Time     : 2025:03:06 12:13:31+05:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.02
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Current IPTC Digest             : 7a78f3d9cfb1ce42ab5a3aa30573d617
Copyright Notice                : PicoCTF
Application Record Version      : 4
XMP Toolkit                     : Image::ExifTool 10.80
License                         : cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9
Rights                          : PicoCTF
Image Width                     : 2560
Image Height                    : 1598
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 2560x1598
Megapixels                      : 4.1
```

We can notice intresting line in License field - `cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9`, which looks like base 64 encoded string.
``` bash
echo "cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9" | base64 -d
picoCTF{the_m3tadata_1s_modified}
```


