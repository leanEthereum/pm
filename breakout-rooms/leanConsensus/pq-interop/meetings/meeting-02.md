# PQ Interop - Meeting 02

## Meeting Overview
**Date:** July 23, 2025
**Agenda:** https://github.com/ethereum/pm/issues/1632
**Recording:** https://youtu.be/yZBlCnXRIEY?si=xaEHakUfO5alg498

---
## Meeting Notes

- **PM Organization and Resources**: Introduced the new LeanEthereum GitHub org and PM repo for collaborative documents, meeting minutes, and resources; shared the PQ Interop PM document as a central hub linking Devnet specs, proposals, and contributor lists.
- **PQ Devnet-0 Specification Review**: Discussed the initial PR for Devnet-0 spec based on PandaOps templates; Ream provided feedback on signature parameters (stable at 64 chains/8 length, lifetime 2^32 with activation up to 2^18), slot duration (aggressive 4s), networking (QUIC discussion needed), and iteration sequence (start with chaining/minimal fork choice, then PQ sigs, aggregation later).
- **Team Views Process**: Agreed to have teams provide non-definitive views (strongly support/support/oppose/strongly oppose) on spec subsections for discussion, allowing evolution over time.
- **Cryptography & Library Updates**: Reviewed the "LeanSig for Post-Quantum Ethereum" paper integrating hypercube improvements into the original framework; library updated with serialization for signatures/keys.
- **Gajinder's Discussions Review**: Summarized needs for sig-to-bytes/bytes-to-sig interfaces (now implemented); simplified key format for Genesis generator without full EIP-2335, focusing on public/private pairs as files.
- **Simulator Insights**: Kamil shared preliminary results emphasizing <2s aggregation targets; key factors include bandwidth (>50Mbps recommended), topology (Grid faster than GossipSub 1.1 due to fewer duplicates), subnet size (~668 validators for <2s with Grid), and aggregator ratios (10% sufficient); future work on GossipSub 1.2, QUIC, and optimized strategies like parallel aggregation.

**Action Items**:
- Achieve consensus on Devnet-0 spec (networking, iteration sequence, types/containers) by end of July.
- Set up EF R&D Discord channel for async discussions.
- Discuss/secure help for Kurtosis setup (e.g., from Barnabas/PandaOps) and Genesis generator modifications.
- Finalize peer discovery (whitelisted peers, no full Discv5) and add relevant repos to LeanEthereum org.
- Kamil to share updated simulator report early next week; include GossipSub 1.2, QUIC, and revised aggregation rates (10 snarks/s).
- Invite PandaOps to next call for Kurtosis/Genesis tool support.

---
## Meeting Transcript

### Introduction and PM Organization Review

**Host:** I guess this is the first public PQ interop call. I've got an agenda. I will drop into the chat. I feel like my recordings have a pattern of being a little atypical to everyone else's because I'm always sharing my screen. I'm sharing my screen. I'm going to be trying to do that less so that I keep the gallery view going. All right. There is the agenda. We have a number of items to cover. I guess first and foremost, you know, there's just some links. You know, we talked about just getting some basic project management processes in place for being able to post collaborative documents, keep track of meeting minutes, things of that nature. So we talked about this new GitHub organization, LeanEthereum. Normally, we keep track of all this in the main Ethereum PM repo, but we've created a PM repo within the LeanEthereum org just so that we could have a little bit faster turnaround in terms of like pushing PRs and merging things. But yeah, within that org, you will find a folder at the top, PQ interop. And within there is a primary file that links to the PQ interop PM document, which I just dropped into the chat. And this will just be this is, I guess, on par with what you would find in the main Ethereum PM repo where we try to keep a running link list of links to the different Devnet specifications as they get authored and try to note the dates that they're activated and deprecated and then meeting minutes below that. Just below that, I just transcribed some of the things that we had in that Google Doc, which are just a list of additional resources, original proposal-related lectures and notes. And then some of the LeanEthereum specifications that have been coming together. I also like to get a list of contributors. I'm still new, trying to learn people's names and things. This is helpful for me. I figure it's helpful for others. Happy to add to it as we go. And then I'll kind of maybe hand it off to... O, with this last link that I'll drop from the chat, this is our first working pull request. So evident had a real chance to look at the comments on this, but I just put together a basic template based off of what PandaOps usually authors for a normal Devnet like we've done for Pectra, Osaka, much of other things. So I guess this is like the main objective for the rest of this month is to arrive at a shared specification and parameters for this Devnet zero. I don't know, O or Gajendra, if you want to speak to some of this. I picked up your comments that you had on the agenda issue, you know, just trying to like foreground some decision making and some processes of, you know, if you want to speak to that, it'd be great.

