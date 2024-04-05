---
tags:
    - forensics
    - picoCTF2024
    - file_format
    - 100pt
---

## Description

!!! abstract "Description"
    The Network Operations Center (NOC) of your local institution picked up a suspicious file, they're getting conflicting information on what type of file it is. They've brought you in as an external expert to examine the file. Can you extract all the information from this strange file?
    Download the suspicious file [here](https://artifacts.picoctf.net/c_titan/8/flag2of2-final.pdf).

## Solve

The challenge itself is fairly easy. We are given a file with `.pdf` extension. It also opens as a PDF file, however examination with `file` command showed the information:
``` title="file command output"
flag2of2-final.pdf: PNG image data, 50 x 50, 8-bit/color RGBA, non-interlaced
```
Which indicates that there is possibly another file packed inside that one.

Opening the file in hex editor (e.g. Bless) confirms that there are two sets of magic numbers:

1. The beginning of the PNG file:

![magic numbers PNG](/assets/picoCTF/secret-of-the-polyglot/magic_num_PNG.png)

Which is written in hex as:
``` title="hex representation of PNG magic numbers"
89 50 4E 47 0D 0A 1A 0A -> ‰PNG....
```

2. The end of the PNG file and the beginning of the PDF file:

![magic numbers PDF](/assets/picoCTF/secret-of-the-polyglot/magic_num_PDF.png)

Which is written in hex as:
``` title="PNG trailer numbers"
49 45 4E 44 AE 42 60 82 -> IEND®B`‚
```

``` title="hex representation of PDF magic numbers"
25 50 44 46 -> %PDF
```

Since we know that there are multiple binary files concatenated in one file, we can try to extract it with some tools or extract the binary data itself and save it to the new file.

!!! tip "Quick type change"
    There is also a neat quick trick that handles the file type - extension. If the user changes the extension of the file, as in this example from `.pdf` to `.png` or otherwise, the different parts of the file will be read by the appropriate applications to open such files.

    In short - we can just read the part of the flag from `.pdf` section and then change the extension to `.png` and read the data from there to uncover the flag!

    Magnificient! 🧐

## Flag

``` title="flag"
picoCTF{f1u3n7_1n_pn9_&_pdf_2a6a1ea8}
```