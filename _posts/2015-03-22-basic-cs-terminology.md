---
layout: post
title: Resource Sheet » Basic CS Terminology
category: technical
tags: [computer-science, resource]
---

<!-- more -->

A lot of computer science concepts were never covered during my time at Dev Bootcamp, which was expected. However, I'm still really interested in these basic concepts, and I feel that knowing them will solidify the foundation for my growth in the tech industry. So here is a quick resource sheet I put together to cover some of the basics.

- `Bit:` smallest unit of binary (0 or 1)
- `Byte:` 8 bits

Binary numbers are expressed in a **base-2 numeral system**.

- 2^0 = ones
- 2^1 = twos
- 2^2 = fours
- 2^3 = eights
- 2^4 = sixteens
- 2^5 = thirty-twos

Using that system, we can calculate the value of these bytes:

* 00000001 -> 1
* 00000010 -> 2
* 00000011 -> 3
* 00001010 -> 10

* * *

- `Hardware:` things like a desktop, tablet, smartphone; runs programs
- `Operating System:` a software program that allows the computer hardware to communicate with the computer software
- `RAM (Random Access Memory):` volatile memory; the data is temporary and is erased when the computer turns off
- `ROM (Read-Only Memory):` non-volatile memory; the data is permanently written and will still exist even after the computer turns off

When a computer is turned on, it loads data from the hard drive to RAM. When an application opens, the application's data loads into RAM. The application only loads essential items at first and then will load more data as the user requests it. Once you close the application, the data is purged from RAM to make room for other applications.

* * *

Software are computer instructions/data that run on your computer. It's "soft" because it changes/updates all the time.

There are two types:

* `Systems Software:` includes operating system and utilities that enable the computer to function
* `Application Software:` enable users to complete tasks; also known as *end-user programs*

* * *

* `Low-Level Programming Language:` machine language, no compiler/interpreter needed (think assembly and machine code)
* `High-Level Programming Language:` strong abstraction (think Ruby and Python)

To get from a high-level language to a low-level language, it needs to be compiled in order for machines to understand it.


* * *

**Client-Server Model**

* The *client* sends request to the server
* The *server* sends content back to the client


* * *

A *paradigm* is a way of doing something so a **programming paradigm** is a way or style of programming.


* `Procedural:` step by step procedure we should follow to solve a specific problem (C)
* `Functional:` look at programs like mathematical programs; outputs depend solely on the input and no other factors/side effects (racket)
* `Object-Oriented:` uses code objects to mirror real world objects; they have *attributes* (data associated with that object) and *methods* (actions that can be performed on the object and its attributes)

