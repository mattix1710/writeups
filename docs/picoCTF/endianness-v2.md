---
tags:
    - forensics
    - picoCTF2024
---

!!! abstract "Description"
    Here's a file that was recovered from a 32-bits system that organized the bytes a weird way. We're not even sure what type of file it is. Download it [here](https://artifacts.picoctf.net/c_titan/115/challengefile) and see what you can get out of it

You get a strange file with some gibberish data extracted by `strings` command. Maybe some steganography involved?

Ha ha. No. 🙄

**Fortunately**, there are clues hidden in the title and the description!

At first, we have `endian` - there are two main binary notations: **Big and Little Endian** [(wiki)](https://en.wikipedia.org/wiki/Endianness). 

The author mentions in the description that the file was written in some _weird way_ in **32-bit system**. This gives us the clue of the need of changing the byte order from one Endian representation to the other.

By searching through the Internet I also found [this](https://www.eecis.udel.edu/~davis/cpeg222/AssemblyTutorial/Chapter-15/ass15_3.html) website that describes the differences in examplary 32-bit system.

Following that, we can create a simple script that will input a file in binary format, reorder the bytes and output the solution:

``` py title="challenge.py"
with open("challengefile", "rb") as file:
    
    stream = file.read()
    endian_change = b''
    
    for it in range(0, len(stream), 4):
        chunk = stream[it:it+4]
        endian_change += chunk[::-1]
        
    with open("image.jpg", "wb") as img:
        img.write(endian_change)
```

The output file has `.jpg` extension because after reading the initial reordered bytes we can find out that these are the [magic numbers](https://www.garykessler.net/library/file_sigs.html) of images:
`b'\xff\xd8\xff\xe0\x00\x10JFIF\x00'` -> `FF D8 FF E0 00 10 4A 46 49 46 00`.

The flag is hidden in the image itself. The last part of the flag is probably generated automatically for each person - final flag CAN be different than provided.
