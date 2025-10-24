# Lean Day - Ream Team Development Update

| Item | Details |
| ---- | ------- |
| **Session** |  Development Updates - Ream Team Progress |
| **Speaker(s)** | Unnawut Leepaisalsuwanna (aka 'O'), ReamLabs - Team Lead |
| **Slides** | [link](https://docs.google.com/presentation/d/1FhDGb2INtzzZPup6eOKGBZa_Qwgg6GQCrnejkxv2M3A/edit?usp=sharing) |
| **YouTube** | [link](https://youtu.be/KvkcH3rJWY0?feature=shared) |

>back to: [Lean Day Overview Page](/workshops-and-interops/2025/lean-day-EthCC/00-index.md)

---

## Ream Team Development Update

### Introduction and Team Overview

**O**: Hi everyone, so my name is **O**. You can call me by my nickname as well. That's **O**. So I represent **Ream Team** today. So I'm the coordinator for Ream Team. We have a few members. **Kolby, Tee-Hoon, and Jin** in the audience as well. So feel free to come up to us to discuss more.

Just a quick update. So basically we are **Ream Labs**. So we are globally distributed, Rust-based, mainly primarily focused on building the Beam client. We have **three tracks of work**. I'll mention a bit more later on.

#### Global Distribution

We are globally distributed, as I mentioned, because we are living in **five countries across most continents**:
- **Tee-Hoon and Jin** are in South Korea
- **Kolby** is in Canada
- **Cherokee and Patrick** are in Canada
- Based in **Bangkok, Thailand**, and **Maryland**, and **India**
- Part of the team in the **UK**

So very globally distributed.

#### Communication Channels

There's a few channels that you can follow us:
- **GitHub**
- **Twitter**
- **HackMD**
- **Telegram**

---

## Roadmap Evolution

**O**: So yeah, so I just want to start off with our roadmap. When we started off our team, early in 2025, we basically thought our Beam specs—it's coming up, it's a year from having our Beam specs. So maybe we start off something simple, something that we are manageable.

### Initial Simple Plan

So we were focusing on maybe our first milestone: **P2P**, second milestone: **Beam state transition proving**, and so on and so on. Pretty basic, pretty straightforward.

### Expanded Scope

But after getting a few months into the progress, **the scope got much bigger**. So we are now basically doing about **three tracks**:

1. **Building a minimum Beam client implementation**
2. **Prototyping Beam features**
3. **Creating Beam awareness and go-to resources** and also talent building, in terms of protocol engineers

---

## Track 1: Minimum Beam Client Implementation

**O**: Track one, I think our main output here is our **Beam client**. So it's a Rust-based, minimum Beam client. Our objective here, three problems:

### Three Objectives

1. **First** is we use this codebase to **train up new consensus client engineers**
2. It's also the **testbed for Beam chain features**—so a lot of things that we are experimenting with, the Beam specs or the research that are coming out will actually require some codebase to test against. So this beacon codebase helps a lot
3. **Thirdly**, we could potentially use this during the actual transition or the actual hard fork that could happen. We could potentially use this codebase as the one that's running as well

### Progress So Far

Our progress so far:
- We have developed the **full beacon STF up to Petra**
- A lot of the **P2P code are in place**—not complete of course, but most of it are in place right now
- The **validator progress is also mostly done**

I think we are aiming to actually try to run the validator but connect into other CL clients in the next month or so. And the new goal is basically to build this beacon client so that we can **run it on devnet and testnet as soon as possible**.

And you can see all the codebase here in that link below.

---

## Track 2: Beam Feature Prototyping

**O**: The second track is **Beam feature prototyping**. Basically, the objective for this track is doing **early engineering experiments with Beam chain feature candidates**.

### Progress So Far

Our progress so far:
- We have been able to do **per-operation zkVM** testing on **SP1**. So by what I mean by per-operation is like, you have process block, within process block, we have like process attestation, process block header and so on. So we are actually able to run those like process attestation, process block header inside **zkVMs** and those are **SP1 and RISC Zero**

- We have also implemented **SSF's implementation in Rust**. Hopefully we are able to convert those like SSF functions into zkVM as well. So potentially we can do a **SNARKified SSF version** as well

### Additional Engineering Challenges

There are other processes that as we build the beacon validator client, we figured out there's a lot of components that need to be hashed out for the Beam chain. For example:
- We are **drafting an ERC for quantum secure keystore format** because I think current keystore format uses not quantum secure encryption. So we are working on a draft on that ERC

### Near Goals

Some other near goals is that we want to run the **complete beacon STF on SP1 and RISC Zero**. We have already benchmarked those STF operations as well. So basically the other thing is **SSF in zkVM**.

### Open Source Work

Our work are open source so you can find out all the implementations for zkVM, for SP1, for RISC Zero, also the **SSF Mini**. Also we are also maintaining **developer notes**. So for whatever challenges we stumble upon or took quite some time to fix or find a workaround, we also write our dev notes here so you can follow all of our progress here.

---

## Track 3: Beam Chain Awareness and Talent Building

**O**: Our third track is basically **Beam chain awareness and talent building**. So basically we want to:
- **Attract new talents** to protocol engineering in general, having more people pay attention to the consensus layer, help build new things
- We want to **build up open and comprehensive knowledge base** for the Beam chain. So we built this **BeamRoadmap.org**. I think it's being used pretty frequently
- And also we want to **maintain the go-to resource for onboarding new engineers** who Beam chain effort

### Progress and Metrics

The progress so far in terms of the codebase, so far we are actually having **28 contributors**. So it expands a lot beyond our core team members. And if we count all of our repos including all the zkVM benchmark study group and so on, we have **38 contributors** so far.

#### BeamRoadmap.org Statistics

And in terms of the Beam roadmap itself, this is the first time I shared the numbers. So far we have launched this for **70 days**. We have had:
- **1,000 unique visitors** so far
- About **1.1K total visits**
- That's around **16 visitors per day**

It's not a lot if you compare it to big websites. But knowing that there are like 16 people who are interested in Beam chain and coming to visit and actually use these resources—yeah, I'm very happy with that.

#### Repository Statistics

Near goals is keep growing the interest here and also keep the website up to date, especially from the content from this event.

Yeah, and these are some numbers. So our repo, we had **96 stars** so far. We have opened over **338 pull requests**. I think it's more by now. **38 contributors** and also **200 issues closed**.

And these are all of the members that we—well, all the contributors have contributed to Ream and the repos that we have.

### Contact Information

So I think that's about it, our update. So to find out more about us:
- All our code is on **GitHub**
- You can follow our **status updates**. We have weekly status updates on **Twitter**
- All dev notes are in **HackMD**
- And if you want to chat more in the group, we have a public group in **Telegram**

Yeah, thank you.

---

## Q&A Session

### Question 1: Work Allocation and Focus

**Moderator**: It sounds like for both teams, building a Beam client is not enough. Zeam is kind of tackling the execution layer and you guys are tackling the beacon chain. **How much work would you say is being allocated to the Beam project specifically?**

**O**: So I think we can go by the number of people working on it. Our team is roughly more or less like **six people** right now. I think we have about **four people working on the beacon client implementation** and then **two are more on the zkVM stuff**.

### Follow-up: Engineering Synergies

**O**: And actually just to add on to it a bit, I think there are some interesting stuff that at least for us is interesting to do both the beacon client and the Beam chain stuff together. For example, in order to do this zkVM stuff, we figured out there are a few **engineering challenges** that we need to answer.

#### Serialization Challenges

For example, let's say that you have a host and guest machine for the zkVM. And you have this really huge state. **Where do you actually decide to serialize** and do you serialize the data from native Rust structs into SSZ? That impacts the performance. You can either serialize in the host or deserialize in the guest itself. There are some pros and cons.

So being able to figure that out I think is important for us. For example, we figured out actually **serializing inside the guest machine itself is more useful** because then in order to verify the Merkle tree doing the proofs and all that, you will require the SSZ format, not just the Rust structs. So doing that inside the zkVM even though there is some performance difference, the benefits is a lot more.

#### SSZ Library Limitations

For example, on the SSZ level there are multiple libraries in Rust that you can use SSZ with, but it turns out that **they all kind of suck for Beam chain**. And so Beam chain doesn't have zkVM [optimizations]. Some of the assumptions, for example, if you are going to do the hash, the Merkle tree hash root, I think all the existing implementations right now basically just **rehash or recompute everything every time** there is a state transition function.

And that's not possible to do inside the zkVM because that's a big performance hit. So we could use SSZ **partial updates** and a few more tricks, but then those are actually part of the SSZ specs, but I think no one actually took care of actually implementing those parts.

So finding those things and actually deciding whether you are going to have a workaround or we can rewrite something, I think that's some of the engineering questions that we actually discovered by actually implementing the beacon client and tackling the Beam chain features.

### Comment from Moderator

**Moderator**: I mean RISC Zero is able to SNARKify a beacon client and my guess is that they're not even using the partial updates. So maybe there's a massive speedup that RISC Zero gets just by doing partial updates.

**Moderator**: Okay, any last questions? If not, thank you so much.

---

*[End of O's presentation]*

*Note: Transcript generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*