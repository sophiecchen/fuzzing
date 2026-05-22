# Exercise 1 - Xpdf

This exercise fuzzes **Xpdf** PDF viewer to find a crash/PoC for [**CVE-2019-13288**](https://www.cvedetails.com/cve/CVE-2019-13288/) in XPDF 3.02. 

<details>
  <summary>CVE-2019-13288 Vulnerability Details</summary>
  --------------------------------------------------------------------------------------------------------
  
  **CVE-2019-13288** is a vulnerability that may cause an infinite recursion via a crafted file.
  
  Since each called function in a program allocates a stack frame on the stack, if a a function is recursively called so many times it can lead to stack memory exhaustion and program crash.
  
  As a result, a remote attacker can leverage this for a DoS attack.
  
  You can find more information about Uncontrolled Recursion vulnerabilities at the following link: https://cwe.mitre.org/data/definitions/674.html
  
</details>

## Process

I ran this exercise in the provided [20.04.2 Ubuntu LTS VMware image](https://drive.google.com/file/d/1_m1x-SHcm7Muov2mlmbbt8nkrMYp0Q3K/view?usp=sharing).

I followed the instructions provided to download necessary tools and build Xpdf. Of the recommended initial PDF examples, the last two (from africau.edu and melbpc.org.au) did not exist: as a result, I used a [sample PDF](https://ontheline.trincoll.edu/images/bookdown/sample-local-pdf.pdf) from Trinity College in additionto the first hello world PDF.

I installed AFL++ via `sudo apt install afl++` instead of the provided Docker or local methods. I then ran AFL++ with the following command:

```
afl-fuzz -i ./pdf_examples/ -o ./out/ -s 123 -- /home/fuzz/fuzzing_xpdf/install/bin/pdftotext @@ /home/fuzz/fuzzing_xpdf/output
```

I then realized, I forgot to set more cores per processor for my VMware machine. Rather than stopping the process (hello, sunk cost fallacy), I decided to step away from my computer since I had some evening plans. I came back when AFL++ had run for almost 3.5 hours, resulting in 1737 paths. For comparison, the original tutorial took about 18 minutes to find 2069 paths, including the path that crashed (though this is also likely due to differences in the PDF examples we had).

![At this point, I had run AFL++ for almost 3.5 hours, resulting in 1737 paths discovered.](./images/image.png)

I quit the program, set the number of cores per processor from 2 to 8, and continued to fuzz (I used `-i-` instead of `-i ./pdf_examples` to continue from where I left off previously).

## Do it yourself!
In order to complete this exercise, you need to:
1) Reproduce the crash with the indicated file 
2) Debug the crash to find the problem
3) Fix the issue