**Ream Representative:** Yeah, maybe I can share this screen and show our review comments just a sec. This is not it. So, yeah, so these are the feedbacks that we have talked inside the team and shared as a single comment. So signature wise, I think we're pretty stable at like 64 hash chains and like eight. I think from the next suggestion, I think it looks like we can go with the max lifetime of two to the power 32 while introducing an activation time, which is basically that the actual length of the actual lifetime that will be generated the keys for up to two to the power 18. So I think that's good. If this is get gender, so that's good. So he passed one on this. Slot duration, our team also think, you know, let's go with the aggressive four seconds number. And then if things get worse and then we can't meet this, then I guess like we can adjust it over time, but I think like going with aggressive number first is probably more beneficial than using a more, more longer time, longer slot time. This is something I think that we can we can discuss a bit more in this call. Basically, I think I still I'm so not so sure what what the values would be out of like using TCP. So if if Kamil can can illustrate that more, I think that would be great. Yeah, I think we can go for web 1.2 instead of like beacon chains 1.1. I think this is also another big discussion that we can take basically from our first call, I think Justin suggested in the last few minutes of the call that we could maybe break down into four iterations or four big iterations. First iteration would be the PQ sig without aggregation second iteration we add a single subnet aggregation. The third iteration we add chaining so basically putting parent root into child block and then add 3SF at the end. We discussed this a bit in the team and we are quite concerned with the timeline for the aggregation scheme. But generally, we agree that we can break down into into like multiple iterations. So instead we propose that we start off with PQ sig with no aggregation. So that's that's as is. But instead of doing aggregation, then maybe we can move up to chaining and some consensus. Maybe 3SF as a second iteration and then aggregation comes as a third iteration. So this one if anyone has some time I think so we haven't we don't have a consensus spec yet, but so we have already kind of like started thinking about like what containers what types are we going to use for the client and eventually we have to agree this across teams. So I kind of feel that we are missing something similar to a consensus spec test where when we when we built our POC we can client that helped a lot because it's basically the baseline of what we have to do in order to to to form the client. So I five code that this lean spec test up. So it's basically kind of like consensus spec test the only difference is that in the beacon chain consensus spec test the test vectors are already in SSZ and already compressed in Snappy. But that for me I think like it's really hard to to actually like understand because like you have to decode and all that. So what I propose instead is that we actually based out of like Python Python code and then only when we agree upon we can actually run a command so that it turns that Python code into SSZ data and then maybe compress it we need to. So hopefully we can then use this kind of format to also discuss about like discuss and also confirm like the types and containers and possibly like the state transition functions if you to confirm them. The rest I guess like the rest are probably creating minor that we can actually discuss in offline. Yeah, so I think most mostly it's up for discussion is yeah the networking and I guess like the sequence of the iterations we want to do.

### Devnet Iteration Sequence Discussion

