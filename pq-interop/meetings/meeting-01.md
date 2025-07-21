# PQ Interop - Meeting 01

## Meeting Overview
**Date:** July 18, 2025
**Agenda:** https://github.com/ethereum/pm/issues/1631
**Recording:** https://www.youtube.com/watch?v=AeCY-vabyuY

---
## Meeting Notes

- **Zeam Proposal Overview**: Emphasized collaborative Devnet for accelerating post-quantum (PQ) signatures in Beam's lean consensus, starting with unaggregated signatures, minimal blockchain features, Many-Three-SF variant, SSZ with Poseidon hashing, and 4-second slots; targets stable month-long Devnet by next Beam Day, scaling from 1,000 to 1M validators with 98% attestation correctness.
- **Ream Proposal Overview**: Focused on iterative Devnets to validate PQ signature viability, multi-client interoperability, and resource requirements; proposes no initial aggregation or full consensus, QUIC networking, simplified block structure without parent roots, and time-boxed to end-of-year completion with success metrics like 1-hour stable runs and <1-second signing times.
- **Key Discussions**: Alignment on starting small (e.g., 1,000 validators, short signature lifetimes) for fast iteration; debates on networking (TCP vs. QUIC), consensus mechanisms, and validator counts (keys vs. nodes); updates on upcoming Poseidon and PQ signature specs, minimal ZKVM progress, and potential adversarial testing like signature killers.
- **Broader Outcomes**: Value in multi-client participation for diverse perspectives and testing; introduction of third client team (Quadrillion); plans for Beam Week workshop in September for deeper collaboration on PQ crypto and consensus designs.

**Action Items**:
- Finalize PQ signature parameters and release libraries/bindings for client experimentation.
- Achieve consensus on initial Devnet spec (including networking and consensus details) by end of July.
- Set up shared repository in Lean Ethereum org for collaborative documents, specs, and plans.
- Schedule next standing call for Wednesdays at 2 p.m. UTC; prepare agenda to include unresolved questions and simulator insights.
- Reach out to additional consensus layer teams for potential participation and share meeting summary/recording.

---
## Meeting Transcript

### Introduction and Agenda

**Host:** All right, so this is exciting. Glad to have this first call. As I noted, this is kind of like coming together, bootstrapping this first call, but really the plan will be to migrate this to a public call on the protocol calendar moving forward. We just wanted to make sure that we can resize the meeting, align on some objectives and artifacts. I went through and pulled together a bunch of different resources. This is all in the document that's attached to the invite. So there are some resources here—happy to add to it. Trying to document who's going to be contributing to this effort. Really, the goal is to use this call to first talk about the different proposals that were put out by the Zeam team and the Ream team, discuss any takeaways from Beam Day, and then hopefully converge on a shared work plan that we want to then socialize and have be the framework for what we do moving forward. I'm going to pause for a moment... Okay. So, if maybe Gajinder or... Oh, if one of you wants to kind of start us off by walking us through your proposal, I can pull it up on my screen and talk through it.

**Participant:** Yeah, sounds good. Maybe Zeam can go first.

**Host:** Yeah. Please go ahead.

### Zeam Team Proposal Presentation

**Zeam Representative:** All right. So you already clicked on the meme. The point is that the meme is the main thing. This is a meme-led development. The meme says that we will not work in silos. We will collaborate, and Devnet is the right way to collaborate, as we have already seen in the Beacon development roadmap. Various teams come together, collaborate on the spec, figure it out, try to run the tests released on the spec, make sure they can pass the tests, and then try to interoperate where other issues are discovered. The spec gets simplified. This is what we are hoping from this particular series.

The aim of this series is to accelerate the post-quantum signature schemes for Beam's lean consensus. The end is trying to basically position as the endgame for Ethereum consensus. We know that quantum computer development has really accelerated, and it's high time to iterate, implement, and prove that we have a scheme that works—whether it's implemented as a Beam hard fork or just absorbed by the normal Beacon research and development. It's a win-win for us in both scenarios.

Again, the aim of this Devnet series is to prove that the post-quantum signatures work, to achieve interoperability and aspect consensus, to accelerate research, and to provide a test bed for P2P research that we think will be required in the larger signatures and the kind of validator set that we are trying to target. Bonus outcomes would be to build the tools around this.

