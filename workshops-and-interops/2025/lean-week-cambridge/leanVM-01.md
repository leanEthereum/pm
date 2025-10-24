# leanVM-01: leanVM Crash Course & Sumcheck Optimization

## Categories
- leanVM
- PQ-implementation

## Related Talks
- [leanVM-02](leanVM-02.md): Memory Models & Lookup Arguments
- [leanVM-03](leanVM-03.md): Compiler (workshop)
- [pq1-05](pq1-05.md): Minimal zkVM for Lean Ethereum
- [pq1-06](pq1-06.md): XMSS Signature Aggregation
- [pq2-04](pq2-04.md): Minimal zkVM
- [pq2-09](pq2-09): Generic VM Compilation Architectures
- [pq3-03](pq3-03.md): Minimal zkVM - Cairo M Design 

## Summary
Thomas Coratger and Emile presented a crash course on the minimal zkVM for Lean Ethereum, focusing on its design for PQ signature aggregation and merging using XMSS and Poseidon2. The zkVM aims to prove hash-intensive operations efficiently, with a simple ISA (add, mul, dereference, jump, Poseidon variants) and write-once memory model using assertions. Discussions covered frame-relative addressing, function calls via jumps with hints, recursion for loops, compilation passes to compress memory (removing unused cells), and Poseidon24/16/8 for optimized hashing (aligned by 8 for lookups). Participants explored trade-offs like loop handling (recursion vs. allocation), malloc alternatives, and performance (e.g., vectorized memory reducing footprint by ~10x). The session emphasized formal verification and industry adoption for broader scrutiny.

## Key Takeaways
- zkVM unifies aggregation/merging in one program, proving hashes/recursive SNARKs over KoalaBear field for efficiency.
- ISA: Add/mul for relations; dereference for pointers/cross-frame access; jump for calls/returns/control (with compile-time footprints); Poseidon variants (24/16/8) for compression (e.g., 64 chains to Merkle leaf 2x faster).
- Memory: Write-once with assertions; frames for functions (hints allocate new); two-pass compilation compresses (95% usage, remove zeros/gaps).
- Loops: Transform to recursion (no AP register); avoid variable iterations to fix footprints; TCO for tail calls avoids returns.
- Optimizations: Aligned accesses for lookups; precomputing for hashes; potential malloc via dereference (costly without frames).
- Challenges: No dynamic allocation in frames; if/else overhead (unused memory pruned); industry-wide use for security/performance.

## Speakers
- [Thomas Coratger](https://x.com/tcoratger) (EF - zkEVM)
- Emile (EF grantee)

## Resources
- [Slides](https://drive.google.com/file/d/1MM1CvrfCCJ9fcGQsEQNMmnZ3qromYyL2/view?usp=drive_link)
- [minimal_zkVM](https://drive.google.com/file/d/1F0r4oQBpKFDN1Vk5RlOS3R8BRYQGXiAR/view?usp=drive_link)
- [YouTube Video](https://youtu.be/unuRZoEfhF0)

> back to: [Index Page](index.md)

*Note: Summaries were generated in part with the help of AI, so names and terms may not be 100% accurate.*