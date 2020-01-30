---
title: Write up Part 1 for Cipher Combat Beginners 2020-Reverse Engineering
tags: [CTF, Cyber Security, Hackerearth]
style: 
color: 
description: Writup for reverse Engineering challenges for cipher combat ctf held on 22 january 2020.
---
<a class="text-center" href="" onclick="window.open(this.href, 'subscribe',
    'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;">Subscribe for New Posts</a>

It was one more challenging and adventurous journey for me. I wanted to get my hands on the **cyber sec tools and techniques** to face some real world problems and scenarios. And I was ruminating from where to start. Since, I am a self-taught programmer and therefore I know that when I jump to some new domain it is always painful in the beginning as there is no clue or trail. Google always helps but in a diverse ways. But when you’re committed enough to learn then everything just becomes a cakewalk afterwards. There is a saying, _"When you want something badly and that for the welfare of human kind; the universe conspires to bring that to you in mysterious ways.”_

I got this chance as I saw a challenge hosted by [hackerearth for cyber sec beginners](https://ciphercombatforbeginners.hackerearth.com/). I had no clue how to begin with but I wanted to begin with; so I registered for the challenge. I am blessed that I managed to get [32nd rank](https://cybersec.hackerearth.com/users/1285) where 1598 people registered. However, rank is not that important for me as this was my first attempt and first time I applied for any live challenge. The thing that mattered for me the most was the journey. I learnt a lot of new things while appearing for the challenge. I will talk about what this challenge (Capture the flag) is all about in some article. This article is about the challenge problems which I solved. So, I will share some of the write ups and useful links which will help me and someone like me who is not sure how to proceed with these challenges.

### 1. Reverse Password and Tic-Tac-Toe Challenges
In these two challenges, we were given a binary file **_rev_** and  from where we have to capture the flag.
##### Things I learnt
1. Check the type of file using `file` [command](https://www.computerhope.com/unix/ufile.htm).
```
$ file TicTacToe 
TicTacToe: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=b0ab92b6d5cf556d432de814dc8e7ab26b3974da, not stripped
```
2. Using the `strings` [command](http://www.linfo.org/strings.html) to return each string of printable characters in a file.
```
$ strings TicTacToe
X always goes first, and in the event that no one has three in a row,
        the game ends in a draw and is called a stalemate.
        Enter your move in coordinate form ex:(1,3) 
        Your Move: 
        *****Human Player Wins*****
        Computer Move: 
        *****Computer Wins*****
        *****Stalemate*****
        HE{You_are_a_tic_tac_toe_Ch@mpion}
```
The flag(HE{You_are_a_tic_tac_toe_Ch@mpion}) is captured successfully.
