# Lean Day - Post-Quantum Signature Aggregation Testnet

| Item | Details |
| ---- | ------- |
| **Session** |  Post-Quantum Signature Aggregation Testnet for Beam Chain |
| **Speaker(s)** | Thomas Coratger (TC), Ethereum Foundation - Cryptography |
| **Slides** | [link](https://docs.google.com/presentation/d/1CI72eI1BFARqkhbWdzBnE-rW1decOKgzx24SxChd-ek/edit?usp=sharing) |
| **YouTube** | [link](https://youtu.be/iNxd4CsmyGg?feature=shared) |

>back to: [Lean Day Overview Page](/workshops-and-interops/2025/lean-day-EthCC/00-index.md)

---

## Post-Quantum Signature Aggregation Testnet

### Introduction and Urgency

**Justin**: This post-quantum testnet has quickly [become] available and hopefully by the end of this week Thomas can prepare for the report. These are all the next steps to get that testnet.

**TC**: Okay, Thomas. Thank you, Justin. So hello. I'm **Thomas**. I work at the EF as a researcher or product manager. Call me what you want. Yeah, I'm going to put a slideshow.

So today I'm going to present you briefly the **post-quantum Ethereum** and more precisely the **post-quantum signature aggregation**. I will try to do an overview talk. A lot of technical details have been taught this morning, so I will not go again over this. But more about **what we have done so far, what needs to be done and what needs also to be done right now**.

---

## The Post-Quantum Reality

**TC**: So about post-quantum Ethereum, as we can see, as you may have heard, **post-quantum things are not just science fiction**. It needs to be done. A lot of tech leaders say that **quantum computers can become a reality in five to ten years**. We don't really know, but this is a really important situation for Ethereum and **we need to be prepared**.

### Current Problem with BLS Signatures

So right now the problem is that we have **BLS signatures integrated into the consensus** and to prevent post-quantum attacks, we need to change this model and we need to have a **new signature scheme**.

So for this, we thought about different solutions. Of course, **XMSS is one of the most promising ones**. This has been presented this morning, so I will not go again over the details of this. But this is one of the possible candidates. For now it seems the best one, but we need to investigate also, of course, all the possibilities. You have amazing research posts that have been made about **lattice stuff** and all of these things, so this needs to be open.

### Challenges with New Signature Schemes

But the problem with this signature scheme is that the **aggregation of them to submit a proof on Ethereum is a bit more complex than BLS** that was very easy to aggregate. And also the **signature size**. It's a bit larger than BLS was with **2.5 kilobytes or 3 kilobytes approximately** against only **96 bytes for BLS**.

So we need to handle this larger signature and also aggregate them. For this, as XMSS is a hash-based signature, as it was presented this morning, we need first to **benchmark hashes in STARKs**.

---

## Hash Function Benchmarking Results

**TC**: And so it was our first idea to do this benchmark last year. And so what we did is that we benchmarked different hash functions such as classical ones, **BLAKE3, Keccak**, and **SNARK-friendly one, Poseidon 2** over different proof systems. To see a bit, what can fit, and what would be the best choice, both as a hash function and as a proof system.

### Clear Winner: Poseidon 2

So as you can see on these charts, **Poseidon 2 clearly stands out**. This means that with Plonky3, as you can see on the left, you can actually prove in a STARK, approximately **one million hashes per second on a classical CPU**, while the other hash functions such as BLAKE3 or Keccak are way, way slower.

So this led to the fact that probably, at the moment, **Poseidon 2 is our best choice for the hash function**, and our initial benchmarks by studying all the proof systems and studying the different hash functions revealed principally **three major trade-offs** for on-chain usage:

### Three Major Trade-offs

1. **Prover speed** - We need something that is fast to prove to be used on-chain for the Ethereum future
2. **Reasonable proof size** - We cannot afford one or two megabytes proof. We need something around **128 kilobytes**, something around this
3. **The bottlenecks that we have right now between FRI performance and proof size**

---

## Proof System Analysis

**TC**: So I have listed different elements in the table where I put different comments:

### Current Assessment

- **First, Poseidon 2 is the best choice for now** as a hash function
- **Second, FRI needs very fast proofs but they are often too large** of the order of 1 to 2 megabytes
- Recently, we talked with **Giacomo**, with the author of the **Whir paper**. And Whir is a reasonable, optimistic way to do a PCS. Why? Because the prover speed needs to be improved but we know that there are a lot of rooms for improvement and we are discovering that each week
- But on the other side, they achieve **very small proofs of the order of 128 kilobytes and very fast verification time**

So this could be a very promising alternative and **we pushed a lot of effort on this**. And we have also three different other topics that are under construction and that I will detail over the next slide.

---

## Whir: Our Chosen Direction

**TC**: So as I just mentioned, at the Ethereum Foundation starting from all of this context, we started to push our effort. So **Whir** basically is a PCS, so a **polynomial commitment scheme** that can be directly introduced into our proof system to aggregate together signatures.

### Whir Technical Principles

So it is based on two main principles:

1. **Recursive folding**, inspired by STARKs and FRI. So at the beginning, you start with a very big polynomial problem and then you shrink your polynomials step by step, round by round, similar to STARKs or FRI

2. **Plus, you integrate during each round, the sumcheck** that is inspired by Lasso and Jolt—both are works that are doing sumcheck for having at the end very lightweight verification

### Performance Characteristics

So now, if we try to integrate this PCS into our signature aggregation, we can have:
- **Very tiny proofs of the order of 100 kilobytes**, that is perfect for us
- We can have also **microsecond verification** and this is very important to have this low-query complexity to build proofs as verifiers that can run on-chain
- And we have also, finally, the **post-quantum alternative** to what we know today

### Current Performance

So right now, with this setup, we can achieve on a quite performant GPU, like a **4090, one million hashes per second** over a full proof system with this tiny proof. And on the **CPU consumer laptop, we achieve recently like 100k hashes per second** and our final goal is to have **one million hashes per second** to be really performant and be able to consider that as a possible alternative to aggregate signatures.

---

## Three Ongoing Efforts

**TC**: Related to that, we have two different ongoing efforts and one static effort:

### 1. XMSS AIR

So the first one is the **XMSS AIR**. So it's a work led by Han from the beginning of the year. So it's a transformation—the transformation of the XMSS signature into an **AIR constraint system**. You have a repo and a technical note there where you can find all of the details about this arithmetization and all of the work that has been done.

### 2. PIOP System

And also on the side, we have a **PIOP system** that has been developed by **Emil** and that is **tightly coupled with Whir effort** because we can make the two together to have a full proof system to aggregate signatures.

So this basically encodes the entire system into a **single multilinear polynomial** instead of univariate polynomials on each column and it uses **sumcheck instead of the quotient polynomial**. So thanks to this, we can have very small proofs and we are starting to optimize week after week.

### 3. Recursion Effort

And we started very recently the **recursion effort**. We have a lot of background, a lot of research that we have done since a couple of months about the recursion, meaning that we should be able to **aggregate together fresh signatures but also fresh signatures with proofs of aggregation of signatures**. This is basically the recursion that was explained by Emil this morning.

---

## Recursion Solutions and Research

**TC**: For that, we have different possibilities and a lot of research to help:

### Research Infrastructure

- We have the **zkProof initiative** from the Ethereum Foundation where we have a lot of feedback, a lot of alpha from the different zkVM vendors and this is a place where we share some numbers, benchmarks, solutions, and all of the research ongoing about the zkVMs

- We have also a lot of **academic research papers** about folding schemes, accumulation schemes, lattice techniques that can lead to some recursion

### Two Solution Approaches

So the solutions that we have identified as possible for the recursion are twofold:

#### Long-term: Folding and Accumulation

We have a **medium-long term perspective** that is about **folding and accumulation schemes** and a **short-term solution** that is a **minimal zkVM**.

On the long-term side, the **folding accumulation technique seems very promising** because this is a single mathematical model. You can code that, but we need much more feedback and much more perspective on adoption plus implementation and tooling. We can think about **Jolt or Nova** on the lattice side, but we have at the moment **no existing implementation** so we can have enough feedback to say that it will be the proper solution.

#### Short-term: Minimal zkVM

But on the shorter term, we have the **minimal zkVM**. What is that as described by Emil this morning? This is a **very minimal zkVM that can handle this recursion**.

And on this side, we already have a lot of feedback and we know that **it's tractable** because we know that all the zkVM vendors, almost 30 at the moment, have done general purpose computation with zkVMs using a lot of different techniques that we can grab inspiration from.

### Design Philosophy for Minimal zkVM

But here, our **design philosophy will be completely different** because:

- **We don't need to be general purpose**. We need to be **application specific** because we only need to aggregate signatures and not do any arbitrary computation. This is not the purpose of this

- We need also, as a result of that, a **very minimalistic ISA** with probably **one or two built-in coprocessors** and not ten of them

- And we need also a **very simple, predictable recursion model** and a **very simple memory model**

---

## What Still Needs to Be Done

**TC**: So to finish the talk, I will just mention—and it will open the session about the round table—**what else needs to be done?**

### 1. Comprehensive Specification

So we need a **Python spec**. This will be a reference for future implementations to onboard new contributors, to onboard client teams, for them to have a reference for the implementation. This is very important and this will be a tricky one because we need to have the spec for different aspects:

- For example, we have the **PCS that needs to be specced out**
- We have the **PIOP, we need to be specced out**  
- And we have also probably the **minimal zkVM, the hash function, the signature model**

So we have **a lot to spec**.

### 2. Integrated Testnet

And also a **testnet**. So at the moment, we have only worked separately on the signature aggregation program. But we have **never tested compatibility with all the other buckets of work** of the blockchain.

So we need to **run the model in reality with the other components, the P2P layer**, to know if this will be a good fit and if everything will be okay.

So yeah, that is the end of my talk and probably opens the round table.

---

## Summary of Progress

### Completed Work
- **Hash function benchmarking**: Poseidon 2 identified as clear winner
- **Proof system analysis**: Whir selected for optimal size/speed trade-off
- **XMSS AIR implementation**: Hash-based signature arithmetization
- **PIOP system development**: Multilinear polynomial encoding
- **Performance milestones**: 100k hashes/sec on laptop, 1M hashes/sec on GPU

### Technical Achievements
- **Proof size**: ~100-128 kilobytes (vs 1-2 MB alternatives)
- **Verification**: Microsecond verification times
- **Post-quantum security**: Full quantum resistance
- **Recursion research**: Both folding/accumulation and minimal zkVM approaches

### Immediate Next Steps
1. **Python specification**: Comprehensive reference implementation
2. **Integrated testnet**: Real-world compatibility testing with P2P layer
3. **Performance optimization**: Reach 1M hashes/sec target on consumer hardware
4. **Recursion implementation**: Choose between long-term vs short-term approaches

**TC**: Thank you.

---

*[End of TC's presentation]*

*Note: Transcript generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*