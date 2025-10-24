# Lean Day - Opening Keynote

| Item | Details |
| ---- | ------- |
| **Session** |  Opening Keynote - Strategic Overview of Beam Chain |
| **Speaker(s)** | Justin Drake (JD), Ethereum Foundation - Protocol Architecture |
| **Slides** | [link](https://docs.google.com/presentation/d/1VIVV3WEPgJFcXSOiFQidHH0xwrdic4cvd2wTcmo_f-Q/edit?usp=drive_link) |
| **YouTube** | [link](https://youtu.be/CmehdbYr_qM?feature=shared) |

>back to: [Lean Day Overview Page](/workshops-and-interops/2025/lean-day-EthCC/00-index.md)

---

## Welcome and Introduction

**JD**: Okay, welcome, good morning to—good morning and welcome to Beam Day. It's great to see so many people on a Sunday coming to think about the future of Ethereum. And yeah, today we have a really, really packed day, so yeah, let's get started.

What I want to do today is zoom out a little bit. We're going to have many talks that are going to zoom in into specific topics, and I want to kind of share some thoughts on the broader strategy because what we have on our hands is an extremely ambitious project, to say the least. And so we need to have some sort of a plan, and this is, I guess, some of my thinking which is still evolving.

So in the first part of this talk, I'm going to share ideas for strategies that apply for the whole Beam project. And then in the second part, I'm going to talk about specific tactics that we can employ for modules of the Beam chain. And one of the themes in the tactics is to try and use time as leverage, where things kind of age like fine wine.

---

## Part 1: Overall Beam Project Strategy

### Strategy 1: Keep Projecting Out

**JD**: Okay, so the very first strategy is just to keep projecting out. So the mental model that I have here is that we have the status quo on one end of the spectrum—where we are with Ethereum today. And we have the idealized, platonic state of Ethereum where it could be if everything was perfect.

And in some sense, the goal of Ethereum research is to push the state of the art and to try and minimize the delta between what we know is possible and what is actually possible. And one of the realizations in the last few months and years is that maybe this delta here is very, very small. Like more and more we're finding results that are lower bounds. There's proof that there isn't much space to improve things.

And so I have this kind of big dream in some sense to try and get Ethereum to the state of the art. And I believe there's an enormous gap here between where we are and where we could be. And really what the Beam project is all about is kind of this mentality, this attitude that we would start with the state of the art and walk backwards, as opposed to starting with where we are and going forward. So we're trying to minimize path dependence and we're trying to take a fundamental first principles approach to things. And hopefully at some point we kiss in the middle and Ethereum is in a great position.

### Personal Project Delegation

**JD**: Now this is a bit of a personal slide, but I've been working on many different projects in the last couple of years. And what I'm trying to do is spend more and more time on the Beam project. But in order to do that, I need to find ways to delegate these projects here.

And I share this slide kind of tentatively, but there's various people who I'm hoping will be able to champion some of these efforts here. So we have Dankrad for the Data Availability Sampling lab, Luca—this is shared confidentially from EthCC—might be championing native rollup labs, and we also have Farah in this room who might be championing the EF proof project. And so I'm hoping that all of these other projects can be segmented away and I can spend most of my time on the Beam show.

### Strategy 2: Pick the Right Battles

**JD**: Now there's a lot of stuff that we can improve in the consensus layer. And what I think we should do is try and pick the right battles. We need to basically focus on the things where we have the most leverage.

Now this is kind of this mental model that I introduced in my DevCon talk, where there's three buckets of improvements and each bucket has three meaty improvements. So in the block production bucket we have single slot finality, attestor voting, and shorter slots.

And one of the amazing outcomes of the last few months is that we actually have EIPs that move in that direction. So the incrementalists are very much excited about improving the block production. We have a FOCIL EIP, there was a proposal to improve the issuance around that, we might go into the H4, we'll see. There's also EPS which is an incremental step towards APS. That's getting a lot of momentum recently, especially since Berlin. And we also have a proposal to shorten the slot times to six seconds.

So all of that is things that we want and because others are working on it, maybe we don't have to spend that much time on it.

#### What Not to Focus On

**JD**: Another strategic thing that I would not spend much time on is the issuance. The issuance is a very political and sensitive topic. And the nice thing is that it is very encapsulated. All of the complexities and macroeconomic thinking—and then it distills down to just one line of code, which is the issuance curve. And so my suggestion is to punt this to after the Beam fork and not spend any resources on the front end.

And a similar thing can be said with the VDF project, which is trying to improve the randomness. By the way, I brought one of the VDF rigs that we manufactured. And the reason here is that all of the complexity is encapsulated in the hardware. And again, it's one line of code essentially to integrate it into the Beam chain.

#### What to Focus On

**JD**: So what should we focus on? Well, I would argue that the highest priority is actually this moon-math cryptography. Both the quantum security for the attestations and the proposals, as well as the SNARKification of the chain. And the good news is that not only is this the highest priority in my opinion, but it's where we're making the most progress.

And I think we should also focus on the staking, including SSF and Rainbow Staking. And the good news here is that we already have a rich body of literature that have pushed the state of the art, and things like SSF at this point in time are fairly well understood.

### Strategy 3: Beachhead Strategy

**JD**: So what I would do basically is have this strategy where we anchor ourselves in something of very high quality—the post-quantum testnet. We nail that part of it and then we grow from that point onwards, incrementally, which is called the beachhead strategy. So we establish a beachhead with post-quantum and then we move our way up to related topics. And if we can nail this, I think we'll be in a fantastic position to nail the other things. And that's part of the reason why we'll have a round table later today on the post-quantum testnet.

### Strategy 4: Be Pragmatic and Reuse Infrastructure

**JD**: Now, another strategy is, you know, with such an ambitious project is to be pragmatic and reuse infrastructure where possible. And this is a slide that I had in DevCon. There's a lot of infrastructure that we can reuse from libraries to even funding infrastructure.

And I think we also have an opportunity to partner with lots of teams that didn't exist when we were doing the beacon chain. So we didn't have a DevOps team, a security team, incentive team, testing team and networking team. All of that is stuff that didn't exist when we did the beacon chain. So if we were able to ship the beacon chain without it—and also without all of that stuff, all of that basically had to be built—I think we're in a great position to be successful.

### Team Structure and Specialization

**JD**: Now, one question that I get asked a lot is, do we need nine new consensus layer teams in addition to the six existing ones? Do we need 15 teams? And I guess there's two parts to my answer here.

One is that I'm expecting natural consolidation. So, historically, for the beacon chain, we had 10 new teams appearing—those consolidated down to five. I'm expecting something similar here, but also what I will encourage is specialization.

And we're starting to see this. So for example, the Quardrillion team—they are networking experts that create the libp2p C++ library. And I think it makes sense for them to focus on data availability library. Similar story for Kakarot. They are SNARKification experts in the context of Cairo. I think it makes sense for them to focus on just the state transition function and its SNARKification.

And there's also other opportunities as well for teams to specialize. So for example, we need to have the post-quantum equivalent to BLS. Someone needs to write that and that could be a library that is beneficial to all the teams. And there's also some middleware around having multi-client infrastructure for multi-proving and all of that stuff.

### Ethereum Foundation Re-org and Timeline Considerations

**JD**: Now, one of the things that has happened recently is that there's been a massive re-org at the foundation, specifically when it comes to the protocol R&D. And we basically have a new captain, one of which is in this room.

And on June 2nd, they basically made this announcement that we have these new strategic objectives. And what that means is that for the short term, for the next 12 months, we have things that we want to do, which are scaling L1, scaling the blobs and improving UX. And one of the things that you notice here is that the Beam chain doesn't fall into any of those short term priorities.

And so what we need to do is just need to be patient until June 26th. And I think at that point, we can start becoming more aggressive. So for a period of a whole year, I think what we need to do is basically put our heads down and basically do the best work that we can in preparation for the new team.

### Timeline Strategy

**JD**: Now, this is one of my slides from the DevCon talk. I will actually argue that it's my most hated slide ever. Because people complain about how the timelines are so long. But I think it's... I like this slide. I don't think there's nothing wrong with it.

And one of the things that has happened is that the peeling phase, I would argue, has successfully ended. And now we have these 15 independent teams—6 plus 9—that are potentially going to be building clients. And now, we're very much in the second phase, where we're writing specs for different modules. And eventually, we will produce a full feature complete spec.

But going back to the first point, there's going to be this period of time here where we should not disturb the existing devs. Because they have this very important mission around the short term strategic objectives. And so we should time the release of the Beam spec, not too early.

**Speaker 1**: Okay, any questions so far?

---

## Part 2: Tactics for Individual Modules

**JD**: Okay, let me just move on to part two tactics. So here, what I'd like to do is focus on individual work streams within the Beam project. And oftentimes highlight this theme, and I think that time is usually to our advantage. The Beam project spans multiple years—three, four, maybe five years. And the fact that we have so many years is actually a structural advantage, which I want to highlight.

### Poseidon Hash Function Strategy

**JD**: So what is the plan for Poseidon? One of the amazing coincidences is that SHA-256 and Keccak were both eight years old when they were included in Bitcoin and Ethereum. It's kind of a coincidence. But what I'm hoping we can do is we have a similar story for Poseidon. Poseidon is five years old. And hopefully over the next three years, we will really mature it into a very robust and trusted hash function.

And there's like all of these efforts to accelerate this maturation process. So for example, in the first half of 2025 alone, Ethereum Foundation has organized three different cryptanalysis workshops in three different locations, inviting experts all around the world to try and break these hash functions. And we've also given six different grants to teams around the world to think about breaking this hash function over the next six months.

And so I really do think that if nothing happens and no breaks are found, then in three years time, we will be in a great position and Poseidon will have aged like fine wine.

### Post-Quantum Cryptography Progress

**JD**: The post-quantum cryptography is going very well. And one of the features here is that we have a very modular design with different components and different people—different experts basically working on the different modules. And one of the nice things here is that we have designs which are extremely simple and in some sense close to optimal.

And one of the nice things is that when most of the complexity lies, which is the signature aggregation, we're going to benefit from SNARKing over time from the broader industry. So in the short term, we started the process of building a minimal zkVM to SNARKify the attestations recursively. And this zkVM should improve over time as the state of the art of research also improves.

### Quantum Narrative Timing

**JD**: Now one thing that I'm curious to see how it unfolds is the quantum narrative. It seems that every quarter, every six months, the drums get louder and louder and louder as we get quantum computers with more and more qubits and the algorithms improve.

One of the amazing things that happened is that last year, the state of the art for breaking discrete log was requiring 10 million physical qubits. And then earlier this year, that number was brought down to a million physical qubits. And just a few weeks ago, we had a new paper which brought it down to 400,000 qubits.

So we have the quantum computers that are increasing in qubits and then the algorithms improving. And it's possible that this quantum narrative, again, is going to age like fine wine to our advantage.

### SNARKification Benefits

**JD**: SNARKification. The amazing thing here is that all the industry is just doing the work for us. We can just sit back and relax. This is a zkVM tracker from ETH Prague. And what's happening is that every week we get a new tick and a new green box. And so by the end of this year, basically this section of the spreadsheet should be essentially completely green.

And we'll be able to reuse all of that super sophisticated zkVM infrastructure to our advantage without doing any work. And we're going to have a talk this morning that is going to kind of highlight the fact that this is happening in practice. So RISC Zero is going to basically be announcing that they've managed to SNARKify the whole beacon chain as it exists today. And that really bodes very well for SNARKifying the whole Beam chain as well.

### Finality Improvements

**JD**: Now, finality, I'd say, is also going pretty well. So we have a whole team within Ethereum Foundation dedicated to consensus and finality. And what needs to be done? Well, Potuz wrote this minimal spec, which is 112 lines of code. We need to do some last mile integrations and we need to do more replications.

And what I think will happen is that here again time is to our advantage in the sense that a lot of the competitive blockchains have much, much, much faster finality on the order of seconds. And so every year that passes is one where there's more and more desire for Ethereum to catch up with the state of the art.

### Rainbow Staking

**JD**: Rainbow Staking. We're going to have a whole talk on this. But the basic kind of TL;DR of Rainbow Staking is that we take validators, which are very bundled roles, and we unbundle them into attestors, inclusors, proposers, and then aggregators.

And one of the advantages of Rainbow Staking is that for the last three roles, the barrier to entry is extremely low, both technically and financially—you only need one ETH. And for the attestors, the plan here is, this is you have a capped number of attestors because that's limited by the number of signatures that we can aggregate per slot.

And over time, through Nielsen's law, which is the improvement of bandwidth, 50% per year compounded, we'll be able to increase the number of validators that we support, all the way potentially to one million validators. So the idea is to basically aggregate a whole million validators' signatures in a per slot.

### Block Production Strategy

**JD**: Okay, I already mentioned that block production, we have these three EIPs. And basically the plan here is just to make small improvements. If you take EPS, for example, in order to get APS, you just need to add an auction mechanism. Which auction? It could be execution tickets, it could be execution auctions, it could be execution taking this—there's a whole design space, but a lot of the complexity is basically handled by EPS.

And then for FOCIL, what I expect will happen is that the beacon chain will have vanilla FOCIL, and then there's also some improvements that we can add on top—like FOCIL and zk-FOCIL if we deem that to be valuable.

### Issuance Strategy

**JD**: Okay, one of the final slides, issuance. So I already mentioned that issuance is a very delicate topic, but I don't think it should be completely forgotten. And the thinking that I have here is that this trend here is going to be like a pressure cooker. The more negative issuance that we have, the more pressure—for example, more pressure to have LRTs, more pressure to not have ETH allocated to DeFi, things like that.

And I think that's going to ultimately lead to a breaking point, where the community will say yes, now it's clear to us that we have to change the issuance. And the tactic here basically is to do all of the preparation necessary so that when this day happens, it's just a one line change of code in order to implement this issuance change.

**JD**: Okay, that's it. Happy to take questions, and looking forward to the talks coming up.

---

## Q&A Session

### Question 1: Proposer Unbundling

**Audience Member 1**: I didn't quite get the part where a proposer is unbundled on the leader. The proposer only needs to stake one ETH—I'm a bit surprised by that.

**JD**: Yeah, so the way that the unbundling happens for proposers specifically—hey, I'm talking about execution proposers—is APS. So the way APS works is that there's some sort of auction. And so for example, execution tickets. You buy tickets ahead of time, and then there's a sort of a lottery process to determine who will be the proposer for that slot.

And in order to participate in those auctions, the minimum amount of ETH required will be one ETH. And this is just a very basic anti-spam protection. So this is FOCIL, this is APS, and this is aggregation, which is a standard now for the attestors.

### Question 2: Attestor Numbers and Consolidation

**Audience Member 2**: So with the attestors, like you said, it was capped by a low number of signatures, it's about 16K. And yeah, I have 800,000 of those 16K, and if you anticipate we're going to have to massively reduce the number of attestors and kind of gain them back over time, do you see that being a concern?

**JD**: Okay, so the good news is that today we only actually have 800,000 validators. So the reason why I kind of think 16K is a good number is because there's a lot of buffer relative to what we have. The bad news is that we don't have consolidation yet.

So what I'm hoping will happen is that when we introduce post-quantum public keys, the existing validators can specify the same key for all of the validators that they control. So it's going to be basically speeding up consolidation above and beyond what we're going to see with MaxEB.

And yeah, the main worry that we have, basically, is that if a very large entity, for example Coinbase, decides to not consolidate—they want to be like maximally adversarial where they split all of their stake into equal sized validators that are just at the limit—then that could be a denial of service attack and it could push up the minimum amount of stake to be a validator, way beyond 32 ETH to be potentially closer to a thousand.

And so what I'm hoping will happen is that in the short term we can rely a little bit on social dynamics whereby Coinbase are not trying to be adversarial as we know them today. And then over a period of just a few years kind of harden the L1 to support a million validators.

And the reason why it grows by four every time here is because the proposed signal aggregation is a two-step, is a two-layer aggregation. And when bandwidth grows by a factor of two, you actually get a compounding on the two layers. You get a factor of four increasing the total number of signatures that you can aggregate. And so every two years bandwidth doubles. So every two years we should be able to increase by four. And so this is only an eight-year journey. What the only thing that we're doing is increasing, just changing one parameter as internet bandwidth improves.

### Question 3: Aggregators

**Audience Member 3**: Are the aggregators on this slide, do you consider them kind of as part of the attestors or are they just probabilistic members that's not having a stake or just a few of them?

**JD**: Yes. So my mental model for who are the aggregators is that they are a subset of the attestors. And specifically my assumption, which I think is a reasonable assumption, is that 10% of the attestors will have the compute necessary to do the aggregation. So 90% of the attestors can be on Raspberry Pi Picos, or on phones or whatever. And then 10% can be on laptops powerful enough to do the aggregation.

But I think it would be nice and I think that's the natural design to make the aggregation permissionless. So that anyone who's not an attestor can still help with the aggregation.

And the way that it would work in terms of selecting the aggregators, and this is something that's good for the testnet to be here, is that I'm hoping that the default is that you opt in. And so I'm hoping that the way that the clients are built is that when you boot up your client, it tries to aggregate a thousand signatures, for example. And it times how many milliseconds it takes. If it takes less than 100 milliseconds, then you automatically enroll as an aggregator. If it takes more than 100 milliseconds, and your compute is a bit too slow, then we're not going to enroll you as an aggregator. But overall you won't even notice that you're an aggregator, because the CPU load and whatnot will be minimal.

### Question 4: Prover Requirements

**Audience Member 4**: There's been some discussion recently about provers and their requirements, hardware, electricity. I'm curious what's your take on how that will look like in the Beam chain too?

**JD**: Okay, so this is my take. Let me go through this in detail. So in 2022, Succinct, the company behind SP1, founded itself over the whole premise of SNARKifying the beacon chain. And what they were doing basically, they wanted to build bridges or whatever, or L2s, and whatnot.

And what they achieved was using a very, very powerful machine like this GPU workstation with a terabyte of RAM or whatever. They achieved to only SNARKify a single committee, and they have this proof time of 1.5 minutes, but at the cost of most of one full slot. And they have extremely high latency.

Now in 2025, what's happening is that RISC Zero, who built a zkVM, is revisiting this problem that Succinct tried to tackle. And this is one of the talks this morning, so I won't ruin it. But what I'm hoping will happen for the Beam chain is that we're going to have everything will be green here, meaning that the provers can be a single laptop CPU.

So a single MacBook CPU will be able to SNARKify the full chain at every single slot with only one slot of latency, meaning in real time. And so the honesty assumption is extremely low. We just need one guy in the world who has a laptop who's willing to contribute proof, and surely we have enough.

And we can do a similar trick to the attestors, we could say, you know, if your computer is powerful enough to SNARKify the chain, for example, produce proof of a state transition in the last 100 milliseconds, then you automatically get enrolled, and you won't even realize that you're generating these proofs. But most of the validators, 90% are still assumed to be on extremely lightweight devices.

### Question 5: Post-Quantum Uncertainty

**Audience Member 5**: I think this is, I don't want to go into a topic that has been a Pandora's box that's talked about many times. But I am interested in, you still hear a lot of debate on post-quantum about how close quantum really is. And even I saw recently, some people saying that actually there's a lot of ink spilled for no reason, we maybe don't even need these post-quantum cryptography. The quantum computers will have a very hard time breaking these things. So in light of this kind of cloud of uncertainty, when will post-quantum really matter? Like how do you think about navigating that dynamic?

**JD**: Right. So I have the luxury of—so I live in Cambridge, and the father of my son's friend is the founder of one of the largest quantum companies in the world. And, you know, I get to pick his brain. And yes, he does confirm with me that a lot of the headlines are, you know, just fluff. But he also very much acknowledges that there is a lot of real stuff. And he's actually extremely bullish, maybe also because he's biased on the future of quantum.

Now, I guess my take regardless of the technology is that the narrative is going to play to our advantage, whatever happens. Another thought that I have is that it's going to take a very long time for, you know, the validators to move to post-quantum. So we need to plan for this transition period.

The way that I envision doing it is basically, we have a hard fork where we allow the validators to specify a post-quantum key. And then there's a period of time, for example, one year, where they specify the key. And then at the end of the year, whenever the Beam fork activates, only the subset of validators that have migrated over would be involved as validators. And we want to make sure that this number is high enough, 80% or 90%. So we might have to wait even more than one year.

The other thing I'll say going back to the narrative is that we're starting to see it trickle down, for example. Recently, BlackRock had this addition to the ETF prospectus. And this is something I can share, like, you know, the cryptography team, for example, had a call recently with BlackRock, right? Just to explain to them what is the situation in this area.

And even though even if the fundamentals of quantum are still maybe a decade out, or maybe 15 years out, narrative is just going to get louder and louder. And I think we need to, because it's core security, I think the only reasonable approach here is to be maximalist in terms of defense.

### Question 6: Spec Writing Process

**Audience Member 6**: I just wonder if everything is evolving so fast, how do you see the writing specs process? Is it like a big problem?

**JD**: Yeah, great question. Yeah, so the way that I would tackle the spec writing is by dividing and conquering. So I would start with specs for individual modules. We already have a spec with SSF for example by Potuz. We already have a spec for, you know, one of the most complicated pieces, which is the aggregation of signatures by [inaudible]. I want to see just all of the specs kind of written out.

And for module, for example, the post-quantum module being the combination of multiple specs. And then from a timing perspective, what I would do is basically only put the pieces of the puzzle together once—sorry, yeah. Once we're no longer in the "do not disturb the devs" period, because what will happen I expect is that once we make a big announcement saying, hey, we have a fully featured spec that's going to be FOMO, and then, you know, all of the teams will want to be one of the first to implement.

And we don't want that to be disruptive to the short time objective of scaling L1 and improving blobs. So, yeah, it's a little bit of a political and tactical balancing act.

### Question 7: Research vs. Development Balance

**Audience Member 7**: Why do you think you should not disturb the devs as opposed to being able to work on both models for each other?

**JD**: Yes, okay, so the mental model that I have is that it's 10 to 100 times less expensive to do research and to provide feedback than it is to write code. And we kind of saw this historically with the beacon spec, where the researchers would whip out a new spec every month. And then the poor developers, you know, they had spent six months implementing it and now they have to restart from scratch.

And so I think we should have the devs be engaged using this lightweight feedback mechanism, but we should not have them write a single line of code. All of the code that they write should be on the current beacon chain.

### Question 8: CPU vs. GPU for Proving

**Audience Member 8**: You're slightly about going to a laptop CPU twice as CPU, where can you just do a GPU or a laptop? Because if a CPU is possible, why not do a CPU?

**JD**: My back of the envelope suggests that CPU is fine. So, a single CPU today can do two million Poseidon hashes per second. The Beam chain state transition function is essentially just signature verification and it's way, way less than two million. So, like, the ideal outcome is actually maybe like 10% of a CPU should be enough to SNARKify the whole state transition.

**JD**: Okay, thank you so much for the questions. Next up we have Dmitri from the Ethereum Foundation.

---

*[End of JD's presentation]*

*Note: Transcript generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*