What we want is to set a target for ourselves: have a stable, month-long running Devnet by the next Beam Day, and then work backwards from there to figure out the milestones we need to cover. This is a very simple Devnet series because we are only targeting post-quantum signatures to begin with, and at a later stage, we will also target aggregation of those signatures.

If you can scroll down... The scheme: There have been many papers published on the scheme, and I think there will be another paper published on the actual parameters, which might be 64 hash chains with a chain length of eight. Basically, the aim is to minimize the signature verification or generation hashes so that the prover and verification, when we do for signature aggregation, is quite easy.

The lifetime: These are one-time signatures, and they are created in a multi-tree. XMSS-based scheme is applied on top to make sure you can sign multiple times. For most of this Devnet series, we think we'll have a low lifetime because we want to iterate fast, and there's no point trying to have a tool which takes a large amount of time to generate signatures—that's not contentious. We can generate signatures for a longer lifetime. So, maybe start with a short lifetime and eventually go for a month-long time. If we feel it's imperative to show we can work with signatures for like 5-100 years lifetime, we might go for that.

For aggregation, we are starting off with unaggregated and will add it as and when the aggregation library is ready.

In terms of blockchain, what Zeam is proposing is a minimal viable blockchain, which basically means there should be parent-child linkages—it's quite easy to track the latest block header in the state and do that validation in the state transition function. It's very low-hanging fruit that we should just have.

With regard to justification and finalization, we are proposing a Many-Three-SF variant that we have implemented. I'm saying the Zeam variant here because we did some changes to make sure there's special processing for the first block so that Genesis is slot zero itself. All these are very slight modifications, and then there's translation of the various structures into SSZ containers. It's not really difficult to map those things, and I don't think it's going to be contentious in terms of how we map Many-Three-SF onto our implementations.

For networking, there needs to be a block-by-root request-response apart from block and attestation gossip. We might just start with gossip 1.2 and then iterate on gossip strategies. But we also need block-by-root—if not block-by-range—because if you miss the parent block via gossip, it could become a deadlock. Block-by-root should be sufficient to handle any syncing scenarios, as long as you're not syncing from Genesis on a very long chain. We will encounter variability, errors, and misses, especially in the Devnet when starting something new.

Regarding using QUIC, most likely we are okay with QUIC because we will be either using Rust libp2p or Zig libp2p bindings over some C library. This remains to be seen, and we're trying to figure this out. Without an actual experiment with the Rust libp2p library integration, I don't want to commit, but we can figure it out—whether we can do it or go with Yamux.

Other basic handshake mechanisms we might require for networking, like ping, metadata, status—all of these are quite simple and should be okay to implement as basic needs of our client.

For encoding and compression for gossip and request-response, we will go with Snappy and Snappy frames, same as what's done on Beacon.

Another basic functionality we require is checkpoint sync from the last finalized, because we don't care to support checkpoint sync from any other checkpoint. Last finalized is good enough, and then we have block-by-root to sync post-finalized.

With regard to slot duration, we recommend going with four seconds because this is what we want to target eventually. We don't have any baggage from Ethereum, and we don't even have aggregation to start with. Signature generation and verification could be very fast, so I don't see a reason for a longer slot time.

We are recommending SSZ with Poseidon for hashing consensus objects. That's a no-brainer/non-contentious proposal with widespread support.

Coming to success criteria: For any iteration, we propose 98% average attestation correctness over Devnet lifetime—whether 10 days or a month. Once we have this, we say the iteration works and can scale it up, starting with 10,000 validators and 10x-ing on each iteration, up to 1 million validators.

If we can inject adversarial actors that generate bad signatures or try to generate signature collisions, that would be a bonus. Censoring aggregators or, since aggregation is ZK verification, signature killers in terms of proof killers—this is something to consider, though I'm not sure if it's possible due to the limited instruction set.

These adversarial actors are things we should think of surviving in any Devnet iteration—it's a bonus, so we don't need to begin with this; we can add at later stages. If someone wants to work on making these, they can start thinking about it.

Next steps: Finalize the signature parameters—most likely what we and Ream have proposed. Then, releases of the signature libraries with those parameters and bindings/APIs. Once we have that, we can start experimenting at the client level. In the meantime, generate the spec for Beam chain/lean consensus for basic functionality in terms of state transition function and forks.

We will need iterations to reach consensus on basic networking points, but I don't see major contentious issues, so we should reach consensus fast.

