# Lean Day Event Transcript - Lantern Team C Libraries Update

| Item | Details |
| ---- | ------- |
| **Session** |  Development Updates - Lantern Team Progress on C Libraries (recorded presentation) |
| **Speaker(s)** | Mihir Faujdar (MF), Pier Two - Latern Team Lead |
| **Slides** | [link](https://docs.google.com/presentation/d/1ciovpByQi_bG5-9r4E4L96x62-IXxRN0a8JLiZsPdyA/edit?usp=sharing) |
| **YouTube** | [link](https://youtu.be/opIYLuXMtMc?feature=shared) |

>back to: [Lean Day Overview Page](/workshops-and-interops/2025/lean-day-EthCC/00-index.md)

---

## Lantern Team C Libraries Development Update

### Introduction and Overview

**MF**: Hi everyone, nice to meet you. My name is **MF** and I'm from the **Peer Two team**. In this presentation, I would like to provide an overview of the progress we've made so far with the development of **C libraries** that we will be using for creating the Beam client named **Lantern**. I would also like to provide more information on what projects we plan to focus on next.

---

## Completed Project 1: SSZ Library in C

**MF**: So in early Q1 of this year, we finished developing **Simple Serialize in C** as it was a good starting point, and this type of serialization format will be reused in the consensus layer of Ethereum after the Beam fork happened.

### Implementation Details

- The implementation has been **open sourced on our GitHub under the MIT license**
- We also made sure to **test the library across different operating platforms** to ensure compatibility
- This includes **Linux, macOS and Windows**

### Features Supported

In terms of the features supported by the library:

#### Core Serialization
- **Serialization and deserialization** of all basic and composite types
- **Support for union types** is also implemented but it is experimental at the moment. As far as I can remember, it is currently not being used by the Beam chain

#### Merkle Proof Capabilities
- **Support for Merkleization** is implemented as well, as the library can generate a **Merkle proof for any single item** in an SSZ object
- The ability to generate **multi-item Merkle proofs** is the feature we plan to add as an enhancement to the library

### Testing and Validation

Currently, the implementation **passes all required generic and static tests** which were generated using the **Python test generators** available in the consensus specs. For static tests, we use the **Phase 0 beacon state object** as it is a very complex type and it covers all the basic and composite types in its fields, which will ensure sufficient test coverage.

### Planned Improvements

We definitely have some improvements in mind alongside adding support for multi-item proofs, which includes **easy-to-use SSZ generators using macros**. This will make the library more user-friendly in C because it will abstract away the little complexities involved in defining serialization and deserialization logic.

---

## Completed Project 2: LibP2P Library in C

**MF**: So once SSZ was completed, we worked on developing **LibP2P in C** which was completed in **May this year**. This implementation as well has been **made public on our GitHub under the MIT license**.

### Why C Implementation from Scratch?

I would also like to provide more context as to why we chose to develop LibP2P in C from scratch. Before we started this project, we had the option to create an **FFI for the Rust implementation** which would expose callable functions that could be used for creating high-level protocols in C.

But we decided not to use this approach because:

1. **We wanted to avoid potential incompatibility issues** on certain types of IoT devices
2. **The LibP2P library has a lot of different components** and it would have made the design of the FFI a lot more difficult to implement correctly

### Current Library Status

In terms of the library status, it currently supports the following protocols:

#### Testing and Validation
- In our **local testing**, our implementation was able to correctly connect with the **JavaScript LibP2P library** for ping and other basic protocols
- By dialing a localhost client in our testing, **both dialing and listening worked correctly**
- But we only tested the **MPlex multiplexer** for testing YAMUX

#### Integration Focus
Our focus is to implement **interoperability test plans** and then test by connecting with other implementations.

### Future Development Goals

So our ultimate goal with the LibP2P library is to achieve **feature parity with all of the implementations**. To achieve that, we're working on **integrating the transport interoperability test plans** and use that to align any differences.

#### Planned Features
1. **QUIC Transport**: Once that has been implemented, we'll be able to implement support for **QUIC transport** as it has been gaining a lot of support in other implementations
2. **GossipSub Protocol**: For the Beam chain client, having proper support for **GossipSub protocols will be necessary** and that will be our main priority once support for QUIC has been added
3. **API Improvements**: We also have a lot of improvements in mind for the library's API and plan to integrate them over the course of time

---

## Next Project: SSZ with zkVM Integration

**MF**: Now I would like to provide an overview of the **next project** we want to make progress on. So we already have the C implementation for SSZ and we would like to extend it by adding **support for proving using zkVMs**.

### zkVM Integration Strategy

#### Starting Point
A good starting point for us would be to integrate **SP1**, but we also plan to add support for others including **RISC Zero and zkMIPS**.

#### Project Goals
What we want to accomplish with this project is to have a **library that abstracts away the integration complexities between zkVMs** that can be used for proving changes in SSZ and Merkle tree objects.

### Multi-zkVM Architecture

To achieve that, we will need to make sure our implementation **does not depend on a single zkVM**. It would be ideal to have a **redundancy model** where once several mature zkVMs are available in the ecosystem, users can set a **quorum**—for example, having **N of M proofs** that must be met before any state changes are accepted as valid.

### Cross-Platform Compatibility

Another important target would be to **make it work across different operating platforms** as we want to make sure our Beam chain client has compatibility with **as many devices as possible**.

---

## Conclusion and Future Collaboration

**MF**: So this concludes our presentation on the progress we made so far and our focus for the next phase of our project. We also look forward to **learning about the progress being made in other areas** of the Beam roadmap and are excited to **collaborate on anything that helps accelerate our collective efforts**.

Thank you for your time.

---

*[End of MF's presentation]*

*Note: Transcript generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*