---
title: "CTF Learn 103 - Taking LS"
date:  "2021-09-23T21:47:21+01:00"
cover: ""
tags: ["", ""]
description: ""
draft: true
---

# The challenge

> Just take the Ls. Check out this zip file and I bet the flag will remain hidden. 
> https://mega.nz/#!mCgBjZgB!_FtmAm8s_mpsHr7KWv8GYUzhbThNn0I8cHMBi4fJQp8


The link will lead you to a zip file, which will create the following structure when unzipped:

```
The Flag.zip/
├── _MACOSX/ 
└── The Flag/
    └── The Flag.pdf
```

The _MACOSX folder may not appear if you are using MacOS.

## Solution

The PDF in the folder likely contains the flag we need, but it is locked behind a password. Where is it then? The _MACOSX folder isn't very helpful. It is just an special folder MacOS creates when it zips files. There is no useful data in it and it is completely ignored in other operational systems. Most MacOS users will never notice it because it is hidden by the file manager... 

But what if something else is hidden by the file manager? The title is a play on the phrase "Talking BS", but referencing LS