**Zeam Representative:** So this first DevNet you suggest you would be just the PQ sig generation with no aggregation and then one sets working move on to DevNet one where we would add chaining and the 3SF consensus and then go to DevNet two. So here I would like to differ with Ream I would basically emphasize that we should have chaining from day one itself because if we are sort of voting we need the head that we are voting to and we at least need chaining to run LMD GHOST and come to that head because if we don't have then it that and basically you know there will be more and more bugs and errors to arrive at what was the correct head to be voted at and I think that would waste more time than what we are trying to say over here. So chaining is not really a big deal unless we have that fork choice fork choice writing a fork choice will be a big deal for it but I think it is pretty doable and we should go with chaining when if we don't want to be a start from the get go.

**Ream Representative:** That sounds good. What kind of fork choice algorithm which is suggest just LMD GHOST for 3SF.

**Zeam Representative:** Yeah so 3SF we don't need because basically 3SF is used for justification and for finalization so we don't need to have source and target in the votes we just vote on the head and the head is basically you know whatever is the heaviest tip that that is out there right so have you steep or have you slams right so because there so what what really means is that we would still need to account for validating. We would still need to account for the validators latest votes and sort of have the fork choice where we we alter the weights when we get new votes on the latest cycle and sort of decide what is the next head to be voted on and it is also important because our success criteria is depend upon the head accuracy so. That so we need a correct head and if we don't have that then there is a lot lots and lots of scope for bugs if we don't have 3SF but we don't have this that okay you know we'll have a chain that could be quite for key in the sense that one could try to create an evaluator could come up that can try to create. Fork out of an ancestor which is very old but we are running only 10 days long DevNet so that should not be a problem we can just even keep all the all the states in the head all the states in the memory so there would be no pruning because there is no justified or so we can keep all the states in the memory and since it just 10 days long DevNet may be. We'll be able to survive it but if we are not able to survive it in the first iteration then we can just run it maybe to 2 days or 3 days depending upon how much memory we can survive with so so once we don't have the finalization then basically we can sort of lower the length of the DevNet that we are going with but we still need chaining I think chaining is something that we should not do away with.

**Ream Representative:** Right so we for example we're not going to test sync so we don't define all the stations but when we say for which is here I guess it's be country for which is right.

**Zeam Representative:** Yeah it's it's not really be contain for us in the sense that it is a very very simple fork choice because we have a fixed validator set there are no moving through there is no epoch transitions you don't have any epoch transitions there is a fixed validator set each validator has a weight of one but with respect to the fork choice mechanism that is out there we just run it everything else except the finalization and the justification bits. So we so we have so in the fork choice we need to track the latest votes of each validator and then basis upon that we need to keep updating the score so we so for example Zim already has this implemented along with 3SF but we that you have a proto array like how we have in the beacon and you have the weights attached to it and then basically you know you can do it. You run your update head on each slot to get what is the latest head and basically then what a recording that so that is something I think is the basic mechanism we should not worry with.

**Ream Representative:** Yeah I think it makes sense but I think I think we want to have some sort of spec so that we don't need to we don't have to read each other's implementation to figure out how is implemented.

**Zeam Representative:** Yeah I guess we can work on the spec because it's not going to be difficult it would be it would be quite easy so yeah sounds good we can work on that spec. So we will have a minimal consensus spec so it's not like that we'll have no consensus but there will be a consensus mechanism there will be a straight route there will be straights to maintain because all those are sort of part of running a very basic blockchain and if we for example also have a block by root then we basically have a block by root then we basically have a block by root. So we need to sync the parents to get a chain to finally if if a client for example goes round and is coming back up again because all these kind of things will happen even if we think that they might not. But since you know with this is this happens even in the mature clients so it will definitely happen to the new clients like us and while we are figuring all this out. So then we should read in and we can work on the spec for that which includes the fork choice how we find the head for it.

**Host:** Great, great. Then in your mind would it be like would it be like the first iteration would be just changing with minimized fork choice and then we test aggregation or do you think we better do both of them in the first iteration.

**Zeam Representative:** No, we don't need aggregation in the first iteration because aggregation can come later aggregation is only there to save the block space right it is. I mean sorry we want to have a signature. Yeah, a signature sorry. I can't face. Got it. Yeah, so what we can do is we can have devnet zero just with chaining and then we can do the devnet one with the signatures we can we can do that because that would sort of help us zero down to any issues that that are there so that will be fine because in the meantime you know we'll also get our signature libraries ready and all that so that sounds like a good plan.

