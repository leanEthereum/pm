# Lean Day - Shorter Slots Roundtable Discussion

| Item | Details |
| ---- | ------- |
| **Session** |  Shorter Slots Roundtable Discussion |
| **Key Contributors** | Barnabé (EF), Sean (Lighthouse/Sigma Prime), Justin (EF) and various community members |
| **Slides** | no slides |
| **YouTube** | [link](https://youtu.be/G3AlC_F3yQk?feature=shared) |

>back to: [Lean Day Overview Page](/workshops-and-interops/2025/lean-day-EthCC/00-index.md)

---

## Shorter Slots Roundtable Discussion

### Opening and Context Setting

**Justin**: Thank you so much. Congratulations to everyone who made it so far. It's been a very long day and great to see the room being very full. Our very last event is a second roundtable where we have Sean and Vanne. I have a call about everything. Sean, do you want to kick us off?

---

## Current State and Low-Hanging Fruit Approach

### Simple Slot Time Reduction Strategy

**Sean (Lighthouse/Sigma Prime)**: Sure. Hello. I'm Sean and we're going to Lighthouse at Sigma Prime. I have a thing about shorter slot times in the context of what can it be done in and what can it be done in the short term. Then I'll say what is optimal. I said we can discuss short term, what can we do, sort of low hanging fruits for it here today. Then what is optimal, be in context, then what changes in the beam context.

I don't mind starting this off. I think today we have **12 seconds slots** and we have so much understanding of when messages are propagating and how long they take. We have a sense that there is slack in parts of the slot right now.

#### Proportional Scaling Approach

We can take a very simple approach and just **change a single constant, which is 12 seconds slot time**. We can keep the rest of the slot just like proportionally the same, as far as we're going to propose your sending messages. I think in there we can get to maybe **eight seconds, six seconds** relatively quickly, like maybe in the next hard fork.

I think that's maybe something that is a low hanging fruit and something that's easy to coordinate around just because it's conceptually simple.

### The Research Perspective: Delta-Based Analysis

On the other side of the argument is changing the slot time in this way, too invasive to do when we can maybe lay the groundwork for more optimally adjusting how a slot structured.

#### Current Slot Structure Analysis

So the way I think I see the slot from one of the research articles is that there is basically **a delta to the latency** and in a slot, current slot we have **three delta**. So if basically you assume that away, the nodes will process everything faster, generate faster and do aggregation process.

So you have basically three delta and that for each delta in Gossip's up you have about **six hops** right now, which could be maybe **600, 700 milliseconds**. And if it was three times that, so basically you have about **2.5 seconds slot**, you can essentially have that.

#### Impact of Proof Segregation

But if you add **proof segregation** in non-entrine that becomes **four delta**, so you can probably do a **three seconds slot** as well. But if we can actually drop the number of hops, for example, that you know the strategy way you can form a grid and say that again, **two hops** you can distribute the signatures to everyone. So basically when that further drop it down, but question is, yeah, so how do we do that?

---

## Delayed Execution and Slot Restructuring

### EPS and Message Overlap Strategy

**Sean**: So I think an interesting thing there is the number of delta, like we can reduce number of delta. And there's been a lot of talk lately about **EPS and delayed execution**. I think the idea there is to maybe overlap to the delta where we have the message that you want to attach to is central and first, which is the **block header**.

And then you can send out attestations for that, unless you start seeing out attestations, then publish the **full block, blogs**, and in parallel attestations can be aggregated.

#### Integration with ZK Proofs

So is that like a type of slot structure that makes sense? Is that like more or closer to what is potentially optimal? And then additionally, like it seems like it might fit in well with **ZK proofs**, where you send around the header while you're generating ZK proof. And then this overlap in time between the votes, aggregation, rebroadcast can also be done parallel like the proof generation's broadcast.

### The Prioritization Debate

**Sean**: And maybe to ground the discussion with one of the proposals, I feel like everybody wants both to restructure the slot because we need like this delayed execution for proving and other things. Everybody also wants to shorten the slot time.

I think at the moment there's a debate around **do we shorten the slot time first and then we're restructuring, after we do the restructuring first and then we shorten the slot time**.

#### Timing Budget Concerns

So one question I have, I am a problem of shorten even a slot time first because I believe the effects are very, very important and getting a sooner is very good. At least I could be a huge win. But the question that's often come up is let's say we don't restructure the slot first.

And now we have that recurrent structure also proposing a testing aggregating. The proposal that we have is to do **three second proposing, one point five second, a testing, one point five second aggregating** is that to a point.

Like we don't currently have firm numbers and looking at childless life looks a bit optimistic to do **one point five second, a testing**. And so I'm wondering if you know, yeah, better numbers or if you would propose a different partitioning of the slot. As you mean we want to have this **six second track target**.

---

## Technical Feasibility and Trade-offs

### Realistic Timing Budgets

**Sean**: That sounds like a reasonable formula to me like **three one point five one point five**. But I don't know how to talk this. I think perhaps with some more of the **P2P low-end toolbox**. It's achievable. It seems like the simplest low-hanging thing that would be like, it feels like it's about four seconds that you just like take out a slot. It still has the same proportions. So I might be the easiest thing like starboard.

### Delayed Execution Complexities

So once we have the execution that we don't need to allow extra time to block well-engaged. But on **delay execution** like there's block building time is usually in competition with execution time. Like you have essentially like a lot of time for propagating. So you can have like **more blobs** for instance.

#### Builder vs. Executor Time Allocation

During that time you could execute if you were receiving like the execution payload there. There's arguments for why leaders might want to send the execution payload still as late as possible within that time. But after all this propagation and potential execution time then you have execution time until the end of the slot which needs to be both for the most to execute so that they can attest their view or propose the next block.

And also for the **builders to build the next block**. I believe you are externally. If you do externally it may be even worse because when the builders have to build the next block and also participating in the auction. So that's like these extra trips.

#### Proving Time Considerations

But yes, essentially these are what these delay execution proposals do. And that's also where you would have a **six-track proving time** for the for the proved because once they get the execution payload as a output they can immediately start proving if they were separated from the builders.

But as I understand there's also like people making the argument that **proves and builders shouldn't be separated** so that they can start proving as early as locally they have the block and that might be the best combination. So even if we were to like try to separate them in protocol like the economics might be towards just integrating because having the input locally allows you to just professionalize but yes that's the general discussion everything.

---

## APS Priority and Timing Games Discussion

### APS Implementation Timeline

**Audience Member 1**: But it is incorrect that from your perspective you think **APS has been to prioritize in the short term**. That's question number one. And question number two. Do you remember this tape that the more we short on the slot times the more we short on the reduce the slot to penalty show the more **timing gains becoming more important** and therefore the more APS becomes important.

And so if we're thinking about this end game kind of salt duration which is actually assume that APS is there because the timing gains will become really prevalent.

#### APS vs. Shorter Slots Trade-off

**Sean**: Yeah on APS I think once you start thinking that shortening the slot time is a very strong objective you start looking at things that stand in the way of doing that as potentially that and **APS means that you need an extra round** of things we have like a become proposal and an execution proposal. So it means like you're essentially **doubling the distance between blocks**.

Oh you say no. Do we have overhead APS as well? In APS a little different because it's the **builder role that we are taking out** and it's not the proposal and the input actually makes quite a bit of a difference in the overhead of coming to consensus on one versus the other.

Like we would want to have a **full round of consensus on the output** of the proposal in APS which we don't really have on the builder in APS like we just had the PTC that's doing a availability committee essentially.

### Pre-confirmations and Slot Duration Relationship

**Sean**: Right. Yeah that's an interesting question. I think we would still need to have... I'm not sure. Yeah I want to think about it. But another reason that maybe it was deeper, it is feeling that one of the main consumers of APS would be **pre-cons** and feeling again that short of slot times address the need for more pre-cons in the system and so that maybe a little less load bearing.

#### The Competitive Landscape Argument

So one pre-cons the story that I'm telling is that if we reduce the slot durations in a very decentralized system like Ethereum **we're never going to get less than one second**. And one second is just very very long compared to **Solana** for example. And so if we're going to always lose unless we embrace the **free animations**. And so I almost had to say let's go all the other information so that's the only route that is coming in as to where.

**Audience Member 2**: Sorry it's different. Back in front of me.

**Sean**: Yeah I think that's an argument. I still feel like you can have **pre-confirmation systems without having APS** in city like APS just removes the roll out and like specifies it in protocol. But I think it's possible like if we are willing to eat the extra possible centralization pressure that we just keep the roll bundled with the validating work and that might be possible where we're doing that.

And so that's like we move some latency of having like **two different parties in the same look**. And then there's a question around timing games.

---

## Timing Games: Problem or Feature?

### Reconsidering Timing Games

**Sean**: I also like revisiting a bit the thinking around this. I do think it can be a problem to let the consensus. But I also feel like there are **interesting impacts of timing games** not to glorify them.

The inverse leader ball that we have like they celebrate people who play timing games very great. And the reason is because it **shortens the transaction latency** between the sender and the inclusion of the confirmation.

#### Benefits of Strategic Timing

Like when I send my transaction within the slot it can go immediately from like the beader to actually being confirmed on chain. Like that's one thing that they find improving. But what does it mean? It's all about **delay inclusion** and for the very last minute you have more transaction.

But if everybody is delaying by like three seconds. I mean that whenever my transaction comes into that interval it's it's it's just shift independent almost. And what timing games do is they force the block proposes to **reduce as much as possible the latency** between their block being produced and their block being communicated to the network.

#### Latency Optimization Effects

Because the longer that latency is the more and we leave a lose during that interval. It doesn't really actually matter where the line is. What matters is just like the **latency between the block being ready and the block being like sent to the network** and confirm.

So I don't think we should go to that extreme. But at the same time. The complete thing convinced that timing games will necessarily like they were into the consensus being completely broken on any reasonable timescale. So yeah, I don't know that we should be like so we'll go worried about them.

---

## Practical Questions and Implementation Details

### Seven-Second Slots Alternative

**Audience Member 3**: Have a bunch of grab bag of silly comments. One is would you consider **seven second slots** because it sounded like six might be a little slow. Eight everybody knows it's going to work. But if you look at the one there it was like 1.9 was where the 1.5 still works. So it looks like you could probably just do seven easily. And I just like not as nice or a number. It's odd. But anyway, it's just worth worth mentioning.

### Community Appetite for Simple Reduction

**Audience Member 3**: And personally, another question I would have to the whole room is **does anybody so far haven't heard a lot of appetite** of thinking that restructuring the block ahead of time is important. It sounds like if you can reduce four or five six seconds just naively by just reducing it. Does anybody here think that that's a bad idea?

**Audience Member 4**: That's not the bad idea.

#### The Bus Analogy for Timing Games

**Audience Member 3**: And then finally, for this thing you mentioned, it's like a with the timing games. It's like the **difference between the bus leaving and when the doors of the bus close**. So the bus always leaves at the same time. But the timing games make it so the doors of the bus close very very close to when the bus leaves. And so more people can get in more often right at the end. That's my problem.

**Audience Member 5**: What do you mean restructuring the book?

**Audience Member 3**: The restructuring the EBA's execution. BAA is like changing how the like instead of the bus. BAA, like how when different parts of block production like cycle happen. I can leave you like that.

### Implementation Readiness Assessment

**Audience Member 6**: Maybe it's the same answer to both your first and your second question. You wouldn't do seven seconds and everyone is happy or why we should think about restructuring. I think at the moment there's just been **a lot of work on restructuring**. Like it's been talked about for a very long time. So there's a lot of code that's already ready.

I don't think people are like blocking on six seconds being like, oh, this is pushing it too far. Actually people are fairly so far like in my interactions have been like **fairly eager to explore** where it was feasible.

#### Missing Implementation Work

The problem is just that the work of moving to the short hours, a lot of times, whoever like **10 or 8 or 6 just hasn't been done**. So there's a lot of code that's still missing. It's not impossible to deliver it by the time certain. Like if we are committing to it, but **there's a lot of momentum behind the slot restructuring proposals**. And so that's also an argument for going for the option that seems more ready available, I think.

---

## The Complexity of "Simple" Changes

### Challenging the "Simple" Narrative

**Vanne**: So yeah, some video about 10 or 8 or 6 in New Bien at least. I also like comment explicitly about the quote unquote as far as the, I forget which at her you used, I apologize, but as far as the quote **just or simply** or something.

Right. So I must say this is a bad thing, but I think that I am for a reason slots not going to be clear. Like I guess, you know, as described by the people here. What I want to say is that **it is not clear that it is a simpler option overall** in terms of implementation.

#### Variable vs. Shorter Distinction

And the general idea is, and I want to kind of rearrange this. And this is the front you're painting, try to push, but it doesn't look at double the slot times. That would be approximately as far as having slot times. And that is the key thing here. **It is the hard thing is not shorter. The hard thing is variable**.

When people say shorter slot times, they're way to be, this is **variable slot times**. And that is what note, I mean, right now a lot of clients work in, the no sense, they work in minimal, free set, and that's fine. Five seconds, six seconds. They're what we know works. But instead of areas that test nest, that nest have run faster for perk nests.

#### Global Change Implications

Right. So, what it is a much **less global change actually**, whether it's 786 or 7732, some of the other proposals. Virtually all of them are in a lot of way actually a **less global scheme change than are simply reducing slot time**. And one can get into more details, but that's not just.

### Learning from Previous Experience

**Vanne**: So. Totally fair. And I would say on this, that's good. Question. The work of Jenning and slot time we have to be done, even if we can set that seven, seven, seven, three, two. I know I agree with you. But I'm taking terms of it. It's one of the analogies called the various, sorry, in various places.

#### The YIP 7549 Lesson

But anyway, there was a now slightly infamous, **YIP 7549** in the previous cycle. That was, it works really simple to people. And it's now, because of that experience, we all now are, you know, walked for these things.

But, but the point is that it, the idea is it's, it has that **deceptive quality** to it. And it's not that bad. It's not that won't have to be done. It's that in terms of the, it is not nearly the obvious prioritization to get the end result we want. Do that first, because that's the easy, simple thing. And then we do the like tricky little thing of, or the tricky thing of like, we stop slot restructuring.

#### Implementation Reality

That there is actually quite a bit easier to have the **slot restructuring in certain ways** across the, like, from one work to another to just say, well, now we have a new slot within slot move, because it only affects that, as opposed to like, a global view of the chain.

---

## User Experience and Technical Benefits

### Why Shorter Slots Matter

**Barnabé**: And maybe something that is a bit missing in the discussion as well is **why we want to photos**, so perhaps I can take two minutes and play you through laptop too. And finally, proposal that we made for Lapsodam, because I feel beyond like the implementation of these days, that anyways we're going to have to pay at some point. **It's really a prioritization problem**.

And I think that can only be solved by asking ourselves **what do we need the most, not today**, because that's not like Lapsodam ships. But hopefully nine months from now.

#### The Confirmation Engine Improvement

So there's a small bucket list of things here. Yeah, to my, my main catchy phrase is to say, **shorter slot times may be if you're confirmation engine better**. And that has just flow on effects on many different parts of the protocol.

### User Experience Benefits

**Barnabé**: So the first one that I think is very important is just **UX**, so your transaction confirms faster, like you get some kind of tick faster. Basket art with 12 seconds slot times. On average, you get confirmation in six seconds.

#### The 2-4 Second Rule

I've gone recently from someone at Coinbase, but **users think there is a problem with a transaction**, or with whatever they're doing, like on digital interfaces, whenever the system is unresponsive for **more than two to four seconds**.

And so if we were to have a slot time to six seconds, the average confirmation would be **three seconds**, which puts you like exactly in the sweet spot of the user not crossing that chasm of feeling like something went wrong. And I need to take action. As the first one.

### Censorship Resistance Improvements

**Barnabé**: Second one is **better central resistance**, like you have **two times as many broke producer per unit of time**. So you have just more opportunities for your transaction to come out chain. I think this is not negligible at all.

### Application-Level Benefits

**Barnabé**: Another argument is about **apps getting just fresher data**, like more of them, their data is refreshed. So for instance, **DEXes**, they are quoting better prices most of the time, which means that they're losing less value to the outside that has more up-to-date prices, which means that more value stays within the decks, which means that you have more liquidity providers, which means that the spread data for users. So you have **lower fees on these apps**.

And you can make similar arguments for any app that relies on some kind of **Oracle to bring information on chain**.

### System Architecture Benefits

**Barnabé**: You also make the **units of work that happen like every 12 seconds, smaller**, for instance, block building options or networking load, which means that typically it's better for a system to have **smaller, more frequent things happen, that bigger, more frequent things**. That's, I think, easy to understand.

### Interoperability Protocol Benefits

**Barnabé**: Another big consumer of shorter slots is **interoperability protocols**, like a lot of them rely on just, if you're producing blocks to confirm things or to settle things that happen like on different domains. And we've told us a lot that **shorter slots can be very meaningful**.

---

## Implementation Timeline and Coordination

### The Development Reality Check

**Barnabé**: Yeah, so I think I hear the day when we say there's been a lot of work on the restructuring and at this point we should just ship it because we are sick of hearing about it. And I'm also sick of it, so I would really, really want us to have like restructured things.

But I actually think this is even like **going to six seconds would be very meaningful** in the shorter slots, which is blanks. And I was more on the camera of actually saying, let's wait until we have a **shiny new consensus mechanism** that perhaps like beam chain will ship to then deliver these shorter slots, just delay that until then.

#### The Urgency Argument

But I actually think like from everything I hear and from yeah, just all of these arguments that I've tried to list here, but it would be like **such a meaningful thing to do in labs to them**. So yeah, it was my like two minute propaganda pitch for four year dates out of schedule.

### Planning and Decision Making

**Barnabé**: Yeah, but I think this makes it very valuable to me and to have these discussions and to accelerate like the **work that goes into figuring out like how much work it is for clients** to change this because I had realized that it's a lot of work. It's not a one nine change for sure to also understand for each of the restructuring proposals how they fail with slot time to be put us in let's say corner solutions that then get difficult to come out of like it doesn't seem to be your case.

But I think it's meaningful to **think about them together** and third also yeah for planning purposes like we we should at some point **decide what goes into glum sediment commit** because everybody wants short folks that come earlier rather than later. And so yeah, having these discussions now is just like very important.

---

## System Resource and Performance Concerns

### Client Resource Management

**Audience Member 7**: So what I think is that the shoulders of time the more you have **different type of activities in the client over that** and the more you have these overlap the more you have **resource management programs** at the level CPU disk memory whatever garbage collection. All these kind of things are that we compromise which will be still fine for example if you are not doing something else as well that might start to create issues.

This is also very **language dependent and very open system dependent** so you start to be in tell it all these well like some languages suffer more than others and some of the things systems are more adapt than others. I don't know if this is bumbley I'm just saying **shortening slot times has this kind of effect** of more effort needs to be done in the program in this was management so that why it is which is just a lot of support for the human Jesus.

### Geographic Distribution Challenges

**Audience Member 8**: Yeah, that's it. Also, I think related to Saba I'm part of some of the home state communities and I think there's there's need to be issues with **Australian notes** where they tend to get like some inclusion legacy on their test station even with 12 seconds slot so I don't know if like shorter slot time might actually impact those. Yeah, Australian notes specifically.

#### Network Latency Reality

**Audience Member 9**: Maybe from Australia. So yeah, so similar products are starting and we run notes in Australia and there is like a **quantifiable difference like monetary difference**. I don't know the numbers off top of my head though. I imagine short slot times would make it worse like exacerbated but I don't know to what degree. I don't know the community.

**Audience Member 10**: Yeah, exactly. There were some numbers from from from **Tony** right just the other day and he also measured Australia and central Europe like you had some comments on it maybe like it wasn't I'm sure somebody discussed it with him on Twitter but.

Yeah, so Tony made a comparison and there was like a significant it was it took much longer not just in Australia but generally like we won't really expect it to be around **90% through**.

### Optimization Opportunities

**Audience Member 11**: Yeah, I think we need a lot more of these analysis and like yeah, besides one and then. For instance, Shabbat was presenting like **optimization** probably get a bit of a hopefully we don't think we can do before we can. So that even if we are like at 90% confidence but we feel like we can get to 100% by the time we get them. It's my like a hope.

Right now there is the **birth net and blood net** right and it would be cool to utilize those or maybe another one to see also the.

---

## Gas Limit and Network Infrastructure

### Gas Limit Scaling Questions

**Audience Member 12**: One question there is also like how we want to ratio the **gas limit** like whether we just want to keep the same gas per time or we will do the scale as well.

**Audience Member 13**: Yeah, so in my view it is like super like method because I will keep them the same. Yeah, that's that's. Yeah, that's not like that.

### Starlink and Connectivity Solutions

**Audience Member 14**: So do you think **styling will sort of improve this** because they're you can see that the algorithm is like 16 a second. So basically I think this is the wrong grip so once I would be 30. Compared to what like compared to going from for example from Australia all the way to North America right. So you can do.

I mean, it could change basically the **apology of the network in a very valuable way**. I tried running no known **Starlink from Indonesia** and it didn't work like I had troubles getting peers over over Starlink. I tried VPNs and tried some like networking tweaks but it didn't work well.

#### Technical Connectivity Issues

**Audience Member 15**: I think I have a little. I don't think that is because they are nice. I'm not sure. There is some technical issue with just the. So I think maybe but I managed to bring you maybe I was concerned maybe I but I tried VPNs and tried like somehow to. I don't I don't know the right answer but I think it would be interesting to have like **Starlink is like some bandwidth requirement** that we can build around like that's sort of the limit or so.

But also I managed to connect to peers on an internet in plain. So I ran I ran a node in a plane so it's not a problem.

---

## Network Optimization Strategies

### Inclusive Peer Selection

**Audience Member 16**: I think there are some **lower hand input** that someone could do to be more inclusive to those nodes in Australia as well. And that is you know the inclusive so you could. Instruct nodes to had to include other peers in their mesh based on the **RTT** that they see.

So instead of just connecting to like random beers which might be the closest ones that could be an **RTT or latency threshold** where you're going to connect to two beers that are 100 milliseconds away to beers that are 200 milliseconds away and to beers that are 400 or something.

#### Global Inclusion Strategy

And in that case it doesn't mean that Australia is going to suddenly come closer but it's going to be **more included in more like nodes** are going to be included in meshes of other nodes and therefore you know getting the messages will be equal to anywhere else. And then that's a very simple change.

### Satellite Constellation Solutions

**Audience Member 17**: Or you have this **on low orbit constellation of satellites** to solve bridge everyone. What's the problem of the world? Yes, everything else? Are we sending them out to Mars? Yes, send them out. It's still been in the work time.

**Moderator**: I'm very happy to see that. Very cool. How are you on time, Jess? We can wrap up.

---

## Session Conclusion and Future Outlook

### Wrap-Up and Gratitude

**Moderator**: Thank you everybody. Thank you. Thank you everyone for coming. It was definitely helpful for me. I'm hopeful to have at least one day every year. I was hoping to organize a B-moink before that connect but a couple things happened. One is that I was told that might be an interrupt. And the other thing is that I make a second child around the time. So I don't want to give it to you.

### Future Commitment

**Moderator**: Overall, I am expecting to **spend more time on the in-project**. I am hopeful that together we'll age like 5-1. Thanks again for the teamwork that has happened behind this. It's amazing to see about **50 to 60 people here from a dozen teams**. Thank you. Thank you.

---

## Key Takeaways

### Technical Consensus
- **6-8 second slots** appear technically feasible with current understanding
- **Variable vs. shorter slots**: The complexity lies not in duration but in variability
- **Implementation readiness**: Slot restructuring proposals have more existing code than simple reduction

### User Experience Priorities
- **3-second average confirmation** hits the sweet spot for user experience
- **Censorship resistance** improves with 2x more block producers per unit time
- **Application benefits** from fresher data and reduced MEV opportunities

### Implementation Challenges
- **Geographic latency**: Australian nodes face measurable disadvantages
- **Resource management**: Shorter slots increase client complexity across all subsystems
- **Gas limit scaling**: Needs coordination with slot time reduction decisions

### Strategic Questions
- **APS vs. shorter slots**: Trade-offs between additional consensus rounds and timing benefits
- **Timing games**: May provide latency benefits rather than purely negative effects
- **Infrastructure readiness**: Starlink and satellite constellations could reshape network topology

---

*[End of shorter slots roundtable discussion]*

*Note: Transcript generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*