Following how Devnet is done on Beacon chain, we should have first iterations, then ETAs and implementation ETAs. What we need is a Genesis generator that takes number of validators and lifetime, generates the Genesis state, and public/private keys for validators.

This covers the points we need to kickstart such a series. Yeah, that covers most of the points from Zeam.

### Q&A on Zeam Proposal

**Host:** Fantastic, fantastic. Do you want to go through the Ream one and then go over any overall questions, or do people want to pause here and ask questions?

**Ream Representative:** Yeah, I think it would be nice to do a Q&A while things are still fresh. I do have a couple questions myself actually. Maybe I should start with the questions. So, I guess like my personal view first, because we haven't really talked to the team based on Zeam's point of view yet. These are my personal questions and opinions.

I think Three-SF is definitely interesting—we discussed that in the team. But we are not so sure how much effort it would be to actually turn that into an actual implementation that you can run in a network setting. We have the Rust implementation based out of Metallicus' prototype, but that's basically without any networking; it's only simulating in one process.

The second thing is we didn't put down anything about signature killers. Introducing bad signatures would be interesting. It's probably not difficult to introduce a few clients that randomly produce invalid signatures from time to time.

I do have some questions. First is the 10,000 validators. Our estimate of the current actual nodes right now on Ethereum mainnet is around 8,000, so I wonder if 10,000 validators might be a stretch. Also, the effort to set up a 10,000-validator network could be quite challenging.

**Zeam Representative:** I'll go one by one. First, talking about Many-Three-SF: In any case, you will need to generate attestations and blocks. Doing it over networking is not really difficult—you serialize the data, publish it, and others deserialize and import it into their forks. I don't see any challenge there. When packing in the block, it's basically SSZ that does the magic. We can talk about this offline if there are concerns. From our development, we integrated it as part of the Zeam client.

The thing you'll deviate most on is fork choice of blocks. For example, the Alex implementation is not how fork choice is implemented in Beacon chain, but it's essentially the same: You have a view of the latest votes and apply it. You have a vote tracker—if a validator voted at this block previously and now has a new vote, you apply changes when finding a new head on a new slot. Look at our implementation, and we can discuss and figure it out together. I don't see any issue.

Second, regarding 10,000 validators: I don't mean 10,000 nodes; I mean 10,000 keys. Your Genesis generator will generate those keys with the particular lifetime, and votes will generate traffic on the network. We'll see how many nodes we can have, but in any Devnet, we don't have that many nodes. This will give insight into traffic generated. Going to 1 million validators is again about keys—to see traffic and optimize GossipSub.

The aim of this signature testnet is to first prove PQ signatures work, then aid research to make them viable at scale for Ethereum. We'll need to scale up validator keys and handle traffic. Since we're keeping chain length small, signature size is big—that's a critical difference. Once basic signatures work without aggregation, throw aggregation in as soon as ready.

Regarding signature killers: Doing bad signatures is easy, but I was mentioning aggregation killers. I'm not sure if they can exist because the instruction set is small for the signature prover. I leave this as an open question to cryptography and research teams.

**Ream Representative:** I do have one more question on the attestation correctness. What do you mean by correctness? Is it that all validators receive the attestation and validate correctly, or how do you calculate that?

**Zeam Representative:** When voting on Beacon chain, you vote on source, target, and head. If your head is correct (matching everyone else), other things are correct, but inclusion could be delayed. We have parameters to figure out effectiveness of a validator's attestation—that's how rewards are distributed and is relevant to everyone who wants to stake. It's a benchmark that's important.

We'll define correctness as on Beacon chain, track it, and figure it out. Having 98% average means most validators follow the chain and vote correctly. For Devnet, maybe 80% average is good enough, but we want to establish that you can sign, aggregate, and publish on time. Many factors involved—keeping a high bar will expose challenges, and that's what we hope to discover, iterate, and fix.

**Host:** Excellent. O, do you want to ask your question?

**O (Participant):** Yeah, everyone just also had some thoughts about this proposal. I'm more in favor of having fewer validators because, as far as I remember during Beam Day, there were discussions about first testing just one subnet. Even without aggregations, if we have 1,000 validators instead of 10,000, this would give room for validators to optionally enable aggregation—even if they don't publish final snarks to the network. It would be great to check how long it takes to generate aggregations, maybe share to public telemetry like "generated a snark in this period of time."

