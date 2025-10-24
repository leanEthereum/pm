# Lean Day - Post-Quantum Testnet Roundtable Discussion

| Item | Details |
| ---- | ------- |
| **Session** |  Roundtable Discussion - Post-Quantum Testnet Planning and Concerns |
| **Key Contributors** | Thomas Coratger (EF), Yanis Psaras (Probelab), Justin Drake (EF), Various Client Teams and Community Members |
| **Slides** | no slides |
| **YouTube** | [link](https://youtu.be/7iby_2Tzqvg?feature=shared) |

>back to: [Lean Day Overview Page](/workshops-and-interops/2025/lean-day-EthCC/00-index.md)

---

## Post-Quantum Testnet Roundtable Discussion

### Opening and Context

**Yanis**: I didn't get to introduce myself. I am **Yanis**. I think it's just important to have a short intro before we start focusing on the P2P layer of networks. We have a lot of experience with LibP2P. Of course, we are keeping an eye on the Beam chain particulars and could help in any way when needed.

So this session was kicked off as I said. Thanks to the very brief transition. Maybe the first question would be: **when would a testnet be feasible other than next week?** A realistic target.

---

## Data Storage and Chain History Concerns

### Initial Concern: Proof Size Impact

**Audience Member 1**: Yeah, we just had a discussion with other guys about if we have 100 to 128 kilobytes of aggregation proofs. I suppose it should go on chain and we suppose we have like four-second slots. This would produce a lot—how much? 80 kilobytes per day? For one month it will be about 80 kilobytes just for signatures, which is just for signature aggregations.

Is that a problem? How are you going to handle that? I don't know, it might actually make it a lot harder for a light client, unless we have some smart proving scheme.

**Thomas**: Just to complete, today I'm going to able to sync the entire Ethereum in about three months, something like that. And any node that has to store the chain for just three months. And so again, in this case if we keep three months it will be 240 kilobytes just for signature aggregation proofs. I don't know if we can shorten this period for the Beam chain or not.

### Revolutionary Change: History Disappears

**Justin**: So why do we keep that history? If you've disconnected for like two months you want to be able to sync up and replay the chain. But **once you've SNARKified the whole chain you can just sync to the tip just by downloading the proof that the tip is valid**. And then you don't have to re-download the history.

**History maybe is something that can largely disappear**. Every time you have a new block that's produced, it can just be—you prove that you've correctly transitioned from the previous state.

**Thomas**: Yes, so the SNARK that will prove validity of the tip of the chain will still be added regardless of what we want to store. That's a big change.

**Justin**: Yeah, it's a huge change. And then you can make it even more aggressive. So as an equivalent—because I don't know how to prove things to be honest—so you won't have to keep any historical data.

**Thomas**: For consensus purposes I'm talking about. For archive purposes, of course we have to keep that to be able to do other things. But to be a good, honest validator you won't have to keep any historical data.

### Resource Requirements and Validator Diversity

**Justin**: Yeah, it's a good question. I mean one of the memes out there is that **you can be a validator on a Raspberry Pi Pico**. And there isn't that much space to store history there.

I mean one of the things that needs to be done in addition to SNARKifying the chain is doing **data availability sampling** on it. Maybe we're going to need some fraction of the nodes that store the responses to the queries. Maybe we can make a similar assumption. Maybe we can make a similar assumption for the aggregators and the provers where **we assume 10% of the validators have the resources** to store that. And because it's only a minority then it works out.

---

## Technical Optimizations and Security Trade-offs

### Signature Size Reductions

**Thomas**: Maybe like as you've complemented—I think there's a paper, a new paper from **Benedict and Dmitri** has reduced signature sizes again compared to what was presented there. It was not presented this morning but I think this may not solve your problem.

From the STARK perspective I don't think that we can reduce the proof size that much. It is already an ultra-aggressive situation. Like the classical situation in all the zkVMs is to use a wrapper at the end using **Groth16** or something like that to wrap the final STARK proof into a much smaller proof. But the Groth16 system is very small but **we don't want that because it's not post-quantum secure**. So **100 kilobytes is already very aggressive**.

**Audience Member 2**: One note on that is we use an FRI-type proof system right now and our proofs are only like 256 kilobytes. And we have a method for getting them down to 128, so it's still could be not true. It's just like the world of FRI.

### Security Conjectures and Mathematical Challenges

**Thomas**: Also I think that Dmitri mentioned this morning but **we have strong constraints about the conjectures that we can use**. Because for example I know that in Plonky3 they are using the conjecture of the STARK paper and we cannot use that because we are not sure about the validity of this conjecture. So we are also constrained to be very restrictive.

**Justin**: We have a question maybe for Jacob. Ethereum Foundation is considering during the following: we would basically **formalize the semantics of these conjectures** and then we have a **bounty for anyone who can either prove the correctness or give a counter example**. Is that something that you think the zkVMs would want to contribute to the bounty pool?

**Jacob (RISC Zero)**: I understand. So generally it is part of the state as much as possible, but in this case for sure. We would participate in the coordination and maybe at the moment we can figure out. The quote—Jeremy [Wei] is not here. It is like **he won't sleep till it is formally verified**. So we are doing anything we can.

In this case the formal verification is going the extra mile. But really what we care about is **proving those conjectures so that everyone can reduce the proof size**.

### Timeline and Mathematical Reality

**Justin**: Is there something before the testnet?

**Thomas**: No, obviously. This is a **very hard mathematical problem** that there are people who have tried to tackle. But with the right signal and incentives, if many top companies say we need it, then the smart mathematicians... we need—it would be great if we could get the top mathematicians to think about this.

**Jacob**: Maybe I can paint some light on where we stand right now. Our **conjecture security is not the best. It is 97 bits**. We have been very conservative. We are actually paying someone right now to improve conjecture security above 100, maybe even quite far north of that.

And then later this year at the beginning of next year we are going to start proving to prove the **128 bits**. I think that we are going to target **120 bits of security** again.

Even if we use a conjecture for this, if the conjecture turns out to be false, we will not lose everything on security. We can still have like **100 bits of proven security and 128 bits of conjecture security**, which is okay.

---

## Consensus Design Alternatives

### Separating Consensus from History

**Audience Member 3**: I'm picking up on an earlier thread around the proof sizes and whether we can put it on-chain or not. Is it sufficient for the consensus layer to validate that we don't actually need to know the votes if we have proof that the state is valid? Could we **move the aggregation off-chain** and have it maybe not later if we just get a proof that blocks advance the state?

**Justin**: The beacon state is pretty small and new. It's a fantastic point. We only need **data availability sampling on the state, not on the history**. It is a very small amount. We are not doing data availability sampling at all. We are doing it in a way that we can do the blocks.

One of the things that the tablets wants to do is **remove the notion of blocks**, take the execution blocks, all of it including the headers and everything, concatenate the whole consensus blocks and then do **data availability sampling on everything**. We would also want to add in the state to that.

### Security Conjecture Impact on Proof Sizes

**Audience Member 4**: You guys mentioned the security. I don't understand the link or the correlation between security and how it proves that. If someone can prove the conjecture, by how much is the proof size reduced in kilobytes?

**Thomas**: That is the max. The **FRI conjecture is the most aggressive one**. There is the conjecture with someone in the code. Everything is in the paper called "proximity gaps for Reed-Solomon codes." There is a proven result of the **Johnson bound** which can be used in the worst case. This theory of discovery is also valid in the mutual agreement setting.

The real conjecture is this result but **up to capacity**. This is the conjecture that we want to be true to have few more reasons. If this conjecture up to capacity turns out to be false, we will have to use up to the **Johnson bound**. So, more conservatively probably like we expect up to 256 kilobytes instead of today's 128 kilobytes.

**Audience Member 4**: I think the 128 kilobytes uses the conjecture up to capacity. If it is false you want to multiply to 200 kilobytes.

**Thomas**: We are using the conjecture right now. It depends on the field, but it's 16 megabytes. But the 128 kilobytes—is that quick?

**Audience Member 4**: Yes. So you can go down to 16 [kilobytes]. We can't be [sure]. It is the real log of security on the field. Maybe 100 kilobytes.

---

## Testnet Planning and Timeline

### Transition and Timeline Discussion

**Yanis**: Very brief transition from Thomas. Maybe the first question would be when would a testnet be feasible other than next week? A realistic target. The session is about post-quantum testnet. I don't know if we could put our managerial hat on. Should there be a target date? Some requirements. What are the most critical things you have done for those of you that are here?

What would a successful testnet look like? Maybe first kind of metrics, of course, things should not crash. But maybe we could go into a little bit more detail.

### Timeline Proposal

**Justin**: Okay, this is just my opinion. Let me answer two questions. In terms of timelines, the way I see it is that **Q3 is when we would implement and spec out the cryptography**. So, ideally, basically immediately after we would be able to boot up a testnet.

So, what would the testnet do? I guess we could—a natural starting point is just to look at **one single subnet of one of the layers**. And in my mind, that's practically a size of **1000 nodes**. And we could try the simplest thing possible, which is just plugging in GossipSub that exists and see if things break and see what kind of latency we can get.

And if the latency is much higher than what we want, then we start thinking about optimizations to bring it down.

### Interoperability Concerns

**Audience Member 5**: So, for example, interoperability between different implementations at the networking layer, having the signature aggregation and SNARKs would be part of the testnet? Or perhaps more small-scale experiments in the run up to the testnet?

**Justin**: I mean, we really have interoperability. Yeah, of course, it would. So, I guess we just really see a lot of reasons that exist. And we do have as many wrappers as possible around the cryptography in very clean ways. So, those would be like access to multiple wrappers that you need.

**Audience Member 5**: I just wanted to point out that interoperability is really holding you back on that. So, **interoperability is introducing a bunch of edge cases**, which are very important to be created. So, when you are testing during that kind of test for the platforms, well, how far they could provide really incremental development in the middle, actually.

It's better to **separate these aspects and then link the testing** as one part and the platform testing as another one. Because if you put that information on a bunch of energy and stuff, you don't want to blame it on the platform.

---

## Implementation Challenges for Client Teams

### Cryptographic Implementation Concerns

**Yanis**: Maybe an observation related to that. Maybe to ask client teams for the testnet. I have seen that many of you client teams are trying to re-implement, for example, the **XMSS signature scheme** and stuff like that. How difficult is it? Do you have cryptographic teams for that?

In the future, do you plan to use wrappers? Maybe libraries, or do you plan to continue with cryptographic teams? Because I know that this is not common for client teams to have cryptographers at home.

**Gajinder (Zeam)**: So, for signatures, since they are happening outside the zkVM, they are separated from the zkVM itself. So, we will **start off with using bindings** for the libraries that you have created. And then we see at what point they get mature and the scheme is fixed and see if there is some value in translating them into, for example, Zig.

### Standardized Bindings Approach

**Justin**: Actually, that's a good point. So, the bindings for the signature libraries, one of the things that I've heard a lot is that the **BLS bindings are painful to work with**. And so, if we could have the existing consensus clients tell us how they want the bindings done, then I guess we could create them for them.

Any existing consensus client here? I'm almost coming into the bindings.

**Manu**: Is it a **single implementation from a security perspective** like you have with BLS? Or is there a value in people having their own implementations instead of using the bindings?

We have two cases as well. We have **c-kzg**, which is well chosen by the centralized approach. And BLS is so blessed. Not in a pretty set of things, it makes sense to have a strong cryptographic team and the bindings rather than have scattered implementation of cryptography, the micro-sense.

But so far for BLS and KZG, we follow this approach. KZG library, self-hash, bindings, some product, very rare. The latest bindings have been released and client teams themselves, they basically go and maintain those bindings as well.

**Audience Member 6**: Yeah, I would go with one [implementation] and just put as much effort into security, especially for formal verification, which should be possible given the simplicity of the schemes themselves. So the bindings are what are important and the actual logic is just one super optimized thing that's been hardcore verified.

---

## Testing Methodology and Simulation

### Client Implementation Strategy

**Yanis**: So going back to the beginning, if we assume that there could be a testnet in Q4 of this year, maybe we could go around the table and see what clients will want to have implemented by then. Does that make sense?

As I've seen throughout various presentations today, more zkVMs, integrated implementations or having interoperability with other things—would that be something useful to start setting perhaps?

**Audience Member 7**: I guess from my perspective, getting a testnet having a **single client** testnet makes sense to start off first, because then we don't have to worry about interoperability. I think it also—if you want to measure performance, you can see that there will be different performance of different tests and different clients and we can optimize for that.

But in terms of actually getting the testnet up and running, I think important things like how much of the existing specs we want to reuse. For example, LibP2P layers—one of them. So yeah, I think for doing a testnet it would be really achievable. I think right now it's not really well scoped down. So it's a little hard to comment on a lot of this.

But I think if we're reusing GossipSub, something, the LibP2P layer should be fairly minimal and then we'd be reusing Rust libraries. So I don't see a reason why we couldn't achieve this other than it being not really well scoped.

### Simulation and Network Testing

**Yanis**: And yeah, then following up on Kamil's presentation, we've seen a presentation this morning from community about the discrete event-based simulation simulator. So **what would be the studies that you would like to achieve?** Either you or I'd rather have to give it out to the community to carry out tests. What are the actual simulation experiments that you want to run in order—what are the things that you want to verify using this simulator?

**Kamil**: Well, first of all, we need to make sure that we... Yeah, we have **four-second slots**. So we have to make sure that you can perform this aggregation within this period of time. And actually the period is not four seconds, it's actually smaller because there are other things that we need to do during the slot. So I would say it can be even **two seconds or three seconds at most**.

So yeah, so far our tests showing that for approximately—we need to conduct more tests for large networks but for like 500 nodes, for a thousand nodes—it takes approximately **90 milliseconds, second and half to perform the aggregation**. So it should be doable.

The **bandwidth requirement** will be like the cables and the DSL connections. We've been with like **20 megabits per second** and the network works well.

### Network Protocol Design Challenges

**Kamil**: Yeah, so for us it was just the assumptions that increasing the number of validators, increasing the size of the aggregation—so everything works.

Another thing is that as I said, there is a design space like how we propagate, for example, the **SNARK proofs**. Like we cannot simply gossip them as they're too large. And if we do that, then we announce a bitfield, then requesting the SNARK, then this kind of creates **another round of communications** which we do not have right now.

We will need to test this a bit more thoroughly—if it's also the basic aggregation. So far we're just showing that even with GossipSub we stand at the mesh of size 6—we're connecting to 600 nodes. Yeah, it should be good for the SNARKs.

Yeah, for the SNARKs, for the post-quantum signature aggregation scheme. Yeah, there are things that we're like—if we perform SSF, I assume we're running **multiple rounds of aggregation in parallel**. Yeah, maybe this is another thing that we also need to test which we are currently not doing.

We also would be great to check, like, once we perform this aggregation, we would maybe try to test how this aggregation, the final SNARK should be broadcast to every peer in the network so that it could be included on chain. At least it should be sent to the **block proposer**, the next slot I assume.

### Stress Testing Methodology

**Yanis**: So the simulation that you want to do is primarily to verify that these network protocols are going to work okay?

**Kamil**: Yes, yes. And this is a normal GossipSub, not any two-point anything.

**Audience Member 8**: Yeah, so what I wanted to ask is, simulation methodology. I think on these tests, so if you set it up like this—you take the minimum requirements, then you run it. What you get is a **best-case scenario**. If you run many simulations, you might get a percentage like it fails with 10^-3 probability, you run a little.

This is much better if you do it in a different way. It's the **stress testing** for ways to, let's call it **nasty scenarios**, which will happen. During the night it's working, because in the day it's not, because during the day there is a traffic spike.

So you have to search for these nasty scenarios because there is one parameter that you can tune, and then you find the breaking point, basically. When you have this kind of—you manage to map out this bandwidth, scale down, partition network, these kind of scenarios, then you have a **much better view of what's the robustness** of your solution.

**Kamil**: Yeah, and **network partitioning** is a typical good test, so we say the bandwidth between nodes in Asia and nodes in the Americas is limited. And then you see how your nice solution transforms into a catastrophe.

---

## Implementation Details and Tooling

### Observability and Metrics

**Yanis**: I think that also for this kind of testnet, we probably need **more observability** on new things, like the samples of validator cells, validator keys, stuff like that—a new kind of **block explorer**, where we need also to have information about cryptography.

**Justin**: Yes, that ties in a little bit to Kobe's point, which is that the testnet is not fully defined. I guess the most minimal thing that I can think of is that we have **random validators, so we just round robin through them**. And then the block is just the slot number, the parent hash, and the aggregated signature. And I guess the signature of the block itself, and then that's it.

And then the block explorer instead of showing the attestations, for example, that you can see, we'll have this aggregated proof. And that's exactly what we can see going in Zeam right now, in our state transition function, minus the SSF that we implemented, that you have this only basic thing, where you just build the chain.

And when you have signatures on top of it, then you will get to this point, where we can do this step. There won't be any deposits or all that mechanisms. So **if you preset the list of these validators**, then basically if you do round-robin block production, I think that would be pretty simple to handle and demonstrate the signature aggregation.

### Development Tools and Debugging

**Audience Member 9**: And in your current development, like the client teams, do you develop also observability tools, like Grafana or stuff like that, to have more feedback on the design when we launch the testnet or stuff like—do you develop this kind of stuff or is this too far from the problem?

**Kobe (Ream Team)**: I mean adding Grafana metrics is fairly straightforward, because normally you would just have a global Prometheus object, and then you could just inline those where you need them. So I think for metrics, we could get those done super quick, if needed.

### Testing Infrastructure: Kurtosis

**Audience Member 10**: I have a comment about testnets. I'm not really sure if client teams know about this. The EF has a very nice tool called... I heard it was maybe Kurtosis or so. So it sets up a testnet on your computer in your network, like in seconds, which can specify, okay, I want X amount of clients and give a number of data.

So you can very easily test either yourself or your client or interoperability with different clients, twice to check if everything is okay. We really use it before launching any net, depending on the net. And so before launching any testnet, it actually helps to speed up by a big factor the development process.

**Thomas**: Yeah, I mean, I just think that we should use it for consensus and execution layer. Maybe you should extend it to cover as well for a group of consensus clients.

**Audience Member 11**: Just a comment on this. **Kurtosis is a very manual process**. Like you need to create the configuration, you need to do a lot of coding. So that means if you want to use Kurtosis for a Beam testnet, which I agree is a good idea. But we're going to need to get these apps on the case because no one else knows how to use it.

In fact, the company that created it gave up on it. And so EF has to maintain it themselves. I'm just saying that there is probably going to add quite a lot of delay in the timeline.

---

## Final Implementation Considerations

### Single vs. Multi-Client Testing

**Justin**: Just I'm getting a little bit for the one client testnet. I'm not in the sense of one client, but something which is good—was a, for example, so that a **single client can do its own testing**, because then you have full ownership of what's happening, and you can go in and find problems.

But if you have all the success with the interoperability, you can be there debugging interoperability things, and you have no idea what's happening.

So, that's kind of the... So, before it does, yes, you do self-interoperability. Yeah, so it's just definitely the step that we can do before we will start interacting with other consensus systems.

### Hardware Requirements

**Audience Member 12**: I guess another thing that I want, before starting off with the testnet, the **hardware specs**—are we sticking to the current EF hardware specs, or are we going to have upper or lower down the specs?

**Thomas**: For me, I think that this is almost the same because as we discussed, at the beginning, with emulator, particularly on the GPU version, to have the target for the signature aggregation through proofs. At the end, we figured out that the **CPU should be enough**. So I don't think that we need fancy things to do the testnet.

**Justin**: Yeah, I agree. I wouldn't increase the requirements. What we could do is for a very large subset of the validators, **decrease the requirements**. So we can have them run the weakest machine on the cloud, for example, for 80% of the validators, and since the CPU happens.

So I hope people would test that again, I think that's better than before, in the testnet. Yeah, because **we only need, on the order of 10%, 10% of a thousand being 100, 100 aggregators** doing the work. I mean, 100 is even more than necessary.

### Consensus Mechanism Questions

**Audience Member 13**: Maybe another question related to crypto. We have talked about a **minimal zkVM** that could be enshrined into the consensus. Are you particularly afraid of, I mean, like a new VM? Because I know that each time we are talking about VM, we cannot have like a thousand of tweets about this.

Have you considered the possibility of using the data defense index and having two systems—having a single one, maybe keep the BLS and introduce a new STARK one with less future security? So you have **another one anyway**, or something like that. Because that will also address the concern on the zkVM, besides you go with two for a while. And then when we are confident about the new one, we can just ditch the other one and transition.

**Thomas**: I think that this is something that, at some point, we considered, because I remember Justin taught me briefly about this in devcon, like this is the possibility that we could go like, starting from BLS, but I'm not sure that this is the best idea right now, because, as mentioned in your talk, Justin this morning, the best is maybe to **start with a state-of-the-art**, and go backward from that.

So I would say that the testnet should be a good idea to test this minimal zkVM, and also we want it to be really minimal, so not like a RISC-V completely, or something like that. We really want to put it to the minimal.

**Justin**: I mean, one thing that we're very much leveraging in the context of the zkVM aggregation is that we can **merge two bitfields** that overlap—that's something that we can't really do with BLS. So actually, it's an interesting idea, but BLS in that sense is **strictly less powerful** than what we would want for the permissionless aggregation.

---

## Session Conclusion

**Moderator**: We're actually out of time, but this was very productive. Thank you so much for your positive participation. We're going to have a quick break, and then the second round table on shorter slots.

---

*[End of roundtable discussion]*

*Note: Transcript generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*