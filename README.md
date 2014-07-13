perf-tools
==========

A miscellaneous collection of in-development and unsupported performance analysis tools for Linux perf_events, aka the "perf" command, and ftrace. Written by Brendan Gregg (author of the DTraceToolkit).

These tools are designed to be simple to use, and provide advanced performance observability. They are written for Linux systems, for times when perf_events or ftrace are the best tracing options available.

## Contents

- misc/__perf-stat-hist__: power-of aggregations for tracepoint variables

## Prerequisites

Some tools use perf_events, some ftrace. Both of these are in Linux kernel source. See the top of the tool script to see which is used.

- perf_events: requires the "perf" command to be installed. This is in the linux-tools-common package, or can be built under tools/perf in the kernel source. See [perf_events Prerequisites](http://www.brendangregg.com/perf.html#Prerequisites) for more details.
- ftrace: FTRACE configured in the kernel.

## About These Tools

perf_events is evolving. This collection began developent on Linux 3.16, where perf_events lacks certain programmatic capabilities (eg, custom in-kernel aggregations). It's possible these will be added in a forthcoming kernel release. Until then, many of these tools employ workarounds, tricks, and hacks in order to work. This includes passing event data to user space for post-processing, which costs much higher overhead than in-kernel aggregations.

__WARNING__: In extreme cases, your target application may run 5x slower when using these tools. Depending on the tool and kernel version, there may also be the risk of kernel panics. Read the program header for warnings, and test before use.

If the overhead is a problem, these tools can be improved. If a tool doesn't already, it could be rewritten in C to use perf_events_open() and mmap() for the trace buffer. It could also implement frequency counts in C, and operate on mmap() directly, rather than using awk/Perl/Python. Although, these features should one day be available in-kernel from perf_events, so any user-level version is really a temporary workaround.

Currently, this tool collection is more "perf hacks" than "perf tools". That will change as Linux adds more in-kernel tracing features, and these tools can be entirerly rewritten. Older versions of these tools will be kept in this repository, for older kernel versions.

Since things are changing, it's very possible you may find some tools don't work on your Linux kernel version. Some expertise and assembly will be required to fix them.
