# Exercise 3 - TCPdump

In this exercise we will fuzz **TCPdump** packet analyzer. The goal is to find a crash/PoC for [**CVE-2017-13028**](https://www.cvedetails.com/cve/CVE-2017-13028/) in TCPdump 4.9.2.

<details>
  <summary>For more information about CVE-2017-13028 vulnerability, click me!</summary>
  --------------------------------------------------------------------------------------------------------
  
**CVE-2017-13028** is an Out-of-bounds Read vulneratibily that can be triggered via a BOOTP packet (Bootstrap Protocol).
  
  An Out-of-bounds Read is a vulnerability that occurs when the program reads data past the end, or before the beginning, of the intended buffer.
  
  As a result, it allows remote attackers to cause a denial of service or possibly obtain potentially sensitive information from process memory.
  
  You can find more information about Out-of-bounds Read vulnerabilities at the following link: https://cwe.mitre.org/data/definitions/125.html
  
</details>

## What you will learn
Once you complete this exercise you will know:
- What is **ASan (Address Sanitizer)**, a runtime memory error detection tool
- How to use ASAN to fuzz a target
- How much easy is to triage the crashes using ASan

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
1) Find an efficient way to fuzz TCPdump
2) Try to figure out how to enable ASan for fuzzing
3) Fuzz TCPdump until you have a few unique crashes
4) Triage the crashes to find a PoC for the vulnerability
5) Fix the issue