**Host:** So am I writing down correctly that first iteration is chaining with minimum fork choice and signature will only come in the second iteration and then aggregation after. I thought it was part of this.

**Zeam Representative:** Right so so chaining and minimal fork choice you can say it's an iteration zero it's like you know we are just testing our clients out even before that yellow signature devnet is happening so that we are basically we don't we agree on that particular chaining spec and we don't run into those issues so I mean it's like invisible iteration you can say that. But yes, I mean the idea would be to get over this particular iteration quite fast. So let me quick note on that I think I think your last point could you there was important like if we can do this pretty fast and I think it makes sense but if we are planning for this to be by end of the year and only start doing pq sig next year then that's probably I would. I would challenge that but I think chaining yeah I totally agree with you are chaining we should. We can try even by next month and right so we already have it so we just need to agree on a spec and what we what we need to do is we need to undo the 3SF that is not really a big deal. True undo because it's like okay you know we it's like stop having 3SF and so it's quite easy for us so the timeline is totally dependent upon what Ream is comfortable with but I think we can do it by end of next month we can start to have an interop and it will be a good thing because we would sort of also get a good feel about interopping with each other. So I think with that we can probably still aim for it having pq sig devnet by by say DevCon in November so I think that's that's great.

**Ream Representative:** Cool yeah and then I think I think those are my two big discussion topics. Jihoon.

**Jihoon:** Yes do we know if we can help from the test team about Kurtosis or doing it to work on that on our own.

**Zeam Representative:** Yes I can start with my understanding. So as per my understanding Barnabas is quite strong in Kurtosis and in writing Starlock code and I think we will definitely need to cut with them unless we have a Starlock expert in this particular group but eventually I think yes we'll have to own and sort of on on it and sort of run with it once we have the initial help from Barnabas. Most of I think most of it would be regarding the Genesis generator tool that is where I think we will need to sort of begin and help them out because they won't be able to generate the Genesis state for us as well as as well as generating the public private key pairs right so so this is so so basically Genesis generation is the. Key step over here but after that I think we can get some decent help from the PandaOps guys and maybe involve them as well from the get go.

**Jihoon:** That was great I think it would facilitate our progress a lot.

**Zeam Representative:** There are other I will need some you know Genesis generator. So sorry just Ream also what I'll do is we'll look at the Genesis generator tool and basically we'll sort of discuss what the gaps are and maybe see how we can fulfill them I think that is a starting point. Then we I can in the meantime also talk with Barnabas I think Will might also have a conversation and we can talk about how we can do this with Kurtosis.

**Host:** Great Benedict I had dropped a or added a note to the agenda about if you wanted to provide any updates or just overview on the paper that you dropped a link to the LeanSig for post quantum Ethereum I don't know if there's any technical up there. So I think it's on that or relevant libraries that you're anyone from the cryptography side want to touch on it can just give an overview of what it is.

**Benedict:** So we had this first paper hash based signatures for post quantum Ethereum right and then after that we had this follow up research that appeared at crypto at the top of the hypercube with just just an improvement of sort of the trade of curve between signature. So the first paper was a very precise and verification hashing and yeah so the problem was that this thing was in a very written in a very textbook way so it was not really the language that we would use in the in the first paper. So it was more like a textbook kind of thing and not like a practical thing so now we translated that into the language of the first paper and integrated it into the library. Yeah, it's essentially just that so that the document is just taking the ideas from the second paper and integrating it into the framework of the first paper to make sure that everything works there as well and then yeah we updated the library as well. So now you can sort of use the new scheme. Yeah, that's basically it for that document.

**Host:** Great, thank you. And then Gajendra I dropped a note on the agenda you had mentioned maybe wanting to summarize a discussion that you had with Bhaskar & Tamaghna. Need for a sig to bytes and bytes to sig interface for the Rust library.

