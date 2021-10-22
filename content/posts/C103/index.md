---
title: "CTF Learn 103 - Taking LS"
date:  "2021-09-23T21:47:21+01:00"
tags: ["CTF", "CTFLearn"]
description: "Just take the Ls. Check out this zip file and I bet the flag will remain hidden."
draft: true
---

# The challenge

> Just take the Ls. Check out this zip file and I bet the flag will remain hidden. 
> https://mega.nz/#!mCgBjZgB!_FtmAm8s_mpsHr7KWv8GYUzhbThNn0I8cHMBi4fJQp8


The link will lead you to a zip file, which has the following structure:

```
The Flag.zip/
├── _MACOSX/ 
└── The Flag/
    └── The Flag.pdf
```

## Solution

The PDF likely contains the flag we need, but it is locked behind a password. Where is it then? The `_MACOSX` folder isn't very helpful. It is just an special folder MacOS creates when it zips files. There is no useful data in it and it is completely ignored in other operational systems. Most MacOS users will never notice it because it is hidden by the file manager.

The title is a play on the phrase "Talking BS", but referencing `ls`. This is an Unix command that list the files in a certain directory. The equivalent in Windows would be `dir`. Executing `ls` on the `The Flag` folder returns the following result:

```bash
> ls
'The Flag.pdf'
```

Not very helpful... Maybe we are missing something, lets look at the `ls` manual. You can search it online, or you can use the `man` command to open it directly on the command-line.

```bash
> man ls
LS(1)           User Commands           LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List  information  about the FILEs (the current directory by default).  
       Sort entries alphabetically if none of
       -cftuvSUX nor --sort is specified.

       Mandatory arguments to long options are mandatory for short options too.

       -a, --all
              do not ignore entries starting with .

       -A, --almost-all
              do not list implied . and ..

[...]
```

The first options entry reveals something interesting, `ls` ignores files that start with "." such as `.file.txt`. To include those you must add either `-a` or `--all` to the command.

```bash
> ls -a
.   ..   .DS_Store  'The Flag.pdf'   .ThePassword
```

That's more like it! But why were so many folders and files hidden even with the command? In short, because you shouldn't mess with them. Files starting with a "." (usually called dotfiles) are often configurations files created by programs or they may contain data used for other proposes. Showing these files would clutter the interface of a file manager and users who don't know their purpose may decide to alter or delete them. Even for programmers or tech savvy people, dotfiles are usual unimportant and so are hidden in `ls`. These are not the only kind of hidden file (`_MACOSX` is an example of that), but they are one of the most important.

However, there are legitimate reasons for interacting with them. Because of that `ls` and most file managers have settings for showing these files. Lets check what is inside `.ThePassword` with `ls -a`.

```bash
> ls -a .ThePassword
.  ..  ThePassword.txt
```

To get the text inside `ThePassword.txt`, lets use the `cat` tool. It will read the contents of the file and output them to the console

```bash
> cat .ThePassword/ThePassword.txt
Nice Job!  The Password is "Im The Flag".
```

And we are done! Use the password on the PDF and get the flag.