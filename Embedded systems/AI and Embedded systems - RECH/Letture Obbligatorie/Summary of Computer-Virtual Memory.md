---
tags:
  - RECH
  - readings
data: 2026-10-13
source: "[Comp. Arch. - Lecture 22: Virtual Memory ](https://www.youtube.com/watch?v=oEo_kCDwKtE)"
---
_transcription and summarirzation of youtube video_
# Summary of Computer Architecture Lecture 22: Virtual Memory (Fall 2025)
## 1. Introduction & Fundamentals of Virtual Memory

* **Ideal Memory Abstraction:** An ideal memory system would provide zero access latency, infinite capacity, zero cost, and infinite bandwidth [2, 3].
* **Virtual vs. Physical Memory Abstraction:** In real-world systems, physical DRAM is limited in size [3, 7]. Virtual memory creates the illusion of an infinite, private virtual address space for each process, decoupled from the underlying physical storage [3, 5, 9].
* **Analogy - Warehouse Inventory:** Virtual addresses act like barcodes on items. Workers identify items by barcodes without needing to know their exact physical shelf, aisle, or bin location in the warehouse [4, 14].
* **Key Benefits of Virtual Memory:**
  1. *Automatic Memory Management:* Programmers do not need to manually allocate physical memory locations or manage overlays [5].
  2. *Process Isolation & Protection:* Each process runs in its own address space, preventing unauthorized access or corruption between processes [5, 6].
  3. *Flexible Relocation & Sharing:* Code and data can be placed anywhere in DRAM or shared across processes (e.g., shared libraries) [5, 6, 54].
  4. *Heterogeneous Storage Hierarchy:* Physical DRAM acts as a fully associative cache for pages stored on secondary storage (disks/SSDs or swap) [12, 13].
* **Drawbacks of Direct Physical Addressing:** Systems lacking virtual memory force programmers to manage memory bounds, handle memory relocation manually, coordinate non-overlapping addresses across multi-tenant applications, and rewrite code when physical memory size changes [6, 7, 8].

---

## 2. Address Translation & Page Tables

* **Paging Granularity:** Both virtual and physical address spaces are partitioned into equal-sized chunks called pages (or physical frames) [15].
  * Modern standard sizes: 4 KB (x86-64 Linux/Windows), 16 KB (macOS / Apple Silicon), 8 KB to 32 MB (PowerPC) [15, 16].
* **Address Breakdown:**
  * **Virtual Address:** Split into Virtual Page Number (VPN) and Page Offset [17, 18].
  * **Translation Logic:** Only the VPN is translated into a Physical Page Number (PPN); the Page Offset remains identical [17, 18].
* **Page Table Entry (PTE) Structure:** Contains essential metadata:
  * *PPN:* Physical page frame base address [19, 37].
  * *Valid Bit (V):* Indicates if the virtual page is currently backed by physical DRAM [19, 37, 38].
  * *Protection Bits:* Read, Write, and Execute permissions [37, 55, 57].
  * *Privilege Bits:* User vs. Supervisor/Kernel mode level requirements [55, 58].
  * *Reference Bit (R) & Dirty Bit (D):* Track page access recency and write modifications for replacement and write-back policies [37, 48].
* **Multi-Level Hierarchical Page Tables:**
  * Flat page tables for 64-bit address spaces would require petabytes of storage per process [24, 25].
  * Hierarchical tables (e.g., 4-level page tables in x86-64 using 9-bit indexing fields per level) allocate page table sub-trees dynamically only for valid address ranges, saving massive memory [26, 27, 29].
* **Trade-offs of Page Sizes:**
  * *Large/Huge Pages (e.g., 2 MB, 1 GB):* Reduce the number of page table levels (hops), decrease page table footprint, and boost TLB hit coverage [30, 31, 32].
  * *Disadvantages:* Increase internal fragmentation (wasted memory) and incur higher disk transfer penalties during page faults [32, 33].

---

## 3. Page Faults & Page Replacement

* **Minor Page Faults:**
  * Triggered when a virtual address lacks a valid DRAM mapping, but the data does not need to be loaded from disk [39].
  * Serviced rapidly in software/OS (microseconds) [39].
  * *Causes:* Allocation of uninitialized anonymous memory (e.g., `malloc()`), or Copy-on-Write (CoW) triggers during process cloning/forking [40, 41, 42].
