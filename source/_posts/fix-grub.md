---
title: Fix a grub bug
date: 2021-03-20
tag: 
- BUG
category: BUG
mathjax: false
---
In this article, I will record how to fix the problem that grub can't find Windows on nvme. So that someday, I can reuse this to help myself.
<!-- more -->

First of all, install `os-prober`.

Then, run `update-grub` or `grub-mkconfig`.
Then you will find that os-prober is not working. 

It will print a warning saying like: `OS-PROBER will not be executed in order to protect xxx. If you want to know more, please look GRUB-DISABLE-OR-PROBER document to find more information`. 

Well, only stupid people will do as what the warning says.

For me, I just `vim /etc/default/grub` and add one line `GRUB-DISABLE-OR-PROBER = false` in this file.

Then run `grub-mkconfig` again. This silly problem will get fixed.
