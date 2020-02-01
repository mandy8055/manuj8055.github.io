---
title: Write up Part 1 for Cipher Combat Beginners 2020-Reverse Engineering
tags: [CTF, Cyber Security, Hackerearth]
style: 
color: 
description: Writup for reverse Engineering challenges for cipher combat ctf held on 22 january 2020.
---
<a class="text-center" href="https://feedburner.google.com/fb/a/mailverify?uri=Mandy8055&amp;loc=en_US" onclick="window.open(this.href, 'subscribe',
    'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;">Subscribe for New Posts</a>

It was one more challenging and adventurous journey for me. I wanted to get my hands on the **cyber sec tools and techniques** to face some real world problems and scenarios. And I was ruminating from where to start. Since, I am a self-taught programmer and therefore I know that when I jump to some new domain it is always painful in the beginning as there is no clue or trail. Google always helps but in a diverse ways. But when you’re committed enough to learn then everything just becomes a cakewalk afterwards. There is a saying, _"When you want something badly and that for the welfare of human kind; the universe conspires to bring that to you in mysterious ways.”_

I got this chance as I saw a challenge hosted by [hackerearth for cyber sec beginners](https://ciphercombatforbeginners.hackerearth.com/). I had no clue how to begin with but I wanted to begin with; so I registered for the challenge. I am blessed that I managed to get [32nd rank](https://cybersec.hackerearth.com/users/1285) where 1598 people registered. However, rank is not that important for me as this was my first attempt and first time I applied for any live challenge. The thing that mattered for me the most was the journey. I learnt a lot of new things while appearing for the challenge. I will talk about what this challenge (Capture the flag) is all about in some article. This article is about the challenge problems which I solved. So, I will share some of the write ups and useful links which will help me and someone like me who is not sure how to proceed with these challenges.

### 1. Reverse Password and TicTacToe Challenges
In these two challenges, we were given a binary file **_rev_** and  from where we have to capture the flag.
##### Things I learnt
1. Check the type of file using `file` [command](https://www.computerhope.com/unix/ufile.htm).
```bash
$ file TicTacToe 
TicTacToe: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=b0ab92b6d5cf556d432de814dc8e7ab26b3974da, not stripped
```
2. Using the `strings` [command](http://www.linfo.org/strings.html) to return each string of printable characters in a file.
```bash
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

### 2. Shifter Challenge
In this challenge, we were given a snapshot of the algorithm and we need to find what the particular alogrithm was doing thereby collecting the flag. Unfortunately, I was not able to do this in the challenge but afterwards I realized, this is an assembly code for a function.

{% include elements/figure.html image="https://mandy8055.github.io/assets/2020-01-31-shifter-challenge.png" caption="shifter.png" %}

#### Things I learnt

1. Meaning of [dword and qword](https://en.wikipedia.org/wiki/Word_(computer_architecture)).
2. How the String is pushed to a Stack in Assembly language. If we'll notice, we'll see a string specifically **`rf.bo'b/$ke`** is being pushed onto the stack.
3. If we further analyse the code; it can be clearly see that **var_4h** is being compared to **0xb** (hex value for 11) which seems to be the **length** of the encrypted flag. I did not know the meaning of this thing so I got blocked here and was unable to crack this problem. But thanks to @VikasGola for his [wonderful writeup](https://vikasgola.github.io/blog/cipher-combat-beginners-2020) on this topic.**So It meant that there is a loop on the encrypted flag.**
4. In the last block every character of the encrypted flag is added with the increment variable of the loop (i.e. eax).
5. So finally writing a java code for the above logic; we can capture the flag.

```java
public static void main(String args[]) {
      String st = "rf.bo'b/$ke";
      String resFlag = "";
      for(int i = 0; i < st.length(); i++){
         resFlag += (char)(st.charAt(i) + (i + 1));
      }
      System.out.println(resFlag);
    }
```
The flag `sh1ft-i7-up` is captured successfully.
### 3. ThreeWords
In this challenge, we were given a binary file and were asked to give the arguments which are required to run this file. This was also the same file as rev and tictactoe file discussed previously.

##### Things I learnt
1. **INTRODUCTION TO [GHIDRA](https://github.com/NationalSecurityAgency/ghidra) AND ITS USAGE**: This was one of the crucial learning from this challenge. This software is amazing and worth giving a try. It has various functionalities which I haven't yet explored but the one which I did was disassembly. It disassembles the binary file back to its code. The code is same which was initially written only with variable names replaced. So live; you can almost taste'em.:nerd_face:
2. We can use `objdump` [command](https://linux.101hacks.com/unix/objdump/) too which can also fulfill the same result. But as a beginner Ghidra was really helpful. I will share how to set-up ghidra soon.
3. Once, I imported the `threewords` binary in ghidra; it disassembled the binary and when clicking main method it showed the below output.
4. From the image it is evident that in the argument **racker** is being compared with **talkative** and finally **mainstream** is being passed.

{% include elements/figure.html image="https://mandy8055.github.io/assets/2020-02-01-three-words.png" caption="Image From Ghidra" %}

Hence, the flag `HE{racker_talkative_mainstream}` is captured successfully.
  