perf-tools
==========

A miscellaneous collection of in-development and unsupported performance analysis tools for Linux perf_events, aka the "perf" command, and ftrace. Written by Brendan Gregg (author of the DTraceToolkit).

These tools are designed to be simple to use, and provide advanced performance observability. They are written for Linux systems, for times when perf_events or ftrace are the best tracing options available.

## Contents

- misc/__perf-stat-hist__: power-of aggregations for tracepoint variables

## Tool Internals and Overhead

perf_events is evolving. This collection began developent on Linux 3.16, where perf_events lacks certain programmatic capabilities (eg, custom in-kernel aggregations). It's possible these will be added in a forthcoming kernel release. Until then, many of these tools employ workarounds, tricks, and hacks in order to work. This includes passing event data to user space for post-processing, which incurs much higher overhead than in-kernel aggregations.

__WARNING__: In extreme cases, your target application may run 5x slower when using these tools. Read the program header for warnings, and test before use.

If the overhead is a problem, these tools can be improved. If a tool doesn't already, it could be rewritten in C to use perf_events_open() and mmap() for the trace buffer. It could also implement frequency counts in C, and operate on mmap() directly, rather than using awk/Perl/Python.

Over time, it is expected that more of these tools can be substantially rewritten to use more efficient perf_events and ftrace features. Any tool you find in this collection which involves user space processing in awk/Perl/Python should be considered a temporary workaround for older kernel versions, and will be kept in this collection for older kernels alongside the newer tools.

Since things are changing, it's very possible you may find some tools don't work on your Linux kernel version. Some expertise and assembly will be required to fix them.
