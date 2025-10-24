# leanVM-02: Memory Models & Lookup Arguments

## Categories
- leanVM
- PQ-implementation

## Related Talks
- [leanVM-01](leanVM-01.md): leanVM Crash Course & Sumcheck Optimization
- [leanVM-03](leanVM-03.md): Compiler (workshop)
- [pq1-05](pq1-05.md): Minimal zkVM for Lean Ethereum
- [pq1-06](pq1-06.md): XMSS Signature Aggregation
- [pq2-04](pq2-04.md): Minimal zkVM
- [pq2-09](pq2-09): Generic VM Compilation Architectures
- [pq3-03](pq3-03.md): Minimal zkVM - Cairo M Design 

## Summary
Thomas Coratger and Emile discussed memory models and lookup arguments for the minimal zkVM, emphasizing write-once cells with assertions for relations (e.g., differences as additions). Frame-relative addressing enables efficient access, while dereference instructions handle cross-frame data (arguments/results). Function calls use jumps with hints for new frames (compile-time footprints), supporting recursion for loops (no AP register). Compilation involves two passes to compress memory (remove unused cells, ~10x reduction via vectorization/alignment by 8). Poseidon variants (24/16/8) optimize hashing (e.g., 64 chains 2x faster), with aligned accesses for lookups in extension fields. Sumcheck optimizations (univariate skip, Bagad) were explored for PIOP/PCS, focusing on base vs. extension field operations (SS/SF/SL/LL costs). Packing techniques for extension fields (SIMD, transposes) reduce multiplications (~8x speedup), with discussions on integration (Plonky3 APIs, Fiat-Shamir serialization). Hardware targets: Raspberry Pi/CPU for validators, GPUs for subnets.

## Key Takeaways
- Memory: Write-once with assertions; frames for functions (hints allocate); vectorized/aligned for lookups (95% usage post-compression).
- ISA Extensions: Dereference for pointers; jumps for calls/returns/control (store PC/FP for returns); recursion/TCO for loops (avoid dynamic allocation overhead).
- Poseidon: 24/16/8 for compression (concatenate inputs); aligned by 8 reduces lookups; potential specialized opcodes for XMSS/Winternitz.
- Sumcheck: Univariate skip precomputes coefficients (base field); Bagad decomposes for fewer LL ops; packing (SIMD, SoA) for extension fields (8x mul speedup, transposes costly).
- Challenges: No dynamic loops (fixed footprints); if/else gaps pruned; hardware (CPU validators, GPU subnets); proof structs vs. vectors for Fiat-Shamir (structs reduce errors).
- Optimizations: Two-pass compilation; potential malloc via dereference (costly); align with Plonky3 for generic sumcheck.

## Speakers
- [Thomas Coratger](https://x.com/tcoratger) (EF - zkEVM)
- Emile (EF grantee)

## Resources
- [Slides](https://drive.google.com/file/d/1MM1CvrfCCJ9fcGQsEQNMmnZ3qromYyL2/view?usp=drive_link)
- [minimal_zkVM](https://drive.google.com/file/d/1F0r4oQBpKFDN1Vk5RlOS3R8BRYQGXiAR/view?usp=drive_link)
- [YouTube Video](https://youtu.be/3EAPQGIlc6k)

> back to: [Index Page](index.md)

*Note: Summaries were generated in part with the help of AI, so names and terms may not be 100% accurate.*