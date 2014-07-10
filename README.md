perf-tools
==========

A miscellaneous collection of in-development and unsupported performance analysis tools for Linux perf_events, aka the "perf" command, written by Brendan Gregg (author of the DTraceToolkit).

These tools are designed to be simple to use, and provide advanced performance observability. They are written for Linux systems, for times when perf_events is the best tracing option available.

## Contents

- misc/__perf-stat-hist__: power-of aggregations for tracepoint variables

## Tool Internals

perf_events is evolving. This collection began developent on Linux 3.16, where perf_events lacks certain programmatic capabilities (eg, custom in-kernel aggregations). It's possible these will be added in a forthcoming kernel release. Until then, these tools employ workarounds, tricks, and hacks in order to work. Shell, awk, Perl, and Python are used if needed, in that order of preference.

Over time, as perf_events is enhanced, these tools should employ fewer hacks, and will likely be much simpler and cause less overhead. Older versions will be kept in this collection for older kernels.

Since things are changing, it's very possible you may find some tools don't work on your Linux kernel version. Some expertise and assembly will be required to fix them.

## Tool Overhead

Many of these tools pass event data to user space for post-processing, which incurs much higher overhead than in-kernel aggergation tools. In extreme cases, your target application may run 5x slower while using a perf_events tool. Test before use.

If you find a tool where the overheads are causing a problem, and if it isn't already, consider rewriting it in C to call perf_events_open() and mmap() the trace buffer, so post-processing can operate more efficiently. Also consider helping/encouraging perf_events to support better in-kernel aggregations to avoid these overheads, and/or the adoption of similar advanced tools (SystemTap, ktap, ...).
