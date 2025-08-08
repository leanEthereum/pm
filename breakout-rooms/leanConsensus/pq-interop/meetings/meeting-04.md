# PQ Interop - Meeting 04

## Meeting Overview
**Date:** August 6, 2025  
**Agenda:** https://github.com/ethereum/pm/issues/1663
**Recording:** https://youtu.be/WU6y0sZv_i8  

---
## Meeting Notes

- **3SF Mini Implementation Updates**: Reviewed pull request for 3SF mini adjustments (e.g., removing optional fields, changing string hashes to byte32 for SSZ compatibility). Client teams urged to review and comment. Discussion on aligning with Ethereum types library (used in execution specs) for consensus specs; plan to connect with Sam Wilson for further alignment.
- **DevNet-0 Spec Open Items**: Continued discussion on peer discovery (favoring static peers in Genesis), block/attestation inclusion, and placeholder SNARK data. Fixed peers preferred for simplicity in DevNet-0; placeholder data discussion to be resolved offline.
- **Spec Repo Progress**: Poseidon2 subspec merged; repo setup progressing well. No major updates; team to continue iterating.
- **Communication Channels**: No update on Discord-Telegram bridge setup (carried over to next call).
- **Broader Outcomes**: Short call due to key participants’ absence. Focus on finalizing 3SF mini and peer discovery decisions.