With 10,000 validators, this would be complicated because current expected aggregation rate is around 1,000 signatures per second—that would take 10 seconds, and with four-second slots, that's too much. It's generally easier with 1,000 validators, giving insights into what we can expect if we have subnets of 1,000 validators each. So, more in favor of around 1,000 validators for the testnet.

Another thing about QUIC: It's great, but it would give a more optimistic picture. QUIC is better than TCP for streaming and delivering messages faster, but we should optimize for the most pessimistic case. If we know how consensus aggregation or distributing signatures works with TCP, that gives a pessimistic picture we can optimize with QUIC. I'd go with TCP first and try QUIC later.

**Zeam Representative:** It makes sense to start with 1,000 validators so we can have behavior like a subnet, but we can follow the same process of 10x-ing. For QUIC, I'd delegate to experts like you on the best protocol.

**Host:** Great. Any other questions on this proposal? If not, O, you want to walk us through yours?

### Ream Team Proposal Presentation

**Ream Representative:** Sure. I heard the Ream... Maybe can I share just a little bit? For us, this is our view—basically a recap of what was discussed in its entirety during Beam Day. It contains both our opinions and the collective opinions from the day. By no means do we think this document is definitive; we hope it's a good starting point to formalize a plan. Happy if this gets used as a starting template, or if some parts get dropped.

It never makes sense the most for the entire collective to move forward with the Devnet. A few things I can mention: First, we think it's a Devnet test, but that's probably misleading because to be a testnet, features should be more complete. This is more of a one-off, potentially with throwaway things for the first Devnet and then the next.

We think a Devnet is the more appropriate name. We favor iterations of the Devnet over waiting to build a complete testnet. Our first Devnet is probably more about validating PQ signatures, but also lessons on how lean concepts work overall and can be coordinated—both between client teams and with researchers.

The Devnet is probably not intended for public reuse; maybe the infrastructure isn't even accessible by the public. Features should be limited, and we prefer to time-box it—completed by end of this year, potentially before DevCon, so we have demos and results to show. This speeds completion so we can take knowledge to a fresh start next year.

Critical questions for the first group calls: The objective of this Devnet—we discussed in the team that we'll go over steps to get this multi-client Devnet up. It's important to answer why this Devnet exists and why it needs to be multi-client interoperable. If most stuff can be done under a single client Devnet, that compromises legitimacy for building multi-client.

Second, confirm timeline—we can aim for end of this year. Third, who needs to be involved: Zeam is fully committed, Ream is fully committed, but we need other key persons—namely PQ signature researchers who could provide a spec, networking folks. Big question: Are there other client teams (new or existing) that would like to participate? Include them early.

Those are our general views. I can go through our detailed plans—I'll glance over for time.

We broke down objectives into primary and secondary. Primary: Ones we should definitely complete. Secondary: Good to have.

Primary objectives: Prove PQ signature viability; validate multi-client interoperability—this is important as subspecs are developed independently, so first time combining; validate resource requirements (keep specs at IP 7870, validate feasibility); identify specification gaps missing in subspecs for implementation.

Secondary objectives: Standard common metrics—we talk about performance, so come up with temporary standards on measurement, share to common infrastructure for analysis; multi-client coordination protocols; implementation challenges from PQ signatures, Three-SF, key generation (helpful for new and existing teams); reusable testing infrastructure—if we can reuse for subsequent Devnets, useful.

Target features: On par with Zeam—64 hash chains, chain length of eight, max lifetime 2^18. These are numbers agreed upon from Beam Day. No signature aggregation, mostly because readiness of aggregation subspecs—exclude until clear subspecs.

Data types and containers: Need to agree, but skip for now.

Our view different from Zeam: Block—we didn't include parent root inside child block, based on Beam Day discussion. Complications with block syncing and missing blocks; avoiding helps—not implementing APIs for block-by-root simplifies.

Attestation: Pretty much the same—slot, start index, signature; serialization and hashing use SSZ from Beacon chain. Improvements to SSZ proposed, but for PQ Devnet, use tried-and-true SSZ; hashing use Poseidon.

Consensus: Where we differ, based on Beam Day—we haven't decided, so maybe no consensus. Just sign blocks, propagate attestations, everyone saves their view. Hack: No single chain view, so verify by checking each validator for receiving all attestations/blocks.

