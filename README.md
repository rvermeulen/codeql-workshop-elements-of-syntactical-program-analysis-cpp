# CodeQL workshop: Syntactical elements of C/C++

## Contents

- [CodeQL workshop: Syntactical elements of C/C++](#codeql-workshop-syntactical-elements-of-cc)
  - [Contents](#contents)
  - [Setup instructions](#setup-instructions)
  - [Workshop](#workshop)
    - [Learnings](#learnings)
    - [Linux Miscellaneous Driver](#linux-miscellaneous-driver)
  - [Exercises](#exercises)

## Setup instructions

- Install [Visual Studio Code](https://code.visualstudio.com/).
- Install the [CodeQL extension for Visual Studio Code](https://codeql.github.com/docs/codeql-for-visual-studio-code/setting-up-codeql-in-visual-studio-code/).
- (Optionally) Install [Docker](https://www.docker.com/) if you want to build your own CodeQL database.
- (Optionally) Install the [CodeQL CLI](https://github.com/github/codeql-cli-binaries/releases) if you want to build your own CodeQL database.
- Clone this repository:
  
  ```bash
  git clone https://github.com/rvermeulen/codeql-workshop-syntactical-elements-of-cpp
  ```

- Install the CodeQL pack dependencies using the command `CodeQL: Install Pack Dependencies` and select `exercises` and `solutions`.
- Download the [prebuilt database](https://drive.google.com/file/d/1upETVaHIwE9YnJHQcxW9bNJSWCClDmBg/view?usp=sharing) or build the database using the predefined Makefile by running `make`.
- Select the Vulnerable Linux Driver database as the current database by right-clicking on the file `vulnerable-linux-driver-db.zip` in the File explorer and running the command `CodeQL: Set Current Database`.

## Workshop

### Learnings

In this workshop you will learn how to describe syntactical elements of the C/C++ programming language.
With the goal of describing the entry point of the intentionally [vulnerable Linux driver](https://github.com/invictus-0x90/vulnerable_linux_driver) you will:

- Discover how QL represent C/C++ program elements.
- Learn query for program elements.
- Learn how to encapsulate descriptions of program elements using QL classes.

### Linux Miscellaneous Driver

The intentionally vulnerable Linux driver project implements a [Miscellaneous Character Driver](https://www.linuxjournal.com/article/2920) to expose multiple vulnerabilities to learn about Kernel driver exploitation.

The miscellaneous character driver was designed for use cases that require a small device driver to support custom hardware or software hacks.
To register or unregister a miscellaneous driver, the **misc** driver exports two functions for user modules, that can be found in the header [linux/miscdevice.h](https://github.com/torvalds/linux/blob/master/include/linux/miscdevice.h), called [misc_register](https://github.com/torvalds/linux/blob/8ca09d5fa3549d142c2080a72a4c70ce389163cd/include/linux/miscdevice.h#L91) and [misc_unregister](https://github.com/torvalds/linux/blob/8ca09d5fa3549d142c2080a72a4c70ce389163cd/include/linux/miscdevice.h#L92).

With the `misc_register` function as the starting point we will traverse function calls, expressions, structure definitions, and variable initializations to find the user module entrypoint. This entrypoint will be the start of future workshops that will discuss the vulnerabilities that can be found in this intentionally vulnerable Linux driver.


## Exercises