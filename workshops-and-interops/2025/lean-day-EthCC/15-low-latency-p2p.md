# Lean Day - Low-Latency P2P Networking

| Item | Details |
| ---- | ------- |
| **Session** |  Low-Latency P2P Networking for Beam Chain |
| **Speaker(s)** | Csaba Kiraly (CK), Ethereum Foundation, Geth |
| **Slides** | [link](https://drive.google.com/file/d/1fSsSd1N2c7X18sMfoKRqSf2QXD_PohXn/view?usp=drive_link) |
| **YouTube** | [link](https://youtu.be/fGig7U9ld_c?feature=shared) |

>back to: [Lean Day Overview Page](/workshops-and-interops/2025/lean-day-EthCC/00-index.md)

---

## Low-Latency P2P Networking

### Introduction and Context

**Csaba**: Let me start with a little bit of context. When we are speaking of network community, there are a bunch of things there, which we are dealing with. And we are doing improvements on all of these. And when we are scaling, any of these can go bad. And any of these can be the bottleneck. But today, we will focus on these things.

So when we are speaking of **low latency**, we are seeing block diffusion, attestation propagation, and blob dispersal.

---

## Current Ethereum Networking Overview

### EL and CL Networking Components

**Csaba**: For example, in **blob dispersal**, what we have is sharding and data availability. And what we have is kind of—we are just doing **partial delivery**. So the nodes are only receiving part of the actual data. And this is optimizing bandwidth, right?

#### Bandwidth Efficiency Numbers
- With **PeerDAS**, we are going down to **12%** of the usage
- With **FullDAS**, we are going down to—I mean, these are theoretical numbers, obviously, we have a bunch of overhead:
  - For validators: **1.5%** of the data
  - For samplers: **0.1%** of the data that we are getting

And in FullDAS, we are already including a bunch of techniques to deal with low latency.

---

## Path to Scalability

### Slot Time Improvements

**Csaba**: Actually, we saw the times. What we are doing, we are kind of keeping the throughput—the same **transactions per second** or whatever per second is typically same. But we are getting **better granularity** in the system. So for example, we are getting **shorter inclusion delays**. If you have a transaction which is sent to the mempool, it just has a chance to be included faster and other benefits.

And we all know that this requires **better diffusion latency**.

### GossipSub Improvements

So we already got to—because we've surveyed two months. And apart from the GossipSub improvements that we are doing for **bandwidth efficiency**, reducing duplicates typically. Some of them are about **better diffusion latency**. And we also want to keep the protocol **robust**.

### The Trade-off Triangle

And I was doing that nice triangle there, because people like that. Because that's the difference that you have to make compromises and trade-offs. And I think that **bandwidth efficiency, diffusion latency, and robustness** are compromises. Unfortunately, the angles are not always the best model. We also have **privacy**. And I think we could, if we think more, we could add other dimensions.

So yeah, we have to take compromises and different techniques achieve different things.

---

## Part 1: "Propagation Speed is the Problem"

### Historical Block Diffusion Data

**Csaba**: We all know propagation speed is the problem here, roughly, basically. So this is a measurement we did in **2023** on blocks. How do we do this? To make it difficult? This is a log of a big scale. But this is delay here—four seconds. And these are different size blocks. So these first small blocks, everyone is happy. For large blocks, where are we? This was the situation.

Well, since then we developed a bunch of **low latency peer-to-peer techniques**. I co-authored the **low latency peer-to-peer toolbox**. And I want to speak of these techniques.

---

## Low-Latency P2P Toolbox

### 1. Segmentation (Chunking)

**Csaba**: So what are the techniques? The first one is **segmentation**, or sometimes we call it **chunking**. It's just about the fact that we're using GossipSub for something which it's not meant for. Like large messages—you just don't send large messages with GossipSub. If you do large messages through peer-to-peer networks, you are chunking it. That's the way to do it.

#### Store-and-Forward Delay
And you can wait and see. So there's this thing called **store-and-forward delay**, which is if you have multi-hop, you are always getting the whole message, and then you can forward it. If you have smaller chunks, then you can **fast forward**.

#### Implementation Progress
And we are doing it. We are going in this direction. We have this called **sub-message messaging** called **partial messages**, which is doing adding some message support. I think we will also have something like **sub-stream support** or something like that.

This, however, requires **verifiable segments**. So if you want to do this, all segments have to be verifiable somehow. We have KZG commitments, but we need something else.

### 2. Scheduling and Prioritization

**Csaba**: Our second tool is scheduling and prioritization. So if you assume that you are **bandwidth limited**, and you should assume otherwise, the game is easy, and just send it to everyone, and it's done. So if you assume you are bandwidth limited, you have to make choices. **Who am I sending it to? What am I sending?** And this is the scheduling and prioritization.

#### Current Limitations
And the current GossipSub is quite naive in this. So we will introduce things like **batch publishing**. And that's already in some other clients. And you can do things in **forwarding scheduling priorities**.

#### QUIC Transport Benefits
And I was putting QUIC in this category, because what we achieve with QUIC is very related. So we have **independent streams** with QUIC. We can prioritize between them. And then we can also do with QUIC something which is only available on some transports. So I'm sending a piece, and that is already timed out, and I don't need it. I don't have to descend, but I can send something else instead of that.

So this is scheduling and prioritization as part of our toolbox.

### 3. Encoding Techniques

**Csaba**: Next one, **encoding**. We love this. So encoding—Reed-Solomon encoding, fountain codes, random linear coding. These are techniques that we usually use in communications, and it wasn't really used here.

#### Benefits
And what we achieve here—so we actually—basically, I call it typical—you are **cutting the tail of the delay distribution**. So there are those cases when the delay becomes high for some reason, for someone. And you are trying to cut, chop it off. So in the sense that those are getting it faster.

So instead of waiting for the timeout and asking for a missing piece, you are just getting it. We are getting **encoding pieces**. This is **forward error correction**. Or instead of sending duplicates, you are sending another piece, which is a useful piece because of the encoding.

#### Trade-offs
So what you achieve is you are reducing the delay because we are cutting the tail of the delay distribution. We can use **Reed-Solomon codes**. We can use **fountain codes**. We can use **random linear coding**. There is a small downside that this is adding **coding delay**. So you have to be very careful. But it's a nice technique.

### 4. Network Coding

**Csaba**: Then we have **network coding**. Network coding has been there since 2005. It has been there for long. We might apply it. It's basically about in the network, we can do **re-coding**. And with this, we can basically achieve a situation where anything we send goes to the recipient. So there are no duplicates in the sense that, whenever I send something to someone, he can use at least a lot of that.

#### Combines with Random Linear Coding
It comes with compromises. It comes with a compromise that then you have to **collect all the pieces before you can get anything useful** out of it. But it's a technique which combines very well with **random linear network coding**. You can try to do something similar with other codes. It's again, adds coding delay. But it can speed up and make more efficient network.

### 5. Overlay Optimizations

**Csaba**: So we have four tools. Or we have more. So we have **overlay optimization**. So you have this graph overlay. And we do it sometimes, lambda graph. And then you can imagine that the data is ping-ponging back and forth through the ocean. And that seems like useless. The question is not that it's actually useless or not for robustness. But you can do **latency-based optimizations**.

#### Cautionary Notes
Again, you have to be careful because you are reorganizing the graph and you are losing some properties. You might have a bigger distance in the graphs or multi-hop and so on and so forth. But we are working on this. **WFR-Gossip** is a gossip protocol we are working on now. So we are working on another end of this.

### 6. Traffic Pattern Specific Optimizations

**Csaba**: That was number five. Number six, I should have put the numbers. Is **traffic pattern specific optimizations**. So we are using GossipSub now for like a generic tool. Just a little bit of this thing and it's not. So we have different use cases:

- When you're sending **blocks**, these are **isolated broadcasts of large messages**
- When you are sending **attestations**, this is **concurrent broadcasts of small messages**

And there is also a question **why are we even broadcasting those**, which was also discussed. And I could continue this. So for each kind of use case, you can **tune the protocol and get better numbers**.

---

## Current Performance Numbers

### Recent Measurements

**Csaba**: So the numbers. And you might have noticed that I was not putting numbers. So I'm sharing two things here:

#### Block Diffusion Today
One is this is from the explorer, from the labs, from Etherscan. Kind of similar to the tests that you were doing in 2020. It's just a much nicer interface and much more modern. So what you see here is **one block**. One block, which was first seen after **1.72 seconds**. So maybe someone was doing delay games, though, whatever. Anyway, let's assume that's when it was sent.

And then you see how it was diffused under different curves, under different continents and nodes at the continent. Anyway, what you see is that after **2.2 seconds**, 90% had that block. And that block happened to be a **65 kilobyte block**. And I think that was also with blobs. And actually seven blobs. And I think that's included in those curves.

So this is fast, right? That's cool. That's positive. It seems we can do much faster.

#### Attestation Propagation
On the right, you see actually from yesterday evening. When I was running my experiment from 2023 on attestation propagation. So this is my node getting all the attestations for a given slot. And this is filtered for those which were missing slots, or slots which were very bad.

So for four seconds, nothing happened. And then at 4.2, things started. And one thing you can notice is that the **good cases were very fast**. Another thing you can notice is that they are kind of **all different**. And this is the problem with these things.

### Variability Challenge

If I would throw four blocks one after another, they would be all different. And it's really hard to say, like this is the mean here. It's really hard to **give guarantees** on these things. Because there are so many variables in the system that it's hard to understand. And actually, even if you're simulating this, it's really hard to get to the point where you can say, I have **10^-6 probability that it won't go above 4 seconds**. Because you can't capture the complexity of the system.

But I mean, the numbers are nice. We have these small numbers.

---

## Part 2: Is Propagation Speed The Problem?

### The Stop-and-Wait Problem

**Csaba**: So let me go to part two of my presentation, which I was calling "Is propagation speed the problem?" And here, I want to play a bit a naive view of why I advocate. And for that, I have to go back a little bit to how I thought the code is working.

#### Basic Protocol Flow
So there's a **sender**, sending a message. There's a **receiver** receiving it. And then it's sending an **acknowledgement**. And then, when the sender is getting the acknowledgement, it's deciding whether it has to re-send, because it was not getting the acknowledgement, but it sends the next message.

This is kind of how a simple network protocol works.

#### Timeout Selection Challenge
But then you realize that you have to **choose this timeout**. And when you're choosing this timeout, there is some **uncertainty** on how fast the message is propagating and how fast the acknowledgement is propagating. So you have to add **slack** to it. And then you have to choose a timeout, which is bigger.

And then you realize that **message size matters**—like bigger the message, bigger the time. You need even bigger timeout.

### Defining Slot Time

**Csaba**: So the question is how we define the slot. When we are speaking of slot time. And here, I'm just kind of taking out this figure and transforming it into a network instead of just a simple communication.

#### Current Slot Architecture
So you have the **builder**. It is sending the block. And then in this communication, which is not to one but to all attesters, we have to have a bunch of slack:
- The **worst case size**
- The **worst case propagation**
- **Multi-hop uncertainty**
- And you can go on

And then these attesters are sending out attestations. And then that's again, with your propagation—you have a bunch of slack there. So the **worst case becomes huge**.

### The Core Problem

**Csaba**: So the real problem is that we have to deal with this **worst case**. Because it's too hard for us to **sacrifice a block**. And this is kind of my thesis here. **We should make this much simpler. We should make it simpler to sacrifice a block in the system**. And then you can scale.

---

## Learning from Networking History

### Stop-and-Wait is From the Stone Age

**Csaba**: And actually, this thing that I've shown to you at the beginning, this we call **stop-and-wait**. And this is from the stone age in networking technology, like from the 60s. And then networking was evolving. And we developed a bunch of techniques.

### Modern Networking Techniques

#### 1. Parallelization
So we know how to **parallelize**. We can send a bunch of messages on the fly. And then we have something which is called **selective acknowledgement**, which is a **bitmap** of which one was acknowledged. And then you see here bitmaps. And then you can think of maybe I can **merge bitmaps** and that helps. So this is something like **delayed execution**.

#### 2. Capacity Exploration
And then in networking, we know how to do **capacity exploration**. So how many messages you send? We know how to scale this up and scale this down. And we do **flow/congestion control**.

#### 3. Timeout Tuning
And then we also do something like timeout tuning. We also know how to play with this. I do **exponential backoff**, which is I'm extending the timeout. And for example, Vitalik has now a **positive exponential backoff**, I think.

#### 4. Forward Error Correction
And then we know how to do **forward error correction**. So we anticipate losses. We do a bit of encoding. And then we don't have to—well, we don't have retransmissions. Or even if we have retransmissions, it doesn't matter, because we are encoding. And we are just going forward fast.

#### 5. Pipelining
And then we know how to **pipeline**. So it's not that we have to always wait. We can just pipeline it a bit.

### The Core Message

**Csaba**: So I don't know how to map these things into blockchain space. I'm just saying that there's **latency, which will not go away** from the system. And in networking, what we do is **we are working around it**. And sometimes I feel that here we are trying to do this worst case thing which doesn't allow us to work around it.

So I see some ideas to work around it. And maybe we just need to work around it. That's all. Thank you.

---

## Q&A Session

### Question 1: Worst Case vs Working Around

**Audience Member 1**: Yeah, maybe just on this last point, you mentioned that we do this worst case thing. And I guess that sounds somewhat political, I guess, between working, being able to work around it or not. Maybe you could say more on this.

**Csaba**: Yeah, so currently **the cost of throwing away a block is quite big** and lots of effort. And then the network is doing lots of effort to make sure that it's actually getting the block. It's so much that then you don't have to throw away.

And that means that you have to tune your delay such a way that you have **with 100% certainty you are not throwing [blocks] away**. In 99.99% you are not throwing either away.

#### Alternative Approach
If you find a way to **make it cheaper to throw it away**, then you can set this threshold to **90%**. Because then you are throwing at 10% and you're fine. And overall, you are going **much faster**.

That's kind of the underlying thinking. The **curve typically has this long tail**. And then so if you want to set it to 99.99%, then you are going 10 seconds. If you want to set it to 90%, then you go 4 seconds.

Which one is the better strategy?

### Question 2: RLC vs Other Coding Techniques

**Audience Member 2**: Which one is the better strategy? RLC or others? I mean, it might not be like one answer, but what is your take? On other ones here. What is RLC? Doing the network level or doing it earlier?

**Csaba**: It's different usage. So **Reed-Solomon codes** are very good for when you have a big thing that you want to send over like the whole thing. So like the block feels like that. But then you should see that maybe I'm actually better at **streaming the stuff**. So chopping it off to smaller pieces and then streaming it. And then Reed-Solomon codes start to have issues. 

Random linear codes seem to have issues. I see that people are using it in streaming contexts. I always have a problem using it in streaming contexts. So it's not in design. So it might not be that good for that. But I think it's a technique. It has issues with multi-hop. But it's something to explore differently.

And then **RLC** we can just use it for, for example, in **DAS**. We can use it to encode the same thing node. So that the sampler takes is just much stronger. It's not taking a cell, but it's taking a **combination of cells**. So when you have to reconstruct, you have a machine on the reconstruction.

### Question 3: Privacy Considerations

**Audience Member 3**: On one of your first slides you had like different aspects of networking—one of them was **privacy**. What do you mean by privacy? Is it like exposing the address of the nodes?

**Csaba**: Yeah, it is. I mean that is—it can mean that. It's always like a scale. So we have **some level of protection** in the system currently—not that high, but we do have something. And sometimes the question is should we throw it away and make the network more efficient. Or maybe we should enforce it.

And then we have—so it's part of the **compromised space** and who we want to protect at the network level.

**Audience Member 3**: What's your opinion on that? Should we target to try to keep as much as far as possible or?

**Csaba**: I don't know because it's so easy to do analysis at the moment. So it's like—it's always those questions. I don't know.

### Question 4: ZK Aggregation and Privacy

**Audience Member 4**: I'm asking because I keep investigating replacing the votes with ZK proofs. I think partially correcting for a part of it because this privacy assumption—if we don't have it then we could optimize propagation well.

**Csaba**: Yeah, so if we solve it at that level we can make a relatively low latency solution. And my take on that is that we need a **broad path** basically. We need a system which has a **smooth slow path** for players and maybe a **fast path** for faster actions.

And maybe the slow path you are doing **ZK aggregation** and fast path you are not doing ZK aggregation. And these have to be kind of good together. But then the trade-off is—and then the problem. So it's fine to see ZK aggregation. Because **ZK aggregation as far as I see is acting at that level**. When you are doing these encodings, then it's always—you are adding delay to the whole process.

So but how you apply it is always like—if you want to be fast like "I want my transaction included in 10 seconds," then ZK aggregation is a bit inefficient.

### Question 5: ZK Proofs for Staker Identity

**Audience Member 5**: So can we do a ZK proof for for example you being a staker and not giving up private information?

**Csaba**: I think you can. And not give up—you give a ZK proof that you are a staker. So you can have a very efficient protocol. But if somebody is actually observing it, when you are generating this, I think. So if I can observe the traffic, I kind of correlate it with the fact that you are staker and then I can be pointed to value because the schedule is somehow public.

So you have to **hide the schedule**. You have to **hide a bunch of things**. It's just very complicated. And it's always comes with **delay and bandwidth overhead**—huge. So you have to generate the proof. You have to add extra traffic. You have to delay everything. It's just not efficient.

But it's cool. It's just not efficient.

### Question 6: Rainbow Staking and Privacy

**Audience Member 6**: I think privacy might be a place where **Rainbow Staking** is particularly ahead-through. For instance, one reason we want privacy is we don't want home operators to be exposed. But if we said privacy is really a light service property, those nodes are participating. And since those are pretty light, we are kind of more invisible to the network. But we have stronger assumptions on those that are going to attest.

I think I like the idea of having **two tracks**—one, which is the fast track that serves certain purposes. And the other one is for more resiliency oriented services.

**Csaba**: So also the user side, you have this—like someone needs speed, sometimes someone needs privacy. So you have to think of this in the context of some kind of contract.

Can the validator role where some entities will say that, OK, **we don't need privacy, but we want the network propagate faster**.

I think the aggregators could be such roles. So they basically **give up privacy**, but they can—everyone instead of just gossiping transactions with each other, they just send the third, not the transaction signature to each other, they send signature directly to aggregators and the aggregation is faster.

**Audience Member 6**: All the other chains are doing it really. They gave up a long time [ago] because it's so hard and so it's kind of easy to do it right. You might want to have these **classes separated**.

**Moderator**: Thank you so much.

**Csaba**: Thank you.

---

*[End of Csaba's presentation]*

*Note: Transcript generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*