Networking: QUIC on IPv4—based on assumption Beacon clients moving to QUIC (68-70% support, TCP/Yamux deprecated soon). No reason to try TCP. GossipSub: Didn't specify version, but assume V1.1 (typical for Beacon); V1.2 needs team discussion.

Success criteria: Not as aggressive as Zeam. If two applications, network finalizes blocks; at least running one hour without crashing; entire chain full block pipeline finishes within six seconds (not hard—analyze telemetry post-run). Block proposal: Each block signed within one second (based on Beam info); attestation same—one second.

Important: Not just measuring chain proposing/attesting success, but metrics for running each block/proposing/attesting.

These are project management stuff: Steps before setting up Devnet—paper/subspecs published; reference implementation published; Devnet specs reach consensus with at least two clients; clients implement Devnet specs and pass reference tests (lessons from clients: reference tests before Devnet useful, saves time). Only then set up and try Devnet.

Instrumentation and metrics: Pretty straightforward, copy-pasted from Beacon client. New stuff: Tracing start/finish of operations (key generation, block signing/validation, attestation signing/validation, entire block pipeline, block created/published, attestation created/validated). Important for benchmarking bare minimum Beam client, troubleshooting complicated features.

Suggestions to include hardware metrics to see distance from saturating requirements.

Timeline: Aggressive—by end of this month, PQ specs (already out), reference implementation (already out). By end of August, Devnet specification agreed upon (bit more than one month). Next two months: Client implementation. By end November, each team runs own single-client Devnet. By end December, successful multi-client Devnet.

The rest are housekeeping and rationale—I don't have to go over details. This is our view of the Devnet so far, without incorporating Zeam's view published today.

### Discussion and Q&A on Ream Proposal

**Participant (Thomas?):** Yeah, thanks for the presentation. I have a question about the specs: Basically, what you are waiting for is Poseidon's spec plus the signature scheme spec. I think the expectation for both of you is different—on the Ream side, as you're using Rust, you don't need a spec for Poseidon; you can import any Poseidon library like Plonky3 Poseidon. Almost the same for signature scheme—you can use Rust implementations and call that; no spec needed.

On the Zeam side, things may be different because you're using Zig—maybe need a spec for Poseidon (Nassio did Poseidon implementation recently). But you need a spec to use these.

Good news: We just started internally at EF to push a minimal spec forward, set up a repo with Python framework for spec, subspec, tests. Start with Poseidon plus signature scheme—so quite soon, working Poseidon plus signature scheme spec. Straightforward: Taking Rust implementation and transforming to minimal Python spec.

This is all you need for this Devnet—you don't need aggregation, minimal ZKVM, etc., correct?

**Ream Representative:** Yep. To start our first Devnet, we don't need aggregation bits.

**Zeam Representative:** With respect to spec, we might need Poseidon spec because it integrates with our SSZ hasher. For signature, we'll use Rust library with Zig bindings—so don't need spec right away. First focus: Poseidon spec.

**Participant (Thomas?):** Yep, this is my first target. I'll share the repo as soon as setup with testing team at EF. Will first spec Poseidon; share on the group.

Thomas, one question on the hash scheme: Is there a latest version available? The reference implementation released a while ago couldn't be used out-of-the-box by us because Mercury wasn't available for exposure. We could fork it, but prefer using one from EF team, at least for initial Devnet.

**Participant (Researcher?):** Our official implementation is the one I just shared—maybe you were aware of yesterday. Yesterday we merged the new encoding scheme. Benedict and Dmitry should release an explaining paper soon, probably next week. But yesterday we merged the big PR with the new encoding scheme—so things have changed since last check. This is the official implementation at EF.

**Justin:** So it seems like... I just wanted to share a few thoughts. I'm super happy that all this is happening organically, and almost all points can be settled among the devs. But I'm happy to share some thoughts from the presentation if helpful.

On Devnet versus testnet: Agree Devnet is better—we want to underpromise and overdeliver. One way: Start with proof-of-concept and iterate fast. Multiple axes to simplify: Only leaf signatures, no aggregation; single subnet instead of multiple; no chaining versus with parent; whether or not Three-SF. Start minimal; with fast iterations (every 10 days), incrementally add features.

Question on single vs. multiple clients: Good question. We want effort neutral, open; real value in multiple teams. For example, friendly APIs/bindings to crypto libraries—multiple languages provide perspectives. Good for reference/unit testing infrastructure. We're innovating: Felipe from testing team prepared spec template with best practices from Beacon and EOS.

