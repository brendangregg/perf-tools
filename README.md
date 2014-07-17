perf-tools
==========

A miscellaneous collection of in-development and unsupported performance analysis tools for Linux perf_events, aka the "perf" command, and ftrace. Both perf_events and ftrace are core Linux tracing tools, and are included in the Linux kernel source.

These tools are designed to be simple to use, and provide advanced performance observability. This collection was written by Brendan Gregg (author of the DTraceToolkit).

## Contents

Using perf_events:

- misc/[perf-stat-hist](misc/perf-stat-hist): power-of aggregations for tracepoint variables.

Using ftrace:

- [iosnoop](iosnoop): trace disk I/O with details including latency.
- kernel/[funccount](kernel/funccount): count kernel functions that match a string.
- kernel/[functrace](kernel/functrace): trace kernel functions that match a string.

## Prerequisites

### perf_events

Requires the "perf" command to be installed. This is in the linux-tools-common package, or can be built under tools/perf in the kernel source. See [perf_events Prerequisites](http://www.brendangregg.com/perf.html#Prerequisites) for more details.
### ftrace

FTRACE configured in the kernel.

## Install

These are just scripts. Either grab everything:

```
git clone https://github.com/brendangregg/perf-tools
```

Or use the raw links on github to download individual scripts. Eg

```
wget https://raw.githubusercontent.com/brendangregg/perf-tools/master/iosnoop
```

This preserves tabs (which copy-n-paste can mess up).

## Internals, Warnings, and Overhead

perf_events is evolving. This collection began developent on Linux 3.16, where perf_events lacks certain programmatic capabilities (eg, custom in-kernel aggregations). It's possible these will be added in a forthcoming kernel release. Until then, many of these tools employ workarounds, tricks, and hacks in order to work. This includes passing event data to user space for post-processing, which costs much higher overhead than in-kernel aggregations.

__WARNING__: In extreme cases, your target application may run 5x slower when using these tools. Depending on the tool and kernel version, there may also be the risk of kernel panics. Read the program header for warnings, and test before use.

If the overhead is a problem, these tools can be improved. If a tool doesn't already, it could be rewritten in C to use perf_events_open() and mmap() for the trace buffer. It could also implement frequency counts in C, and operate on mmap() directly, rather than using awk/Perl/Python. Additional improvements are possible for ftrace-based tools, such as use of snapshots and per-instance buffers.

These tools are intended as short-term workarounds until more kernel capabilities exist, at which point they can be substantially rewritten. Older versions of these tools will be kept in this repository, for older kernel versions.

Since things are changing, it's very possible you may find some tools don't work on your Linux kernel version. Some expertise and assembly will be required to fix them.