**Gajendra:** Yeah, I think these are just small logistic things but I guess we need I didn't see the interface for that from signature to bytes and bytes to sig. Yeah, so that need. Like half an hour ago, we implemented or Han actually implemented First, the user wants a document for "meeting 2" in the same format as meeting 1, output in a markdown code window.

The format of meeting 1 is:

# PQ Interop - Meeting 01

## Meeting Overview

**Date:** July 18, 2025

**Agenda:** link

**Recording:** link

---

## Meeting Notes

- Bullet points summarizing key discussions.

**Action Items:**

- List of action items.

---

## Meeting Transcript

### Sections with speakers.

The transcript provided is for meeting 2, dated July 24, 2025 (current date).

I need to clean up this transcript similarly: note speakers, add section headings.

Structure:

1. Meeting Overview: Date July 24, 2025. Agenda: The one I polished earlier? User provided an agenda in previous message, but for this, infer from transcript.

   Agenda was: https://github.com/ethereum/pm/issues/1631 (from transcript, but adapt).

   Recording: No link given, so perhaps TBD or omit if not available.

2. Meeting Notes: Summarize key points in bullets, like in meeting 1.

3. Action Items: Extract from discussion.

4. Meeting Transcript: Cleaned up with sections, speakers.

Key points from transcript:

- Discussion on PM repo in leanEthereum org.

- Review of PR for devnet-0 spec.

- Ream feedback on spec: signatures stable, slot 4s, QUIC/TCP discussion, iterations (chaining first, then consensus, then aggregation?).

- Proposal to start with chaining + minimal fork choice (no 3SF initially).

- Invisible iteration 0 for chaining.

- Help with Kurtosis from Barnabas/PandaOps.

- Crypto update: New paper integrated.

- Sig interface implemented.

- Key format: Keep simple, not EIP-2335.

- Simulator insights from Kamil: Aggregation times, bandwidth needs (bump to 100Mbps+), grid vs gossipsub, etc.

- Peer discovery: Static peers, no discv5.

Action items: From transcript, e.g., finalize specs, help with Kurtosis, etc.

Date: July 24, 2025.

Agenda: The polished one from before: # PQ Interop Meeting Agenda - July 23, 2025? Transcript says July 24? User says current date July 24, but agenda was for 23? Perhaps adjust to 24.

Transcript starts with "I guess this is the first public PQ interrupt call." So, meeting 2 on July 24.

No recording link in transcript.

Now, clean transcript: Identify speakers.

From transcript:

- Host (probably Will): Shares screen, agenda.

- Ream rep (Unnawut/O?): Shares feedback.

- Gajinder (Zeam): Responds.

- Justin? Mentioned but not speaking.

- Mercy? Hand up.

- Benedict: Crypto update.

- Gajinder again on keys.

- Kamil: Simulator.

- Questions from chat/others.