This is about greasing coordination wheels—having more than one team valuable. That said, be mindful not to waste developer resources, especially existing consensus layer teams focused on short-term objectives.

Maybe one possible outcome: In addition to Ream and Zeam, one additional existing consensus layer team. Valuable: Experience/wisdom to share; production-grade libraries (SSZ, networking)—not too much work; social bridging to avoid silos between new/existing teams.

Another note: Looking like no Beam Day as envisaged, but a whole Beam Week in Cambridge. Idea: In September, workshop where researchers and devs come together in a college for a few days. Accelerate post-quantum crypto testnet and libraries. Also, different foundation architecture: Architecture and consensus teams around whiteboard for rough consensus on finality gadget and fortress role.

Recent development: Innovations in consensus for low-latency (e.g., Alpenglow from Solana). Question: Incorporate for ultra-low-latency bridging/deposits. Potential: Hybrid—ultra-fast path like safe head confirmation (works most time assuming synchronous network, honest majority); slower finality gadget with three rounds of attestations. Consensus team (especially Jan, new) looking into designs. So, post-quantum + architecture/consensus = Beam Week.

Slot duration: Personal preference—pick reasonable number. 12 seconds feels long (proposal to six seconds). Mindset: More aggressive, like four seconds.

Signature killers: Good news—everything constant time; every signature same hashes to verify; aggregates same size/time. Recursive aggregation means attacks/censorship not meaningful. Not too worried, but happy to chat if someone studies.

Other updates: Ton of progress by Emil and Tomah on minimal ZKVM for recursion. Document explains key ideas/design—super minimal, only six opcodes (two add/multiply with different input widths, jump, memory). Powerful/efficient enough—could go that route. Like Chiral but extreme minimality. Boils to fully formally verifying lean sig.

Suggested branding: Base—Lean Seed (one-time sig + XMSS); then Wear as building block; Lean Snark (currently World Away); Lean ISA (six-op ZKVM); Lean Multi-Sig (wraps together). New Lean Ethereum GitHub for high-level specs, efficient/reference Rust impl (hope to formally verify).

### Closing and Next Steps

**Host:** Awesome, very exciting. We're just about at time. It seems like we got a lot accomplished today—going through proposals, getting questions answered.

In terms of agenda for next call: Goal to continue arriving at shared specification for Devnet, or other things to seed the agenda?

**Zeam Representative:** We could push this further, achieve consensus, figure out unanswered questions. So we can come to consensus by end of month on first Devnet spec. Also, if we do, we can share insights from simulator—doing experiments with signature sizes, throughput, aggregators.

**Participant:** On that, one quick update: Quadrillion is the third client team that received a grant from the Firm Foundation.

**Host:** Beautiful. And Mikel, you're on the call from Problab. Anything you want to say about Problab's role?

**Mikel:** We are collaborating with Push Aga Net, monitoring client implementations fully with spec. We could do similar—ensuring everyone on same page—but need to define collaboration for monitoring.

**Host:** Okay, we'll keep an eye on that. One question: Do we have any idea which third team we're targeting? Good to get them early for discussions and viewpoints.

**Justin:** I don't have a strong opinion, nor that there has to be a third. Will has started temperature checks with existing consensus layer teams. Pros/cons for teams—e.g., Lighthouse: Pro, hired for Beam; con, already one Rust client (Ream), so less learning on crypto bindings. But yeah, if Last is here, maybe he could take responsibility.

**Last/Participant:** Yeah, I reached out to a couple, but generally leave it to them whether to join—not cannibalize resources. Demonstrate we want to move forward; on them to be in/out. Be mindful of resources. Even after calls, keep them informed—on us as coordinators. I'll reach out again, share recording/summary, collect feedback.

**Host:** Great. And then the final thing: The standing call moving forward will be Wednesdays at 2 p.m. UTC—so same time, but on Wednesdays.

**Participant:** Okay. You had a question adding a PM repo to the Lean Ethereum org.

**Zeam Representative:** Yeah, responding to a comment: What's the best place to set up shared documents, collab on plans, publish specs?

**Host:** Yeah, I'll work to have some time proposed for the next call on that front. Great. This has been fun. Have a great week, everyone. Thanks, bye.

**Various Participants:** Thanks, guys. Thanks, well, thank you guys. Bye bye.