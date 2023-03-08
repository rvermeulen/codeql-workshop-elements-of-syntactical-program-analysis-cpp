# CodeQL workshop: Syntactical elements of C/C++

## Contents

- [CodeQL workshop: Syntactical elements of C/C++](#codeql-workshop-syntactical-elements-of-cc)
  - [Contents](#contents)
  - [Setup instructions](#setup-instructions)
  - [Workshop](#workshop)
    - [Learnings](#learnings)
    - [Linux Miscellaneous Driver](#linux-miscellaneous-driver)
  - [Exercises](#exercises)
    - [Exercise 1](#exercise-1)
    - [Exercise 2](#exercise-2)
    - [Exercise 3](#exercise-3)
    - [Exercise 4](#exercise-4)
    - [Exercise 5](#exercise-5)
    - [Exercise 6](#exercise-6)
    - [Exercise 7](#exercise-7)
    - [Exercise 8](#exercise-8)
    - [Exercise 9](#exercise-9)
    - [Exercise 10](#exercise-10)
    - [Exercise 11](#exercise-11)

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
With the goal of describing the user-mode entry point of the intentionally [vulnerable Linux driver](https://github.com/invictus-0x90/vulnerable_linux_driver) you will:

- Discover how QL represent C/C++ program elements.
- Learn to query for program elements.
- Learn how to encapsulate descriptions of program elements using QL classes.

This workshop focusses on the syntactic parts. Some parts in this workshop can be generalized using more advanced techniques, such as dataflow analysis, that are covered in other workshops.

### Linux Miscellaneous Driver

The intentionally vulnerable Linux driver project implements a [Miscellaneous Character Driver](https://www.linuxjournal.com/article/2920) to expose multiple vulnerabilities to learn about Kernel driver exploitation.

The miscellaneous character driver was designed for use cases that require a small device driver to support custom hardware or software hacks.
To register or unregister a miscellaneous driver, the **misc** driver exports two functions for user modules, that can be found in the header [linux/miscdevice.h](https://github.com/torvalds/linux/blob/master/include/linux/miscdevice.h), called [misc_register](https://github.com/torvalds/linux/blob/8ca09d5fa3549d142c2080a72a4c70ce389163cd/include/linux/miscdevice.h#L91) and [misc_unregister](https://github.com/torvalds/linux/blob/8ca09d5fa3549d142c2080a72a4c70ce389163cd/include/linux/miscdevice.h#L92).

With the `misc_register` function as the starting point we will traverse function calls, expressions, structure definitions, and variable initializations to find the user module entrypoint. This entrypoint will be the start of future workshops that will discuss the vulnerabilities that can be found in this intentionally vulnerable Linux driver.

## Exercises

### Exercise 1

Find all the function calls in the program by implementing [Exercise1.ql](exercises/Exercise1.ql).

<details>
<summary>Hints</summary>

- 

</details>

A solution can be found in the query [Exercise1.ql](solutions/Exercise1.ql) 

### Exercise 2

Find all the function calls to the function `misc_register` by implementing [Exercise2.ql](exercises/Exercise2.ql).

<details>
<summary>Hints</summary>

- 

</details>

A solution can be found in the query [Exercise1.ql](solutions/Exercise2.ql)

### Exercise 3

Recall that [predicates](https://codeql.github.com/docs/ql-language-reference/predicates/) and [classes](https://codeql.github.com/docs/ql-language-reference/types/#classes) allow you to encapsulate logical conditions in a reusable format.

Convert your solution to [Exercise2.ql](exercises/Exercise2.ql) into a _class_ in [Exercises3.ql](exercises/Exercise3.ql) by replacing the [none](https://codeql.github.com/docs/ql-language-reference/formulas/#none) formula in the [characteristic predicate](https://codeql.github.com/docs/ql-language-reference/types/#characteristic-predicates) of the `MiscRegisterFunction` class.

Besides relying on the name, try to add another property to distinguish the correct function.

<details>
<summary>Hints</summary>

- 

</details>

A solution can be found in the query [Exercise3.ql](solutions/Exercise3.ql)

### Exercise 4

The definition of the driver is passed as a parameter to the `misc_register` function.
Obtain the argument to the call, determine the arguments type and primary QL class by implementing [Exercise4.ql](exercises/Exercise4.ql).

<details>
<summary>Hints</summary>

- 

</details>

A solution can be found in the query [Exercise4.ql](solutions/Exercise4.ql)

### Exercise 5

The definition of the miscellaneous driver is defined by the structure `miscdevice`.
Implement the characteristic predicate of the class `MiscDeviceStruct` in
[Exercise5.ql](exercises/Exercise5.ql) so we can reason about its use.

<details>
<summary>Hints</summary>

- 

</details>

A solution can be found in the query [Exercise5.ql](solutions/Exercise5.ql)

### Exercise 6

Now that we can reason about the structure `miscdevice` we can look for all it instantiations.
Implement the characteristic predicate of the class `MiscDeviceDefinition` in
[Exercise6.ql](exercises/Exercise6.ql) so we use it to find all its instances.

<details>
<summary>Hints</summary>

- 

</details>

A solution can be found in the query [Exercise6.ql](solutions/Exercise6.ql)

### Exercise 7

The instantiation `vuln_device` defines 3 members of the `miscdevice` structure.
Find the type of the third field initialized with `&vuln_fops` by implementing
[Exercise7.ql](exercises/Exercise7.ql).

<details>
<summary>Hints</summary>

- 

</details>

A solution can be found in the query [Exercise7.ql](solutions/Exercise7.ql)

### Exercise 8

With knowledge of the type of third field om the `miscdevice` structure we can now identify all file operation definitions such as `vuln_fops`.
Implement the characteristic predicates for the class `FileOperationsStruct` and `FileOperationsDefinition` in [Exercise8.ql](exercises/Exercise8.ql).

<details>
<summary>Hints</summary>

- 

</details>

A solution can be found in the query [Exercise8.ql](solutions/Exercise8.ql)

### Exercise 9

The single file operation definition `vuln_fops` is initialized with, among others, a function pointer for the field `unlocked_ioctl`.
This is the functions that get invoked when a user-mode applications performs the `ioctl` system call to communicate with the driver.

Extend the class `FileOperationsDefinition` with a member predicate `getUnlockedIoctl` that returns a `Function` with which the file operations definition is initialized in
[Exercise9.ql](exercises/Exercise9.ql).

<details>
<summary>Hints</summary>

- 

</details>

A solution can be found in the query [Exercise9.ql](solutions/Exercise9.ql)

### Exercise 10

We have successfully identified the miscellaneous driver definition, the file operations definition, and linked the ioctl handler to the file operations definition.
Extend the class `MiscDeviceDefinition` with the member predicate `getFileOperations` that returns a `FileOperationsDefinition` that the miscellaneous driver definition is initialized with in [Exercise10.ql](exercises/Exercise10.ql).

<details>
<summary>Hints</summary>

- 

</details>

A solution can be found in the query [Exercise10.ql](solutions/Exercise10.ql)

### Exercise 11

In this final exercise we have to relate the call to `misc_register` to the miscellaneous driver definition so we can find the driver's entrypoint for user-mode.
Implement the characteristic predicate for the class `MiscDriverUserModeEntry` in [Exercise11.ql](exercises/Exercise11.ql) that relates the classes `MiscRegisterFunction`, `MiscDeviceDefinition`, and `FileOperationsDefinition` to obtain the unlocked ioctl handler function.

<details>
<summary>Hints</summary>

- 

</details>

A solution can be found in the query [Exercise11.ql](solutions/Exercise11.ql)