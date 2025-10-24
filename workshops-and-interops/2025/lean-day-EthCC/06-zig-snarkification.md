# Lean Day  - Zig Client and zkVM Integration

| Item | Details |
| ---- | ------- |
| **Session** |  Zig Client Implementation and zkVM Integration |
| **Speaker(s)** | Guillaume Ballet (GB) Zeam & EF - Geth |
| **Slides** | [link](https://drive.google.com/file/d/1-l6WLNlxZ0US-_qtuZRrlr6nh_XO0GCg/view?usp=sharing) |
| **YouTube** | [link](https://youtu.be/SdACDp0p_A4?feature=shared) |

>back to: [Lean Day Overview Page](/workshops-and-interops/2025/lean-day-EthCC/00-index.md)

---

## Zig Client Implementation and zkVM Integration

### Introduction and Project Overview

**GB**: Yeah, so Gajinder and I are working on the Zig client, which is a Zig client. Thanks for making it, Gajinder. It was quite difficult indeed.

And so as part of this work, there are basically two components. You have the client that with the network connection, you know, producing blocks and loading blocks. And you also have the proving side, which is a prover and against—so you have a guest client inside the zkVM. And we've wrote our own guest program and this is what I'm going to talk about during this presentation.

As a disclaimer, I had a presentation about this very similar topic at Protocol Berg two weeks ago. So I learned that the video was published yesterday. So if you wish that there's going to be a bit of a redundancy, but there's still some new stuff.

---

## Guest Program Architecture

**GB**: So yeah, what is the guest program about? While the structure is very simple. You have the business logic—the state transition function. And you have the runtime, which is where I guess the most interesting part of this talk will be about. And then you have the zkVM, which is the one thing that executes the program. And we'll take samples, build a polynomial, do all the fancy proving stuff.

But yeah, like the biggest challenge is to build a state transition function program as an ELF that runs into the zkVM. And what you have to understand is that those zkVMs, except RISC Zero—I know what you guys are doing—is very much like an embedded target, which means that many languages will not be able to really work into this, unless they have some kind of Linux emulation layer. Once again, good job RISC Zero.

And yeah, so you have to typically emulate the OS and the libc, and this is what the runtime is doing.

---

## Language and Runtime Challenges

**GB**: So in order to build a guest program, you only have two options. Either you take the stock SDK that is provided to you with the zkVM, which is typically written in Rust, and you also have the Rust runtime. If you want to use a different language, like Zig, you can build your own STF, but still somehow linked to the Rust runtime, which can be challenging, especially when it's not really available.

So, sorry for saying that again, but with Zig, everybody else that I interacted with does not [support it]. So yeah, basically, if you want to use a different language, right now, your only option is to also rewrite the runtime, and this is what we did.

### Why Zig?

I think for the short term, or at least we see our mission also to evaluate how good those zkVMs really are in terms of programmer experience. So for us, it's a no-brainer—we have to do the Zig runtime. We have more control. We also have a more streamlined process where we just write an independent STF, and then we have zkVM-specific code, but that's the burden we're taking.

And the reason why we bother with all this is to provide—I mean, there's many reasons, really, but I would say the most consensual one is **runtime diversity**. If we're proving a runtime, like a single runtime, and there are bugs in it, you're proving the bugs. So being able to find issues, and we are finding a lot of issues, it's quite useful to take a slightly different path to get there.

---

## Program Flow Architecture

**GB**: So I'm going to summarize the program flow, and we have, once we have the state transition function, which is code that we write once, and then we have this zkVM-specific code in ELF, which is, like I said, the interesting part of having this—we don't have to write an abstraction layer. We just have our specific code, we swap it, and we can build many programs in one go.

### Execution Flow

So yeah, the idea is that the program starts in the **boot code**, it does the initialization, it gets to the state transition function. Then it gets the input, which typically will be a block that you want to prove from the state and things like this. Goes back to the state transition function, does a lot of zkVM code like hashing, for example, or committing to values, and then does more things in the state transition function. And finally, I should have said, how does data exit? It calls the host system call, and the program terminates.

---

## zkVM Support Status

**GB**: What do we cover so far? So that's one of the things that is different from Protocol Berg. We have **RISC Zero support**—we are able to prove everything at the end. In Protocol Berg, I used to have Nexus, we decided to drop it because it's just not production ready at this time. So we have the code saved for when they have something functional, but currently it's just not worth our time.

### Current Status by zkVM

- **Powdr**: We have powdr, we have the guest program, we have the prover. There's a lot of issues with the verifiers, so I started working on fixing this in powdr. And powdr announced that they were discontinuing their RISC-V VM, so this line is going to drop very soon.

- **Jolt**: And then we started looking into Jolt, and the interest of Jolt—especially interested in Jolt, because it's a RISC-V 64-bit target, which is also—I'm also looking into building guests for Jolt, so it's quite interesting to have a RISC-V 64-bit target, because yes, we will not compare to a RISC-V 32-bit target.

- **SP1**: SP1, we also started, but communication is a bit slow there, so currently it's on the kind of a back burner.

### Future Targets

And then we have the three remaining zkVMs we want to look at:

- **OpenVM**: Simply because we hope we can reuse some of the powdr code to put it in there. The powdr team wants to become integrated into zkVMs, so hopefully the work will be fast on this front.

- **zkMIPS**: And then there's zkMIPS, the only reason is also because it's not a new architecture, and I'm also in the back of my mind, I have "how am I going to compile Zig for MIPS?" So that's also something to investigate.

- **Wasm**: And then there's powdr maintaining the Wasm VM, so we want to look into this afterwards.

---

## Multi-Prover Architecture

**GB**: Yeah, so I was saying, when we want to build the zkVMs, we build all the guest programs in one go, and here you can see—maybe if I can explain here—we have some **Rust glue**, and I'm going to explain what this is later, but we first build the Rust glue to connect to the zkVMs, so here we have powdr and RISC Zero, and then we build the zkVM guest program, so we build them all in one go.

And it's all the same state transition function—we just changed the tiny glue that is there to connect to the zkVM, so we can basically have a future where we have a **multi-prover**. We receive one block, we do all the proving with all the zkVMs, and then we can produce those proofs.

---

## Client Architecture

**GB**: Yeah, so what does the client look like? Well, I forgot the legend again—in yellow you've got **Zig**, and in orange you've got **Rust**. And so you have the classic client code, so the networking, the state management, you receive the block, you do the tree computation, the hashing, everything.

And then when you need to do the proving, you have to go through a **Rust glue** into the zkVM prover, that will kick-start the runtime, execute the state transition function, and come back with the proof. This goes over the network, and then it comes back in another client, into over the Rust glue to the verifier.

### Need for Standardization

And I think what would really improve is if we could come up with a **standard interface**, so we had a discussion about this at the interop in Berlin two weeks ago. Yeah, basically the writing this glue, and the runtime is a lot of duplicated code, and in the case of Rust it's even worse, because Rust is a zkVM, to be fair, but we don't have this problem directly.

But when we have to build some Rust glue, each zkVM has their requirement for a version of the compiler, so currently we're quite fine, but I assume that when we start supporting provers in many, many zkVMs, we're going to have an issue of compiling every glue bit with a different version of the compiler.

### Proposed Solutions

So this is something that would be extremely useful to have—**verification libraries** that are either in C, or Go, or Java, or not, so that every client could just not have this problem of building some Rust glue with many different compilers.

Otherwise, yeah, when RISC Zero did provide a runtime library that you can link into a C program would be awesome, and yeah, that's pretty much it.

---

## Performance and Benchmarking

**GB**: So yeah, I don't have any numbers yet, because it makes no sense, like we don't really have a spec—a finite spec—so we didn't really want to benchmark the execution itself, it makes no sense, because we don't know what it is. So we're doing in terms of business logic, but yeah, for example, for powdr, it takes about **four seconds** to do, like, just update the state and recompute the tree.

So if you're interested in numbers, you can always run the program and tell you how much it takes, but this is not the right time to provide numbers.

Yeah, that's pretty much it.

---

## Q&A Session

### Question 1: Runtime Complexity

**Audience Member 1**: How complex is the runtime?

**GB**: The runtime is, I mean, conceptually very simple, in principle—like the devil is in the details. So what's complex is figuring out what the zkVM does under the hood, so you have to go and read the Rust code and realize how this VM does it this way, but this other VM does it the other way.

Yeah, so the runtime itself is not complex, it's just maintaining multiple versions of it.

**GB**: Let's start.

---

*[End of GB's presentation]*

*Note: Transcript generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*