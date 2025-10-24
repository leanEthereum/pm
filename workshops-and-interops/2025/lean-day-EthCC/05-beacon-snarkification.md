# Lean Day - Beacon SNARKifications

| Item | Details |
| ---- | ------- |
| **Session** |  RISC Zero - Beacon SNARKification and ZKasper |
| **Speaker(s)** | Jacob Everly (JE), Boundless - Product Lead |
| **Slides** | [link](https://docs.google.com/presentation/d/17hhTSaTUrXUR-mWGm2qAT6sm365MVHEI8OyG2PQJsnQ/edit?usp=sharing) |
| **YouTube** | [link](https://youtu.be/_9JM83Comv4?feature=shared) |

>back to: [Lean Day Overview Page](/workshops-and-interops/2025/lean-day-EthCC/00-index.md)

---

## RISC Zero Signal - ZKasper and Ethereum Light Clients

### Disclaimer and Introduction

**JE**: So quick disclaimer, this is still private, so I appreciate it—no one takes a picture and tweets about it. But it'll all be very public, honestly midweek, and then we're actually going to start doing this in two weeks. I don't know how Drake keeps on inviting me and I keep on telling private things. I did this back in November with the balance general.

Who am I? I'm Jacob. I'm the product lead for Boundless. I work at RISC Zero. Boundless is sort of the evolution that we see RISC Zero heading. Boundless is this—the big takeaway, I'm not going to go into details, even though there's a lot of cool things. You can think of the RISC Zero zkVM as a ZK computer, right? It's literally a ZK circuit that looks like a CPU. This is ZK AWS, so you should be able to show up with any kind of proof and just get your proof. So on-demand compute for your ZK applications.

Actually we live on Base and everything, it's fully decentralized, permissionless, anything that can come and prove takes like 30 minutes to set up a prover for 100 GPUs. Just pretty cool. We live on Base. It's time to stress test the market, right? We have a market. We need to test these dynamics within a market with real money. You cannot have someone show up to a website, click a button on screen to send test transactions. That's not going to work for us.

So we started asking ourselves, how can we push this system and solve an open problem and use all of this energy that our community has and point it to a public good. And that's what we're going with Signal. Additionally, if you see a lot of builders on your timeline, that's us.

---

## The Problem: Ethereum Light Client Trust

**JE**: And this is the problem we wanted to attack that all of y'all are going to be very familiar with, right? We wanted to talk to Ethereum and have it in a trusted way unless you're running a full node. This has caused a historical reliance on either trusting an RPC provider, trusting a multi-sig, or a set of oracles to relay this information to you if you're not running a full node.

There have been some really great attempts at this. So you have Helios, which you know is like an execution consensus client hybrid thing. And it's not just that—it validates that the sync committee is operating properly. Same with Polyhedra has tried this as well. And I think Drake mentioned it was a sync committee one a few years ago. But these have weak economic securities.

I think if you want to talk about Altair light clients, James Prestwich has done a bunch of work around that. But we're on this like insane trajectory of ZK. I think you would be surprised how our internal Slack reacts to performance. Like sometimes we get a number that can't be true. We have to go rerun it and whatnot.

---

## RISC Zero Performance Improvements

**JE**: So recently we released V2 of the RISC Zero ZKVM. It's like a 5x improvement. And additionally we're about to release V3 and the new orchestration system that we should get a really nice bump. The Boundless market comes with a really nice bump in cost and whatnot.

We're moving from charging people from on-demand AWS cost to on-prem people where like governments are subsidizing energy, so it makes it really inexpensive or cheap. And then also we have this new thing called **Proof of Useful Work** where we can sort of now understand how much work a prover has actually done to generate a proof and retroactively give them an incentive to bootstrap this market.

And then lastly, this is probably the most important one—with doing this full SNARKification is that in Petra, EIP-7549 reduced the cost of validating attestations by 100x for us. So we were doing this a few months ago. No chance. One of our engineers like "hey this EIP got included in Petra" and we clicked a button, ran the programs. Oh my God, it works now. Which is wild. So if you worked on that, thank you.

---

## Our Solution: ZKasper

**JE**: And here's our answer. So we've developed what we call a **ZKasper** where we generate a full validity proof of the Casper finality gadget, verifying that a checkpoint has been finalized by the validator set. And what is sort of the result is we get this verifiable...

[*Technical interruption with ChatGPT*]

I can use ChatGPT to answer that. You can enable ChatGPT on your Mac. And that's what I get for Apple not being very good at AI.

So the result of this is that we get this **verifiable block hash** that can be used to execute queries against Ethereum's most recent finalized state. So the TL;DR here is we're sticking the Casper finality gadget along with a bunch of edge cases that I'm sure the consensus teams are aware of here inside this zkVM. It's actually not that bad.

### Performance Metrics

So when we generate this proof, it's about **40 billion RISC-V cycles**. So this is like a per-epoch packet thing. In comparison, a EVM block—pictures are fine. Yeah, yeah, yeah. Which is, you know, in comparison to ETH block is not that bad. ETH block is like 300 to 500 million RISC-V cycles. I know there's some people that are also doing like a beacon STF in about 5 billion cycles. Really not bad.

### Latency and Timing

What does latency look like here? Of course we have to wait for a block to be finalized, which is 12 minutes. And then it's only **6 to 8 minutes** to generate the proof. These numbers I'm showing here are very conservative. These are on our old V2 orchestration system. So these are sort of similar numbers what we were quoting for real-time proving.

But maybe y'all have seen Jeremy talk about how we can keep up with the execution with much less machines. Essentially we found a way to stick three proving agents on a single H100 because we don't take up that much memory when it's perfect. So you can see that number from 14 H100s go down to only 18—16, 15 H100s with their new orchestration stuff. And then V3 again should be another bump.

### Pricing

So pricing is **$80** is a very conservative number here just because it's hard to price things. But you know, less than $80 per epoch on on-demand AWS pricing. You know, we go back to using the Boundless market which is where we'll be generating these proofs. We currently use an instance on AWS that's like $1.80. I'm getting quotes from like GPU providers like 20 cents for the same GPU. I don't know how they're getting this cost but I'll gladly take them and they're still making money on me. So there has to be a 30-40% margin there, so it's even cheaper.

### Performance Bottlenecks

And then the bottlenecks—you see I couldn't pull up a profile or proof and I couldn't turn it on my laptop because it takes too long to execute. But about **30% of the proving time is BLS** and then about **3% is SHA-256**. And then just a reminder: as we move to like more hash-based signature schemes, BLS is much more expensive than hashing. I don't know a number off the top of my head but it's at least a magnitude more expensive. Yeah.

---

## Steel Library and State Queries

**JE**: And then one really cool thing about this is we have this library called **Steel** and now that we have this like, you know, sort of checkpoint block hash, we sort of like can run Solidity inside the zkVM now and check this block hash and then compute against that state.

And the cool thing with Steel is that you can actually go backwards. So if you use that checkpoint state you could sort of query what's happened in an epoch. You can do a range of blocks all the way back to Deneb and you can actually query for events as well.

---

## Signal Genesis - Beyond Ethereum

**JE**: And it doesn't stop at Ethereum. We're calling this the **Signal Genesis** because we want to prove more things and we think with Proof of Useful Work and the Boundless market we can just start to do so. I think something you're going to see in a few weeks—we're going to be doing **ZKasper**. So why not just turn some rollups into ZK rollups? You can sort of do this without their permission, which is interesting, as long as you assume the sequencer doesn't rug you.

And this really starts to deliver on some of the interop we're seeing. And what I really like about this approach versus maybe some standards that are going out or like, you know, you take something like the Agglayer and we have to like opt into their standard. Is this just retrofits what already exists out there? And it's possible to do. And it's not that expensive.

### ZK Rollup Conversion Costs

This is a number for like ZK-Stack that could turn Base into a ZK rollup. This wasn't much. So I assume the number is only lower. You know, this is sort of an annoying number. But right now, per block for Base we're at like **one cent**, which is not bad at all. Yeah.

---

## Trustless Interoperability Vision

**JE**: And then we really think about like—this is a lot and I'm not going to go into the details but sort of the explaining idea here is, you know, as we start to prove more and more chains like Ethereum, you now enable **trustless outbound messages**. You know, Ethereum you can sort of think of as like gateways in the internet.

And let's say we start to like prove Base or our customer—hoping we can prove just Base and you know Ethereum on one. Now if you post the block hash on Ethereum, any other L2 can trustlessly just query what's happening on Base because of, you know, a verifiable proof, which is really interesting. You no longer have to, you know, pay for these multi-sigs and their messaging protocols to charge you a fee. You're literally just going to query a block hash that's already existing on the chain you want to operate on.

We do think this is like a really, really strong foundation for future interoperability.

---

## Use Cases and Benefits

**JE**: Some of the use cases that we're already seeing come out of this are quite interesting. One, it costs less, right? Like if you go to Wormhole or LayerZero, right, these like multi-sigs are charging you money. We're going to run this as a public good and we're actually already talking to the bridge providers to integrate it instead of, you know, using their reliance on multi-sigs.

I think another really cool one—I am personally a fan of the idea that there will be a lot of L2s, not just like the three or four that are active today, but you know, maybe a thousand. And one big problem with launching L2s nowadays is like, okay, DA—are you covered? Cheap? Yes, we should have more blobs. You know, I can go to Conduit or someone, pay them like a thousand dollars a month to spin a rollup stack for me. It's not very hard.

But the integrations are really, really expensive. So I want CCTP, which is a really key tool to bring larger amounts of liquidity. I'm going to need to pay Circle two million dollars. If I want to integrate Chainlink, rumors are it's like 250 to 500K. LayerZero can be around that amount. Most of them say now, if I can trustlessly query Ethereum state, I now gain access to all of those integrations.

So I can just, you know, query this curve and I'm bringing Chainlink to my chain. I'm bringing, into a CCTP wrapper, which required a little bit more. I'm bringing CCTP. I'm bringing Lido. I'm bringing ENS. And we're just reducing those barriers of entry to L2s even further.

---

## Execution Proofs and Civil Resistance

**JE**: One little bonus thing we're going to do as well is we're going to pair these **consensus proofs with execution proofs**. So not only can you validate that this is the state that the validator set has accepted, but you can also validate, you know, that they're acting honestly. And we're actually going to have this running on a website in a few weeks where you can just show up, click a button, and you're now verifying and, you know, sort of contributing to the civil resistance of the network that the validator set is acting honestly without running a full node.

So if you're really, you know, security-conscious user of like MetaMask, before you know, sending a transaction to the chain, make sure that they're not, you know, screwing you. It's pretty cool.

You know, again, why does it matter? Let's reduce cost, let's reduce trust, and then initially I want to always want to be technically innovating on the sort of interop story. And then, you know, what we're going to commit to here is it's going to be fully working for you when you use it. We're going to maintain it, run it, make sure it's audited, make sure it's, you know, open to the rest of the spec. And we're also going to do quite a bit of subsidization. You can think of that, you know, Proof of Useful Work as an incentive to bootstrap, you know, adoption and demand. And this is going to be the first thing we're going to throw at it.

---

## Key Takeaways and Future Outlook

**JE**: Just two more like key takeaways, you know, going on this journey, I guess I'm going to move on. **Ethereum by far the most ZK-friendly chain**, so thank you for that. We've looked at other chains like Solana—not really possible. They need to change a lot of things at the protocol level.

Last thing that I think, you know, the word's starting to get around here is this **speed improvement on zkVMs**. There's just so many great teams working on it. An example is like Airship—if you guys saw the announcement last week, bonkers numbers. We're going to learn lessons from there and really just there's so many great people. I would expect another **10x improvement in software by the end of the year**. Between knowing things we never could do...

And the last thing is you can sort of just do things. We're going to take this full like Casper finality thing from **idea to production in less than three months**, which is pretty cool. That is a new thing to all of the consensus clients. We have taken a lot of code from some consensus clients and also some of the guarantees we've gotten around the formal verification work has been really fantastic. Yeah.

And then we're really excited. I know the EF is going to help us working with a lot of other people and a bunch of L2s and wallet libraries are really excited about this as well, which we're excited to work with.

And yeah, that's it.

---

## Q&A Session

### Question 1: Execution and Consensus Proof Integration

**Audience Member 1**: I have a question about the diagram that you mentioned and can you explain how it worked? Why do you want to go to website? You want to try to put in the execution and the consensus to be sure that the system is honest?

**JE**: Yeah, so I believe Tarun was on a podcast a month ago talking about this and I wasn't really aware, but the idea is that the validator set could cooperate together and fork an invalid chain and steal funds or whatnot without you knowing if you're just checking like attestation sets of proofs. So they can do the invalid state transition function or whatnot. So this way you at least check both and then are aware of their honesty.

**JE**: Any other questions?

**Audience**: Sweet, thank you guys.

**JE**: Thank you.

---

*[End of JE's presentation]*

*Note: Transcript generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*