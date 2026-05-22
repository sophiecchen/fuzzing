# Exercise 2 - libexif

This time we will fuzz **libexif** EXIF parsing library. The goal is to find a crash/PoC for [**CVE-2009-3895**](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2009-3895) and another crash for [**CVE-2012-2836**](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-2836) in libexif 0.6.14.

<details>
  <summary>For more information about CVE-2009-3895 and CVE-2012-2836 vulnerabilities, click me!</summary>
  --------------------------------------------------------------------------------------------------------
  
**CVE-2009-3895** is a heap-based buffer overflow that can be triggered with an invalid EXIF image.
  
  A heap-based buffer overflow is a type of buffer overflow that occurs in the heap data area, and it's usually related to explicit dynamic memory management (allocation/deallocation with malloc() and free() functions).
  
  As a result, a remote attacker can exploit this issue to execute arbitrary code within the context of an application using the affected library.
  
  You can find more information about Heap-based buffer oveflow vulnerabilities at the following link: https://cwe.mitre.org/data/definitions/122.html
  
  -------------------------------------------------------------------------
  
  **CVE-2012-2836** is an Out-of-bounds Read vulneratibily that can be triggered via an image with crafted EXIF tags.
  
  An Out-of-bounds Read is a vulnerability that occurs when the program reads data past the end, or before the beginning, of the intended buffer.
  
  As a result, it allows remote attackers to cause a denial of service or possibly obtain potentially sensitive information from process memory.
  
  You can find more information about Out-of-bounds Read vulnerabilities at the following link: https://cwe.mitre.org/data/definitions/125.html
  
</details>
 

## What you will learn
Once you complete this exercise you will know how:
- To fuzz a library using an external application
- To use **afl-clang-lto**, a collision free instrumentation that is faster and provides better results than *afl-clang-fast*
- To use Eclipse IDE as an easy alternative to GDB console for triaging

## Read Before Start
- I suggest you to try to **solve the exercise by yourself** without checking the solution. Try as hard as you can, and only if you get stuck, check out the example solution below.
- AFL uses a non-deterministic testing algorithm, so two fuzzing sessions are never the same. That's why I highly recommend **to set a fixed seed (-s 123)**. This way your fuzzing results will be similar to those shown here and that will allow you to follow the exercises more easily.  
- If you find a new vulnerability, **please submit a security report** to the project. If you need help or have any doubt about the process, the [GitHub Security Lab](mailto:securitylab.github.com) can help you with it :)

## Contact
Are you stuck and looking for help? Do you have suggestions for making this course better or just positive feedback so that we create more fuzzing content?
Do you want to share your fuzzing experience with the community?
Join the GitHub Security Lab Slack and head to the `#fuzzing` channel. [Request an invite to the GitHub Security Lab Slack](mailto:securitylab-social@github.com?subject=Request%20an%20invite%20to%20the%20GitHub%20Security%20Lab%20Slack)

## Environment

All the exercises have been tested on **Ubuntu 20.04.2 LTS**. I highly recommend you to use **the same OS version** to avoid different fuzzing results and to run AFL++ **on bare-metal** hardware, and not virtualized machines, for best performance.

Otherwise, you can find an Ubuntu 20.04.2 LTS VMware image [here](https://drive.google.com/file/d/1_m1x-SHcm7Muov2mlmbbt8nkrMYp0Q3K/view?usp=sharing). You can also use VirtualBox instead of VMware.

The username / password for this VM are `fuzz` / `fuzz`.

## Do it yourself!
In order to complete this exercise, you need to:
1) Find an interface application that makes use of the libexif library
2) Create a seed corpus of exif samples
3) Compile libexif and the chosen application to be fuzzed using afl-clang-lto
4) Fuzz libexif until you have a few unique crashes
5) Triage the crashes to find a PoC for each vulnerability
6) Fix the issues

**Estimated time = 6 hours**