Sections: Introduction, Review PM, Review Spec PR, Crypto Updates, Discussions on Interfaces, Simulator, Peer Discovery, Closing.```
# PQ Interop - Meeting 02

## Meeting Overview
**Date:** July 24, 2025
**Agenda:** https://github.com/ethereum/pm/issues/1631
**Recording:** [TBD - Link to recording]

---
## Meeting Notes

- **PM Organization and Resources**: Reviewed the new LeanEthereum GitHub org and PM repo for collaborative documents and meeting minutes; shared the PQ Interop PM document as a central hub with links to specs, proposals, and contributors.
- **PQ Devnet-0 Specification Review**: Examined the initial PR for Devnet-0 spec; Ream feedback included stable signature parameters (64 chains/8 length, lifetime 2^32 with activation to 2^18), aggressive 4s slots, QUIC/TCP discussion, GossipSub 1.2, and iteration sequence prioritizing chaining + minimal fork choice first (as "invisible" Devnet-0), then PQ sigs, aggregation later.
- **Team Views Process**: Confirmed teams will provide non-definitive views (strongly support/support/oppose/strongly oppose) on spec subsections, allowing changes as progress is made.
- **Cryptography & Library Updates**: Summarized the "LeanSig for Post-Quantum Ethereum" paper integrating hypercube improvements; library now supports serialization for signatures/keys.
- **Gajinder's Discussions Review**: Addressed sig-to-bytes/bytes-to-sig interfaces (recently implemented); opted for simple key format in Genesis generator without EIP-2335, using public/private pairs as files.
- **Simulator Insights**: Kamil presented preliminary results targeting <2s aggregation; highlighted bandwidth needs (>50Mbps), Grid topology outperforming GossipSub 1.1 (fewer duplicates), optimal subnet sizes (~668 validators), and aggregator ratios (10%); future additions include GossipSub 1.2 and QUIC.
- **Broader Outcomes**: Agreed on minimal consensus spec for chaining/fork choice; proposed static peer discovery (whitelisted peers, no Discv5); plans for Kurtosis/Genesis tool support from Barnabas/PandaOps; EF R&D Discord channel setup for async discussions.

**Action Items**:
- Achieve consensus on Devnet-0 spec (networking, iteration sequence, types/containers, lean spec tests) by end of July.
- Set up EF R&D Discord channel for async conversations.
- Secure help for Kurtosis setup and Genesis generator modifications (e.g., from Barnabas/PandaOps); discuss gaps offline.
- Add relevant repos (e.g., spec, spec tests) to LeanEthereum org.
- Kamil to share updated simulator report early next week, incorporating GossipSub 1.2, QUIC, and revised aggregation rates (10 snarks/s).
- Invite PandaOps to next call for tool support; teams to provide subsection views before next meeting.

---
## Meeting Transcript

### Introduction and PM Organization Review

**Host:** I guess this is the first public PQ interop call. I've got an agenda. I will drop into the chat. I feel like my recordings have a pattern of being a little atypical to everyone else's because I'm always sharing my screen. I'm sharing my screen. I'm going to be trying to do that less so that I keep the gallery view going. All right. There is the agenda. We have a number of items to cover. I guess first and foremost, you know, there's just some links. You know, we talked about just getting some basic project management processes in place for being able to post collaborative documents, keep track of meeting minutes, things of that nature. So we talked about this new GitHub organization, LeanEthereum. Normally, we keep track of all this in the main Ethereum PM repo, but we've created a PM repo within the LeanEthereum org just so that we could have a little bit faster turnaround in terms of like pushing PRs and merging things. But yeah, within that org, you will find a folder at the top, PQ interop. And within there is a primary file that links to the PQ interop PM document, which I just dropped into the chat. And this will just be this is, I guess, on par with what you would find in the main Ethereum PM repo where we try to keep a running link list of links to the different Devnet specifications as they get authored and try to note the dates that they're activated and deprecated and then meeting minutes below that. Just below that, I just transcribed some of the things that we had in that Google Doc, which are just a list of additional resources, original proposal-related lectures and notes. And then some of the LeanEthereum specifications that have been coming together. I also like to get a list of contributors. I'm still new, trying to learn people's names and things. This is helpful for me. I figure it's helpful for others. Happy to add to it as we go. And then I'll kind of maybe hand it off to... O, with this last link that I'll drop from the chat, this is our first working pull request. So evident had a real chance to look at the comments on this, but I just put together a basic template based off of what PandaOps usually authors for a normal Devnet like we've done for Pectra, Osaka, much of other things. So I guess this is like the main objective for the rest of this month is to arrive at a shared specification and parameters for this Devnet zero. I don't know, O or Gajendra, if you want to speak to some of this. I picked up your comments that you had on the agenda issue, you know, just trying to like foreground some decision making and some processes of, you know, if you want to speak to that, it'd be great.

### Ream Team Spec Feedback and Iteration Discussion