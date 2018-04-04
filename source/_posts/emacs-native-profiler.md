---
title: "Emacs Native Profiler"
comments: true
date: 2018-03-30 10:02:45
udpated: 2018-03-30 10:02:45
categories:
 - Software
 - emacs
tags:
 - emacs
 - profiler
---

If your program is working correctly, but you want to make it run more quickly or efficiently, the first thing to do is profile your code so that you know how it is using resources. If you find that one particular function is responsible for a significant portion of the runtime, you can start looking for ways to optimize that piece.

Emacs has built-in support for this. To begin profiling, type `M-x profiler-start`. You can choose to profile by processor usage, memory usage, or both. After doing some work, type `M-x profiler-report` to display a summary buffer for each resource that you chose to profile. The names of the report buffers include the times at which the reports were generated, so you can generate another report later on without erasing previous results. When you have finished profiling, type `M-x profiler-stop` (there is a small overhead associated with profiling).

The profiler report buffer shows, on each line, a function that was called, followed by how much resource (processor or memory) it used in absolute and percentage times since profiling started. If a given line has a ‘+’ symbol at the left-hand side, you can expand that line by typing <RET>, in order to see the function(s) called by the higher-level function. Use a prefix argument (<C-u RET>) to see the whole call tree below a function. Pressing <RET> again will collapse back to the original state.
<!-- more -->
Press j or mouse-2 to jump to the definition of a function. Press d to view a function's documentation. You can save a profile to a file using C-x C-w. You can compare two profiles using =.
The elp library offers an alternative approach. See the file elp.el for instructions.

You can check the speed of individual Emacs Lisp forms using the benchmark library. See the functions benchmark-run and benchmark-run-compiled in benchmark.el.
To profile Emacs at the level of its C code, you can build it using the --enable-profiling option of configure. When Emacs exits, it generates a file gmon.out that you can examine using the gprof utility. This feature is mainly useful for debugging Emacs. It actually stops the Lisp-level M-x profiler-... commands described above from working.
