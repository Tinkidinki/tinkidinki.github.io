---
title: Fuzzy Search and File Launch
layout: post
tags: all-posts 
usemathjax: true
published: false
---

An act that I detest but have not done anything about is *walking down directories*--for opening a file, or for saving a downloaded file. I know I'm late to the party, but today, I hope to make opening files and applications easier for myself. I'm currently on Ubuntu, and use the [i3 tiling window manager](https://i3wm.org/).


## Envisioning what I need
Ideally, right after I boot, I would want:
1. A keyboard shortcut that opens up a small terminal in which I can search for any file or application
2. The search itself must be fuzzy (and fast)
3. If I select an application, the application must launch
4. If I select a file, the file must open with the default application
5. The terminal must close after this process

I find that [dmenu-extended](https://github.com/MarkHedleyJones/dmenu-extended) performs all of this---except that its search is horribly slow. Meanwhile, [fzf](https://github.com/junegunn/fzf) offers "blazing fast" fuzzy search, while not being tailored to my needs. Can we do something about this? 





