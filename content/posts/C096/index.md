---
title: "CTF Learn 096 - Forensics 101"
date:  "2021-09-22T22:20:36+01:00"
tags: ["CTF", "CTFLearn"]
description: "Think the flag is somewhere in there. Would you help me find it? "
type: "posts"
---

# The challenge

> Think the flag is somewhere in there. Would you help me find it? 
> https://mega.nz/#!OHohCbTa!wbg60PARf4u6E6juuvK9-aDRe_bgEL937VO01EImM7c

Here is the image in the link:

{{<image src="challenge.jpg" position="center">}}

## Preliminary analysis

Clearly, the flag is embedded in the image somehow, but how do we get it out? There is a whole field of study about how to hide information in other messages or physical objects. It is called [steganography](https://en.wikipedia.org/wiki/Steganography) and is much older than computers, going back to 440 BC.

There are tons of ways to [hide information in digital files](https://en.wikipedia.org/wiki/Steganography#Digital_messages), but let's focus on the easy ones for now. The first thing to do is always to check the metadata of a file. The `file` command can give us more information about a given file.

```bash
> file image.jpg
image.jpg: JPEG image data, JFIF standard 1.01, aspect ratio, density 1x1, segment length 16, progressive, precision 8, 236x218, components 3
```

As you can see, the information given is very basic. However, it can be useful at identifying files that are "lying" about their identity, i.e have an extension that does not match the contents of the file. This is not the case right now but can be good to know in the future.

What about the image metadata? Most images contain a surprising amount of information about their origin as part of their metadata, maybe they hid the flag there! The `exif` command-line utility can be used to extract the EXIF headers from images, which contain the relevant metadata. I'll skip the details for now, but I'll talk about EXIF more in a following challenge where it takes center stage.

```bash
> exif image.jpg 
Corrupt data
The data provided does not follow the specification.
ExifLoader: The data supplied does not seem to contain EXIF data.
```

Well, it seems they removed the metadata before uploading the image... Time to search deeper!


## Solution

Another way to insert information in another file is to simply add it as part of the main body. How to do this without corrupting the file depends on the format of the file, but let's see if we can find the solution easily. The `strings` command is used to extract text information from binary files. Essentially, it will look for sequences of bits that can be translated into characters and return them as a list.

```bash
> strings image.jpg 
JFIF
 , #&')*)
-0-(0%()(
((((((((((((((((((((((((((((((((((((((((((((((((((
L?~f
:UwR
y>2|

[...]
```

This output isn't very helpful... There are way too many lines of garbage to sift through, we need a way to filter out the sequences of characters that could actually be useful. The `grep` command can pick out pieces of text that match a given expression. The code below will pipe the result from `strings` into `grep`, which will search for lines with the word "flag".

```bash
> strings image.jpg | grep flag
flag{wow!_data_is_cool}
```