**Action Items**:  
- Client teams (Zeam, Nimbus, Lantern, etc.) to review 3SF mini PR (https://github.com/leanEthereum/leanSpec/pull/5) and provide feedback.  
- Connect with Sam Wilson to discuss Ethereum types library integration (Will/Felipe).  
- Offline discussion on placeholder SNARK data and peer discovery finalization (Ream/Zeam).  
- Carry over Kurtosis/Genesis support update to next call (Barnabas/Gajendra).  
- Set up Discord-Telegram bridge and confirm deprecation of old Telegram groups (Will).  
- Circulate meeting minutes and recording (Will).  

---
## Meeting Transcript

### Introduction and Greetings
**Will:** Welcome to the weekly PQ Interop call. The goal of this call series is to serve as a touch point for researchers and client devs during the iterative process of developing, deploying, testing, and improving a series of devnets focused on aggregate and post-quantum signatures. This breakout room also supports the longer-term lean consensus vision. Let's jump in.

### 3SF Mini Implementation Discussion
**Will:** Oh, I know that we've got a kind of a, the basic agenda I saw that you added a very nice comment underneath it with some detailed information that you like to go through regarding the three SF mini implementation. We could start there if that works for you.

**Ream:** Yeah, sure. I can put my pull request here in the chat (https://github.com/leanEthereum/leanSpec/pull/5). So basically, I think this is relevant to Zeam and also if there's any other client teams that are in this call. So basically from our side of things, we just started trying to implement the three SF mini into our client. And there were some discrepancies that we have to make against the three SF mini that Vitalik wrote mainly so that it supports the basically compatible with SSZ and serialization. So for example, like some of the stuff, like there are some optional fields in three SF mini in Python implementation. There is no optional field possible inside SSZ. So we had to remove that as well as some other, like for example, like hashes that use string in the other original implementation. We changed it to byte32 and stuff like that. So if anyone is interested, I think the PR is up, feel free to comment in there. Yeah, and I think this is quite important in terms of interop because we'll need to agree on all these fields along with their types in order to actually be able to drop. So yeah, I guess like my message is just that.

**Will:** Great. So if people could make a point to review that pull request, I don’t know if anyone from Zeam or Nimbus or Lantern or any other participating client teams are on the call. What are asking a question? Do you have any comments or anyone else?

**Zeam:** Yeah, so Gajindra is the one who worked on our implementation of three SFs. I'm not very familiar with the details. We implemented something like a few months ago. I mean, he did. I know that our library supports optionals. So I think, yeah, we would like to keep it this way if possible. I don't think it's a big deal to implement optionals this day, although I know Ethan from Nimbus is trying to put for a different option now, which is stable containers. But yes, for them concern, I would like to stick to the, to the spec as much as possible so that we don't have, you know, I mean, of course we're happy to discuss, but I think optionals are not that hard to implement. So I'd rather have that than, you know, jumps through hoops to not do that.

**Ream:** I see a note. I can have a look at optional and then give us, give you our feedback sometime later.

**Will:** Thanks, Kim. Oh, maybe a question for, for Felipe, if you're in the call. So I think your comments said that I should be using the diagram types, which is used by execution specs. So I'm wondering if that's like the kind of like direction that the consensus specs will also be moving towards that. Plus, I'm not so sure if there's any discrepancies. For example, like execution layer uses RLP for encoding, right, but consensus uses SSZ. So I'm not so sure if it's like fully compatible. To use it through types for consensus specs.

**Felipe:** Yeah, yeah, that comment was, I mean, wherever it makes sense to you, I think that library is still being built out as well. And Sam would be a good contact there. So yeah, if it makes sense to. Sam Wilson. Sam Wilson, yeah.

**Will:** Okay, cool. Yeah, if you're not connecting with Sam, I can put it in contact.

**Ream:** Yes, please.

**Will:** So I guess the idea is to try to merge both like the consensus and execution into using the same Ethereum types library, right?

**Felipe:** Yeah, so I think the goal for them to create that, they move that out of the execution specs into its own library. And I think just when it makes sense to, but specs to use the same types was my only comment. But obviously if it falls short of your use cases, then perhaps expanding on that library and adding the types there would be a good, a good first step and then importing those types into the lean specs. If that makes sense to you.

**Ream:** Okay, so that's it. I'll look at you at the types.

**Will:** Okay, yeah, and I can get you in touch with Sam as well, I think.

### DevNet-0 Spec Follow-Up
**Will:** The other items that were on the call one. Agenda and Barnabas, we're going to provide an update. Neither of them are on the call. So we can carry that over to the next week if need be. The other two, there is a follow up discussion that people, you know, I noted that people wanted to have regarding the DevNet zero spec. Some of the items that I believe remain open for discussion were finalizing peer discovery, static peers, block attestation inclusion, we talked about the three SF mini modifications. Oh, and then placeholder SNARK data. I don't know if. If Ream or Zeam, if maybe one of you had anything that you wanted to discuss regarding peer discovery. And this may have been already finalized async.

**Ream:** Yeah, the no, I basically also raise this concern. I think on one of the calls that yeah, there is no clear. The same you know how we're going to do this considering that there is no execution client. But yeah, basically we can just add everything in genesis. I didn't know all the peers for. If needed, I can try to think more about that. Yeah, maybe specify somehow.

**Will:** Okay. And then placeholder SNARK data. So I want to explain that to me. What's needed and how to resolve that conversation.

**Ream:** Could you repeat SNARK data? What's that?

**Will:** The same way that we're looking, I guess, for a proxy for peer discovery with no execution. I had another note that people were discussing the need for placeholder SNARK data.

**Ream:** Yeah. I don't know. Last time we raised this discussion. I think Gajindra had some alternative opinion about that. But yeah, if we can, in my opinion, you can do this kind of placeholder. Yeah, how can we move forward to that?

### Spec Repo Updates
**Will:** Okay. And then were there any other updates regarding the spec repo? So, Felipe, I don't know, added note here, you and Tamaghna had a discussion of our regarding the Poseidon2 progress.

**Felipe:** Yes, sorry. What was the question? I guess any updates on setting up the spec repo or anything that you needed to slide for repo?

**Felipe:** No, no, I think it's been proceeding pretty well. The Poseidon2 subspec was merged. And I think maybe if anything, Tamaghna would have a little bit more to say about it. But I think it's in a pretty good spot to kind of hit the ground running.

### Peer Discovery and Wrap-Up
**Will:** Okay. Any other items? I know, yeah, Tamaghna, Gajindra out today. This may just be a short call. That was all I had on my agenda. Maybe getting back to the previous cover topic just so do we want to have like fixed peers participating in the DevNet or we want to kind of be able to add new peers when we want or any assumptions that we have in that regard maybe any ideas.

**Ream:** If it's just a fixed set of peers then yeah, maybe just adding them from the Genesis block course and spec file should be okay. I suppose the peer removing parts for DevNet is zero or the better. So yeah, plus one to fixed peers.

**Zeam:** Yeah, that's too busy.

**Will:** Okay. Well, last call that I think that this probably concludes this for this week. Great. Thank you all. I'll post the recording and notes for later.

**Participants:** You guys. Thanks. Bye.