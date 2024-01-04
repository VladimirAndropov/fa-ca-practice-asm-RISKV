Lecture 8
---

# Memory and caches

## Lecture

Slides ([PDF](CA_Lecture_08.pdf), [PPTX](CA_Lecture_08.pptx)).

#### Outline

* Processor-memory performance gap
* Types of memory devices
* Principle of locality and memory hierarchy
* Cache memory (direct-mapped, set-associative, fully-associative)
* Writing and replacement policies
* Multi-level caches
* Performance considerations

## Workshop

#### Outline

* Cache types
* RARS Data Cache Simulator
* Playing with cache configurations

#### Examples

Linear memory accesses:

```assembly
    .eqv  START 0x10010000
    .eqv  SZ    512
    .text
    li    s0, START
    addi  s1, s0, SZ
loop:
    lw    t0, 0(s0)
    addi  s0, s0, 4
    blt   s0, s1, loop
```

Gaped memory accesses:

```assembly
    .eqv  START 0x10010000
    .eqv  HSZ   256
    # Direct mapping burns out
    # Associative captures
    .text
    li    s0, START
    addi  s1, s0, HSZ
    mv    s2, s1
loop:
    lw    t0, 0(s0)
    lw    t1, 0(s1)
    addi  s0, s0, 4
    addi  s1, s1, 4
    blt   s0, s2, loop
```

Equidistance (try to vary step):

```assembly
    .eqv  START 0x10010000
    .eqv  SZ    256
    .eqv  GAP   3   # Try 5, 11
    .text

    li    s0, START  # start address
    addi  s1, s0, SZ # end address
    li    s2, GAP    # gap in words
    slli  s3, s2, 2  # gap in bytes

    mv    t0, zero
loop_gap:
    slli  t1, t0, 2
    add   t1, s0, t1
loop:
    lw    t2, 0(t1)
 
    add   t1, t1, s3
    blt   t1, s1, loop

    addi  t0, t0, 1
    blt   t0, s2, loop_gap
```

#### Tasks

1. Assume the miss rate of an instruction cache is 2% and the miss rate of the data cache is 4%.
   If a processor has a CPI of 2 without any memory stalls, and the miss penalty is 100 cycles for all misses,
   determine how much faster a processor would run with a perfect cache that never missed.
   Assume the frequency of all loads and stores is 36%.

1. Find the AMAT for a processor with a 1 ns clock cycle time, a miss penalty of 20 clock cycles,
   a miss rate of 0.05 misses per instruction, and a cache access time (including hit detection) of 1 clock cycle.
   Assume that the read and write miss penalties are the same and ignore other write stalls.

1. Use the system with access times of 1, 10, and 100 cycles for the L1 cache, L2 cache, and main memory, respectively. 
   Assume that the L1 and L2 caches have miss rates of 5% and 20%, respectively. 
   Specifically, of the 5% of accesses that miss the L1 cache, 20% of those also miss the L2 cache. 
   What is the average memory access time (AMAT)?

1. Assuming a cache of 4096 blocks, a four-word block size, and a 64-bit address,
   find the total number of sets and the total number of tag bits for caches that are
   direct-mapped two-way and four-way set associative, and fully associative.

1. Try the above examples with following cache configurations (`Tool | Data Cache Simulator`).

   * Placement policy: Direct Mapping / Fully Associative / 2-Way associative
   * Block replacement policy: LRU / Random

   `2×3=6` experiments in total. Report the cache hit rate.

1. Write a program that:
   
   * burns out default fully associative cache with 100% misses;
   * does this in cycle (if previously not);
   * fills only 256 bytes of memory without a gap.

1. Write a program that utilizes memory sparsely, so that its footprint is 100% misses 2-way associative cache.
   However, it fits (almost) into a 4-way associative cache with 16 blocks.

## Homework

Finish all the tasks.

## References

* Large and Fast: Exploiting Memory Hierarchy. Chapter 5 in [[CODR]](../../books.md). 
* Ulrich Drepper. [What Every Programmer Should Know About Memory](
  https://github.com/andrewt0301/hse-acos-course/blob/master/related/cpumemory.pdf).
* [CPU cache](https://en.wikipedia.org/wiki/CPU_cache) (Wikipedia).
* [Memory hierarchy](https://en.wikipedia.org/wiki/Memory_hierarchy) (Wikipedia).
* [Cache hierarchy](https://en.wikipedia.org/wiki/Cache_hierarchy) (Wikipedia).
* [Cache oblivious_algorithm](https://en.wikipedia.org/wiki/Cache-oblivious_algorithm) (Wikipedia).
* [Matrix multiplication algorithm](https://en.wikipedia.org/wiki/Matrix_multiplication_algorithm) (Wikipedia).
