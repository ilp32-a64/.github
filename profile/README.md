# ILP32-A64

**Bringing AArch64 ILP32 back into focus.**

## Why ILP32?
The AI era is driving memory demand — and memory prices — sharply upward.
On memory-constrained embedded systems, every megabyte counts.

AArch64 ILP32 runs 64-bit ARM code with 32-bit `int`, `long`, and pointers:
- Keeps the full AArch64 instruction set and register file
- Smaller pointers and data structures mean better cache utilization
- Our benchmarks show **20–30% lower memory usage** with no performance loss

## The Challenge
ILP32 needs a complete ecosystem, but upstream support remains limited:
- **Kernel** — ILP32 ABI support in the Linux kernel
- **Toolchains** — GCC, LLVM/Clang, binutils
- **Libraries & runtimes** — glibc, musl, and language runtimes such as Java, Node.js, and Python

## Our Goals
- Publish open benchmarks and methodology quantifying ILP32's memory savings
- Maintain and upstream ILP32 support across the kernel and toolchains
- Provide reproducible build environments and examples

## Get Involved
Issues, pull requests, and benchmark results from your own embedded platforms are all welcome.
