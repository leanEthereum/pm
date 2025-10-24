# Lean Day - Zeam Client Development Update

| Item | Details |
| ---- | ------- |
| **Session** |  Post-Quantum Recursive Signature Aggregation for Beam Chain |
| **Speaker(s)** | Gajinder Singh (GS), Zeam - Team Lead |
| **Slides** | [link](https://docs.google.com/presentation/d/1sPNjnSpua6txLuRIIHSfEDkqE_tQKEEmxvLusqdztfQ/edit?usp=sharing) |
| **YouTube** | [link](https://youtu.be/LZ1zUvbh42k?feature=shared) |

>back to: [Lean Day Overview Page](/workshops-and-interops/2025/lean-day-EthCC/00-index.md)

---

## Zeam Client Development Update

### Introduction and VDF Trophy

**Moderator**: I'm sorry, okay, yeah, it is 2:06. We're going to get started in just a few seconds. So this afternoon we're going to have some dev updates from the three different teams that have actually started doing experiments and coding with the Beam chain. And then we're going to have two round tables. One on single slot finality and the other one on reducing the validator count.

Our first speaker here is **Gajinder from Zeam**. And I have a special trophy for him for making it today. It's the **VDF**. So there's only a hundred of those. I have been manufacturing it. This is a 100 NFT and it works. It probably won't be used in production, but it works. It's like Zeam.

**Audience**: [Applause]

**Moderator**: But soon hopefully soon we'll be running in production as well.

**GS**: Cool. Thanks Justin. And we're excited to be here. Great energy. And let's get into it.

---

## Zeam Mission and Vision

**GS**: So I'll talk a little bit about **Zeam**, and since Justin unveiled the ZK Rails for the consensus vision at DevCon, we have been on a mission. And then we'll talk about phase one of that mission. And one of the outcomes of that mission is **Zeam Multi-Prover**. And the fourth point is that we'll also talk about moving with **SSF** because SSF is something that we integrated into our STF function. And then we'll make our **DevNet announcement** hopefully.

### Why Zeam Exists

So the purpose of Zeam's existence is that we wanted to do the **ZK consensus client, Beam client from first principles** because we felt that **Zig** was a language which is highly performant and very versatile to build the target builds, target guest programs that can be proven on zkVMs.

And so that was one of the big motivations. And all of our work you can find us on the **GitHub organization** and particularly at GitHub Zeam. And while me and [colleague name], the primary leads of this project, we have a very vibrant contributor community that is emerging.

---

## 2025 Mission: Two-Phase Approach

**GS**: And so let's talk about what we have been doing. So our **mission for 2025** is to build a proof of concept to demonstrate the ZK Rails in action. And for that we have integrated zkVMs—at least one, and we are working on many others. And while doing that, we will be building various libraries that are relevant to zkVM systems and the community.

So that by end of the year when Beam specs are released, we are Beam-ready. And our mission is divided into **two phases**.

---

## Phase 1 Progress (First 6 Months of 2025)

**GS**: And so let's look into what we have done in phase one, which is the first six months of 2025. So phase one—there are **four pillars** to phase one:

### Pillar 1: Building Blocks and Libraries

These pillars are building the building blocks and libraries, doing state transition function proving, and then basically building up the zkVM client, which does end-to-end proving as a devnet. And the last pillar is community.

So on **building blocks**, we have implemented **SSZ**, which is extendable, and we basically extended it with **Poseidon 2** now. We have been doing optimizations on it, but that is not really the focus because first we want to demonstrate the basic concepts.

On **P2P layer**, we have done some P2P work, but there is a lot to be done over here, especially with the P2P networking research that is ongoing. And then we will have more clarity on it than we will see how we can contribute to it. But for now we will just use what Ethereum researchers are going to turn out over here by using Zig bindings on it.

So in our estimation, this is **80% done** for phase one.

### Pillar 2: State Transition Proving

And then on **state transition proving**, we have achieved proving and verification with **RISC Zero**. And so basically, you need to have state transition function, but we went beyond a basic state transition function and pulled from our phase two goal and integrated **SSF**—with our mini-SSF into our state transition, and we will talk about it later.

And so we also did quite a bit of work on **Powdr**, which we are now pivoting from with OpenVM, but there is still some work. I mean, this work needs to be restarted with Powdr dropping its support for zkVM and VM—Gam mentioned about this in his morning presentation.

So for our phase one goal, it was to basically at least have **one zkVM proving**—so that is done, but yes, we are focused to get other zkVMs working as well.

### Pillar 3: Client Development

And for the **client**, which is the third pillar, we have developed various modules. And most notable of them is **fork choice**, which is now SSF fork choice, again, cross-implemented from Vitalik's work. And then one of the core modules is **proving manager**, which we will also talk about a little bit later.

And now the goal—so there are two things that are pending for now, which we are focused to get out, which is having a **multi-node in-process devnet**, where basically the chains are building on basis of SSF functions, and then the next step would be to make it a multi-node, by having integration with the Rust LibP2P.

So again, we think that we are **80% done** over here.

### Pillar 4: Community

And the last bit is **community**, and we are having active weekly calls. We have active Telegram and X groups, and we have contributors that are working on various initiatives like **Zig Hash, Zig LibP2P**, and having SSZ integrator with our SSZ. But yes, we are trying to make sure that we can get focused outcomes here as well. And we want to collaborate with **EPF** on that and start some initiative there as well.

So here, again, we are like **80% done**.

---

## Phase 1 Outcomes and Timeline

**GS**: So outcomes of this are intended to be a **multi-node devnet** and basically **multi-ZK proving**, multi-proving with different zkVMs. And then we want to basically also integrate **Poseidon** into our SSZ and sort of demonstrate how it will work. And maybe also then do some benchmarking on it.

But as Gam mentioned in the morning, **benchmarking is not that much of a concern right now** because we are focused towards getting the functionality working first. And we think that we'll be able to do it by **end of April**.

---

## Zeam Multi-Prover Architecture

**GS**: So let's look into one of the outcomes of our work, which is **ZEAM MULTI PROVER**. And this is something that you have already seen. But again to reiterate, we have a basic client which basically needs to interact with zkVMs and runtime, produce proof and sort of verify it.

And for that, basically you need to give it **state input**, you need to give it **block that you want to prove**. And then the proof will be generated which will be transmitted over the network. And the receiving nodes will also receive the **state diff** and basically verify that in zkVMs. And basically then they will be able to attest to the network.

### Guest Program Anatomy

Again, going a little bit deeper into it, let's look into the **anatomy of the guest program**. So you have three components which make our guest program:

1. **Instruction Set** - on which zkVMs are—you can think of that circuits are based, the proving circuits are based
2. **Proof Systems** 
3. **Runtime** - which is the state transition functions that various third party suppliers will provide to a client or the client's self-built

So we have quite a good diversity over the **instruction sets** and major being **RISC-V**. And if you look at zkMIPS, you will also see **MIPS** in action. So other instructions are **EVM, Bora, Cairo, Valida**, and all of them are different zkVMs who are targeting these instruction sets.

On **proof systems**, you have **Binius and Plonky3** as the major proof systems. It seems that Binius needs some more work to make it performant. So most of the proof systems are Plonky3, but we also are seeing some custom backends that zkVMs are developing like **SP1 Hypercube**.

And then you have the **runtime providers**, which most of the zkVMs might provide a default runtime, and then you have players like Zeam coming into the picture and providing runtimes to various clients through proving and verification.

### Multi-Proving Process

So now let's look at how **Zeam multi-proving** actually happens. So you have a guest program. But it's basically not just a guest program—it's made up of **runtime**, which basically for us is RISC-V code that is compiled. And that RISC-V code is basically state transition function that we have written with some runtime glue, but it also needs an **invocation glue**, which works with that particular zkVM.

So this is where multi-proving comes in, this is where Gam's work comes in, which he mentioned in the morning, that it conceptually is simple, but **you have to figure out how to hook it all up** to make it work.

And then in Zeam you have, for example, multiple different guest programs and you have multiple different zkVMs that are out there. And they are all coordinated by something we call **Proving Manager**.

### Proving Manager Architecture

And so that Proving Manager takes a **pre-state**, it takes a **block**, and it takes **proving options**, and then basically selects—or it can do multi-selection as well. It selects whatever zkVMs you want to prove on, and then picks that particular runtime, generates the execution trace, commits, and then gives you proof, and something that again you can run back on to the verification chain.

So as you can see over here, this is our **Proving Manager**, which basically takes the state, block and proving options, and what it does is basically serializes that input that it receives and sort of provides it to the **invocation glue**, which then routes that to the zkVMs.

And so this invocation glue then starts up the runtime in the zkVM itself. And the runtime receives that input from zkVM, it deserializes it, and then it runs the **state transition function**, and then you have output—then you basically commit to the output and you have proofs.

So here you can replace this OpenVM with Powdr, but this is essentially what you saw here as well. So we have achieved **proving and verification with RISC Zero**, and now our task is to move to the next one. We did devote a lot of work to Powdr, but it has basically gone down. Our priority this time most probably will either be picking up with **SP1** or with **Jolt**.

---

## SSF Integration

**GS**: And coming to the second part, which is the **state transition function**—so what we did was we pulled from our phase 2 goals, and we decided that this looks quite exciting. So we translated **Vitalik's Mini SSF** into Zeam, and for that basically we came up with **SSZ containers** for that, and then the state transition function as well as implemented the **fork choice**.

### SSZ Containers for SSF

So the containers for SSZ containers for SSF looks something like:
- There is a **checkpoint** to which there is no epoch now, so there is root and a slot
- And then there is a **vote**—vote has on a head, target and source, and all of them are basically the slot root checkpoints
- And you have **block body** where basically you have series of these votes—they are not aggregated arrows now
- And then you have **state** where you track latest justified, latest finalized, and what roots are being justified and who is justifying them

So the roots that have not yet been justified, but are in content are **candidates for justification**.

### State Transition Function

And then we have an **apply state transition**—this is our apply state transition function, which is just like how you do it in beacon consensus:
- You have **process slots**—basically you roll the state to the current slot
- And then you **process the block**, and in process block basically you make sure that you process the block's parent matches with what state's last block header raised
- So you do that to make sure that you establish that this is a chain
- And then you do **process operations**, which is where we apply the votes
- And in the end basically you verify that the **state root** that you get is same as the root that is in the block, and that basically serves as very basic state transition function

### Vote Processing Implementation

So looking at how we **apply the votes**, there is a bit of difference from Vitalik's implementation, because his implementation assumes starting the slot at slot 1. So this is something that we did—okay, we start the slot, keep the genesis slot at slot 0, and this is some of the modifications that we did, and that is the other work that we translated.

And basically we wrote up some scenarios to prove, and that is something that you can just go to our repos and execute, and it will do this particular scenario proving for you.

---

## Updated Ambitious Goals

**GS**: So coming back to our ambition, we are like, you know, **85% complete on phase 1**, and a little bit on phase 2 as well. But we are here to make an **exciting announcement** that with all the incredible acceleration that has happened in the zkVM space, we have sort of updated our target to a more ambitious one.

### Phase 2: EL Integration

And we will basically do **EL integration**—which is okay, I mean it is not really that ambitious—but we will also implement the **latest execution** to it, because we want to do **full block proving with EL and CL**, and with that we want to demonstrate how you will work without EL, because the **receiving client will not have any EL attached to it**, and we will be able to attest to the network without EL.

So we are having **any EL**, and yep, that's all.

---

## Q&A Session

**Moderator**: Thank you, Gajinder. I don't realize that you are doing this big announcement. Any questions for Gajinder?

### Question 1: Benchmarks and Proof Sizes

**Audience Member 1**: I'm just curious, any benchmarks, any of the proofs, like how big are they?

**GS**: So all that is happening, I think that we'll help for that. We will benchmark our state transition functions.

**Moderator**: Any other questions?

**Moderator**: Okay, thank you so much, Gajinder.

---

*[End of GS's presentation]*

*Note: Transcript generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*