* **Major Page Faults:**
  * Occur when requested data resides on secondary disk storage or swap [42, 43].
  * Serviced via OS I/O routines and DMA controllers, stalling the faulting thread while other processes run [42, 44, 45].
* **Page Replacement Policies:**
  * True LRU is computationally infeasible for main memory pages due to huge metadata tracking overhead [47].
  * Approximate algorithms like *Clock* (and variants like Clock++ or ARC) use circular frame lists and reference bits to determine eviction candidates [48, 49].

---

## 4. Memory Protection & RowHammer Exploits

* **Access Control & Privilege Levels:**
  * Protection bits in PTEs enforce permissions (Read/Write/Execute) and privilege rings (Ring 0 for Kernel, Ring 3 for User space) [55, 57, 58].
  * Accessing unauthorized or unmapped pages triggers hardware exceptions (e.g., segmentation faults) [57].
* **RowHammer Vulnerability & Page Table Exploits:**
  * Repeatedly activating adjacent DRAM row lines ("hammering") induces disturbance errors and bit flips in neighboring victim rows [62, 63].
  * *Exploit mechanism:* Attackers spray physical memory with page tables and hammer nearby addresses until a bit flip occurs in a PTE's PPN field [64, 65, 66].
  * This bit flip can redirect a user process's page table entry to point to another page table, granting full Read/Write control over all physical memory and bypassing OS kernel protection [65, 66, 67].

---

## 5. Accelerating Translation: MMU, TLBs, and Caches

* **Memory Management Unit (MMU):** Hardware block per core dedicated to high-speed address translation [68, 69].
* **Translation Lookaside Buffer (TLB):**
  * Small, fast hardware cache storing recent VPN-to-PPN mappings [69, 70, 71].
  * Multi-level TLB hierarchies (e.g., Intel Skylake L1 Instruction/Data TLBs and L2 Unified TLB) [70, 75].
  * Multi-page size probing: Probes 4 KB, 2 MB, and 1 GB entry structures in parallel or sequentially (pseudo-associatively) [76, 78, 79].
* **Hardware vs. Software Page Table Walkers:**
  * *Hardware Walkers:* Automatically traverse multi-level page tables upon TLB misses without raising OS exceptions, overlapping translation latency with execution [72, 73, 82, 84].
  * *Software Walkers:* Trap to the OS on TLB misses; offers page table layout flexibility at the cost of high exception handling overhead [72, 74, 75].
* **Cache Interaction (VIPT Caching):**
  * Modern systems use Virtually Indexed, Physically Tagged (VIPT) L1 caches [87].
  * Indexes the L1 cache using page offset bits (which are untranslated) in parallel with TLB lookup, avoiding homonym/synonym aliasing while hiding translation latency [85, 86, 87, 88].

---

## 6. Modern Research Trends & Emerging Workloads

* **Virtual Memory Bottlenecks:**
  * *Emerging Workloads (Serverless, LLM Inference):* Short, bursty execution dominated by physical memory allocation and minor fault overheads (~32% of runtime) [90, 91].
  * *Traditional Workloads (Graph Analytics, Databases):* High translation overheads; page table walks consume 20% to 30%+ of total execution cycles [92, 94].
* **Key Research Solutions:**
  1. *Hardware-Assisted Allocation:* Offloading minor page fault handling and page table entry population to hardware queues [98, 99].
  2. *Direct / Contiguous Mappings:* Utilizing contiguous virtual-to-physical ranges to translate addresses via base offset addition rather than tree traversal [97, 98].
  3. *Hybrid Hashing & Page Tables (e.g., Utopia):* Combining restrictive hash-mapped segments for fast constant-time lookup with flexible page tables for general allocation [100, 101].
  4. *Transparent Huge Pages (THP):* OS background threads (`khugepaged`) dynamically collapse contiguous 4 KB pages into 2 MB huge pages [104, 105].
* **Evaluation Frameworks (*Virtuoso*):**
  * Developed by SAFARI research group to solve the speed-vs-accuracy dilemma in VM research [112, 113].
  * Uses *MimicOS* (a lightweight user-space kernel imitating Linux VM management) coupled with architectural simulators (e.g., Sniper) to capture dynamic page fault and translation latencies accurately without full-system slowdown [112, 113, 116, 117].
