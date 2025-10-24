# Lean Day (EthCC[8]) Event Overview

**Event**: Lean Day - EthCC[8] 2025  
**Date**: June 29, 2025  
**Location**: JW Marriott, Cannes, France  
**Host**: Ethereum Foundation  

Lean Day was a comprehensive technical gathering focused on advancing a set up upgrades to the Ethereum consensus layer, known as lean consensus (NOTE: at the the time of this recording, the upgrades were known as beam chain).  The goal of this event was to bring together researchers, developers, and client teams to discuss the latest progress and future directions for post-quantum cryptography, zkVM integration, and consensus improvements.


## Event Schedule and Presentations

| Time | Session | Speaker(s) | Topic | Links |
|------|---------|------------|-------|-------|
| 09:30 | **Opening Keynote** | Justin Drake (EF) | Strategic Overview of lean consensus (fka beam chain) | [Slides, Video & Transcript](/workshops-and-interops/2025/lean-day-EthCC/01-opening-keynote.md) |
| | | | | |
| **10:00-10:45** | **Post-Quantum Cryptography** | | | |
| 10:00 | Post-Quantum Signatures | Dmitri Khovratovich (EF) | Post-quantum signatures for lean consensus (fka beam chain) | [Slides, Video & Transcript](/workshops-and-interops/2025/lean-day-EthCC/02-pq-signatures.md) |
| 10:15 | Recursive Aggregation | Emile Hautefeuille (EF) | Post-quantum recursive signature aggregation | [Slides, Video & Transcript](/workshops-and-interops/2025/lean-day-EthCC/03-recursive-aggregation.md) |
| 10:30 | P2P Aggregation | Kamil Salakhiev (Quadrivium) | P2P aspects of sequential aggregation | [Slides, Video & Transcript](/workshops-and-interops/2025/lean-day-EthCC/04-p2p-aggregation.md) |
| | | | | |
| **10:45-11:15** | **SNARKification** | | | |
| 10:45 | Beacon SNARKification | Jacob Everly (Boundless) | ZKasper and Ethereum light clients | [Slides, Video & Transcript](/workshops-and-interops/2025/lean-day-EthCC/05-beacon-snarkification.md) |
| 11:00 | Zig SNARKification | Guillaume Ballet (Zeam & Geth) | Zig client and zkVM integration | [Slides, Video & Transcript](/workshops-and-interops/2025/lean-day-EthCC/06-zig-snarkification.md) |
| | | | | |
| **11:30-12:00** | **Funding and Sustainability** | | | |
| 11:30 | Client Sustainability | Tomasz Stanczak (EF) | Building financially sustainable Ethereum clients | [Slides, Video & Transcript](/workshops-and-interops/2025/lean-day-EthCC/07-client-sustainability.md) |
| 11:45 | Funding Panel | Barnabé (EF), Tomasz (EF), Cheeky Gorilla (PG), Mario (EPF), Pat (Pier Two) | Funding strategies and support mechanisms | [Slides, Video & Transcript](/workshops-and-interops/2025/lean-day-EthCC/08-funding-panel.md) |
| | | | | |
| **12:00-12:45** | **Development Updates** | | | |
| 12:00 | Zeam Client | Gajinder Singh (Zeam) | Zig client development and multi-prover architecture | [Slides, Video & Transcript](/workshops-and-interops/2025/lean-day-EthCC/09-zeam-client.md) |
| 12:15 | Ream Client | Unnawut Leepaisalsuwanna (Ream Labs) | Rust beacon client and zkVM experiments | [Slides, Video & Transcript](/workshops-and-interops/2025/lean-day-EthCC/10-ream-client.md) |
| 12:30 | Lantern Client | Mihir Faujdar (Pier Two) | C libraries and infrastructure development | [Slides, Video & Transcript](/workshops-and-interops/2025/lean-day-EthCC/11-lantern-client.md) |
| | | | | |
| **14:30-16:15** | **Roundtable #1: Post-Quantum Testnet** | | | |
| 14:30 | Signature Aggregation | Thomas Coratger (EF) | Post-quantum testnet progress and challenges | [Slides, Video & Transcript](/workshops-and-interops/2025/lean-day-EthCC/12-signature-aggregation.md) |
| 14:45 | PQ Testnet Discussion | Moderated by Thomas Coratger (EF) & Yanis Psaras (Probelab) | Implementation planning and technical concerns | [Slides, Video & Transcript](/workshops-and-interops/2025/lean-day-EthCC/13-pq-testnet-discussion.md) |
| | | | | |
| **16:30-18:00** | **Roundtable #2: Consensus Improvements** | | | |
| 16:30 | Rainbow Staking | Barnabé Monnot (EF) | Unbundling validator services framework | [Slides, Video & Transcript](/workshops-and-interops/2025/lean-day-EthCC/14-rainbow-staking.md) |
| 16:45 | Low-Latency P2P | Csaba Kiraly (EF Geth) | Networking optimizations and latency reduction | [Slides, Video & Transcript](/workshops-and-interops/2025/lean-day-EthCC/15-low-latency-p2p.md) |
| 17:00 | Shorter Slots Discussion | Moderated by Barnabé Monnot (EF) & Sean Anderson (SigmaPrime) | Slot time reduction strategies and trade-offs | [Slides, Video & Transcript](/workshops-and-interops/2025/lean-day-EthCC/16-shorter-slots-disucssion.md) |

## Key Themes and Outcomes

### Post-Quantum Readiness
- **XMSS signatures** identified as leading candidate to replace BLS
- **Whir polynomial commitment scheme** achieving ~100KB proofs with microsecond verification
- **Poseidon 2** hash function selected as optimal for STARK proving
- Q3 2025 timeline for cryptography implementation, testnet in Q4

### Client Development Progress
- **Three active lean consensus clients**: Zeam (Zig-based), Ream (Rust-based), Lantern (C-based)
- Multi-zkVM support across RISC Zero, SP1, Jolt, and others
- Real-world performance: 100k-1M hashes/sec achieved on consumer hardware

### Consensus and Networking Innovation
- **Rainbow staking framework** for unbundling validator services
- **6-8 second slot times** appear technically feasible
- Advanced P2P toolbox with 6 optimization techniques discussed
- Single Slot Finality (SSF) integration across multiple client implementations being prototyped

### Funding and Sustainability
- Protocol Guild expanding eligibility framework for lean consensus client teams
- Ethereum Foundation balancing short-term priorities with long-term innovation
- Multiple funding models: grants, consulting, corporate acquisition, venture capital

### Community and Collaboration
- 50-60 attendees from dozen+ teams
- Active zkVM ecosystem with weekly performance improvements
- Growing contributor base across all client implementations
- Strong emphasis on interoperability and shared infrastructure

---

*Note: This overview provides links to detailed summaries of each presentation, including full transcripts, slide references, and key technical insights from the Beam Day event. Transcripts were generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*