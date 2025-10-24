# leanVM-03: Compiler (workshop)

## Categories
- leanVM
- PQ-implementation

## Related Talks
- [leanVM-01](leanVM-01.md): leanVM Crash Course & Sumcheck Optimization
- [leanVM-02](leanVM-02.md): Memory Models & Lookup Arguments
- [pq1-05](pq1-05.md): Minimal zkVM for Lean Ethereum
- [pq1-06](pq1-06.md): XMSS Signature Aggregation
- [pq2-04](pq2-04.md): Minimal zkVM
- [pq2-09](pq2-09): Generic VM Compilation Architectures
- [pq3-03](pq3-03.md): Minimal zkVM - Cairo M Design 

## Summary
Thomas Coratger and Emile led a workshop on the compiler for the minimal zkVM, focusing on transforming high-level code (e.g., loops to recursion) into efficient ISA while handling memory footprints, dynamic allocation, and verification. Discussions explored avoiding DSL/compilers via friendly APIs (e.g., circuit-builder like Jolt, RISC Zero, SP1), outsourcing strategies to reduce VM ops, and optimizations (e.g., two-pass compression, precomputing for hashes). Participants debated read-only vs. read-write memory, continuity for lookups (not required but performance-boosting), and index lookups vs. permutation arguments (LogUp* reduces commitments for small tables). Trade-offs included recursion overhead (TCO mitigates returns), malloc via dereference (costly), and hardware targets (Raspberry Pi/CPU for validators). The session emphasized simplifying for formal verification and benchmarking against existing zkVMs (e.g., OpenVM, Jolt) to ensure competitiveness.

## Key Takeaways
- Compiler transforms loops to recursion (fixed footprints, TCO avoids returns); if/else gaps pruned via passes (~95% usage).
- API vs. DSL: Circuit-builder APIs (e.g., Jolt, RISC Zero) simplify without compilers; outsourcing reduces VM load.
- Memory/Lookups: Read-only with assertions; index lookups (LogUp/LogUp*) vs. permutations (reduce commitments ~3x for bytecode/memory); continuity optional but aids performance.
- Optimizations: Two-pass compilation compresses (~10x); precompute hashes; align with Plonky3 for generic sumcheck.
- Challenges: No dynamic loops (upper-bound allocation costly); hardware (CPU focus, GPUs for subnets); verify compiler security.
- Benchmarks: Compare vs. existing zkVMs; explore folding (Nova, Neo) for aggregation.

## Speakers
- [Thomas Coratger](https://x.com/tcoratger) (EF - zkEVM)
- Emile (EF grantee)

## Resources
- [Slides](https://drive.google.com/file/d/1MM1CvrfCCJ9fcGQsEQNMmnZ3qromYyL2/view?usp=drive_link)
- [minimal_zkVM](https://drive.google.com/file/d/1F0r4oQBpKFDN1Vk5RlOS3R8BRYQGXiAR/view?usp=drive_link)
- [YouTube Video](https://youtu.be/GzwR_E5DCkk)

> back to: [Index Page](index.md)

*Note: Summaries were generated in part with the help of AI, so names and terms may not be 100% accurate.*