---
layout: post
title: "Pro-tip - Show Hidden Folder in Finder"
category: tipntricks
comments: true
---

Recently, I wanted to install a new stencil for Omnigraffle, and the suggested
installation folder was __~/Library/Application Support/OmniGraffle/Stencils__ directory.
So, I proceeded to download the stencil zip file in my __Downloads__ folder, and opened 
Finder to move the file from the __Downloads__ folder to the suggested folder, and to my
surprise I couldn't find the __Library__ folder anywhere in my home directory in Finder.

![hidden][1] 

I realized that __Library__ is a hidden folder and is not displayed by default in Finder, but
I couldn't figure out how to display and access this folder in Finder. Now, I could have 
used the terminal and just copied the file using the cp command, but I wanted to know if I
can do this using Finder. So I used Google-fu to find the solution to this problem, and to
my amazement the solution is clunky at best, and unintuitive at worst. So, to access the hidden
folder, you have to open Finder, and select the relevant parent folder that contains the
hidden folder. In my case the hidden folder Library was inside my home directory, so I
selected the home directory. Then select the __Go__ option in the Finder menu bar, and click 
__alt__(also called Option ⌥) key to show the hidden folder(s).


![hidden][2] 

Isn't Mac supposed to be user-friendly for non-technical folks?

[1]: http://i.imgur.com/1za9rY5.jpg?1
[2]: http://imgur.com/yFfjJl5.jpg?1 
