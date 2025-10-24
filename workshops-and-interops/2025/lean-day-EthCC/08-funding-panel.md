# Lean Day - Panel Discussion on Funding and Support

| Item | Details |
| ---- | ------- |
| **Session** | Panel Discussion - Funding, Support, and Sustainability for Beam Chain Development |
| **Speaker(s)** | Barnabé (EF), Tomasz (EF), Cheeky Gorilla (PG), Mario (EPF), Pat (Pier Two) |
| **Slides** | no slides |
| **YouTube** | [link](https://youtu.be/VC758MCbN_Y?feature=shared) |

**Moderator**: Justin Drake (Protocol Architecture, Ethereum Foundation)

**Panelists**:
- **Tomasz** (Co-Executive Director, Ethereum Foundation)
- **Barnabé** (Protocol Architecture Team Lead, Ethereum Foundation)
- **Cheeky** (Protocol Guild)
- **Mario** (Ethereum Protocol Fellowship)
- **Pat** (CEO, Pier Two)

>back to: [Lean Day Overview Page](/workshops-and-interops/2025/lean-day-EthCC/00-index.md)

---

## Opening Questions

### Question 1: Protocol Guild Support for Beam Chain Teams

**Tomasz**: Next up we have a panel discussion and then we'll go for lunch. So on the panel we have Tomasz, Barnabé, Cheeky from Protocol Guild, Mario from EPF and Pat from Pier Two. And I guess this is an opportunity for anyone to ask questions, but I'll just kick it off with a question that I've been asked. I guess this one's addressed to Cheeky, which is: **Can the Beam teams expect to be supported by Protocol Guild and if so, what is the process and story?**

**Cheeky (Protocol Guild)**: Diving right in, alright. Yeah, so the answer is **it's complicated**. So in a way I could answer and say we kind of already do, right? Yourself and other members here are Protocol Guild members, are benefiting from this mechanism, but then I know there's a lot of you that are focused on the client implementations that are not eligible.

And what we basically need to assess from Protocol Guild's perspective is how do we expand our **eligibility framework** to account for Beam chain client development in a way that doesn't open us up to capture in other ways, right?

#### Current Eligibility Framework

And so maybe just to take a step back. So today, I think all mainnet clients are eligible for Protocol Guild membership and they're specifically named in our eligibility framework, but we do have this caveat that to be eligible for membership, you need to have **technical diversity**—basically not just a clone of someone else. You need to be producing your own blocks, mainnet ready.

And so these kind of things would currently make the Beam chain clients **ineligible**, right? It doesn't mean we don't think that you are fantastic core devs. It means that as I said, we do need to expand our eligibility framework.

#### Challenges and Considerations

But of course, we want to do that in a way that's mindful of a few things:

**Number one** is quite frankly, our **funding levels are also far lower than we would like them to be**. If we onboarded all of you into Protocol Guild today, you would not be satisfied with the financial incentives, but that's not your fault. The onus is on us, the core team of Protocol Guild, which includes myself, Trent, Van Epison, and Peter to basically fundraise more. So that's one challenge for us.

And then the **second challenge** for us is that we have made it more difficult for client teams to join Protocol Guild so far. Because we want to make sure that they can't just have someone who just forks Geth and gets the salary from Protocol Guild. And we're also mindful of the fact that **more clients add testing overhead** as we all know, right?

And so we're actually currently discussing with the membership about **capping the number of client teams**. And this will be a tricky discussion. There's a bunch of ways that this can be done, but this gives you an insight into basically how we're thinking about this.

#### Path Forward for Beam Chain

And so fundamentally bringing it back to Beam chain—threat to you guys is that from a **research perspective**, the things that can ultimately impact the execution layer, we already today consider that eligible work. So if you are doing that kind of work and you are not a Protocol Guild member, maybe talk to Justin to interrupt us or talk to me directly. And then we can see if that's actually the case.

If you are a **client team**, we basically have to have more discussions internally and externally, which means with the client teams, for how we can expand our eligibility framework in a way that will not open us up to capture in other ways.

And I guess specifically because Protocol Guild is not operated in a unilateral way. We need to have **buying from the entire membership**. And one thing that we have heard from Protocol Guild members today is that they would really like to consider Beam chain clients. They would have to have more clarity on **what is the timeline for eventually bringing Beam to mainnet**.

And of course these things are very tough. But hopefully this gives you context on the battles that we have internally, both from the fundraising perspective, both from a capture perspective. Because of course we do hope to raise our fundraising by orders of magnitude, at which point the financial incentives could increase the governance overhead of Protocol Guild to a level that is completely unsatisfactory for the mechanism.

#### Summary

So these are the kind of dilemmas that we have. And so again, fundamentally for the group here, for your peers, **if you're working on research, you should be eligible**. **If you're working on a client team, you're probably not eligible at this point**. But please work with us so that we figure out what is the correct wording to make sure that Beam clients can be eligible, even probably before they hit mainnet, but perhaps once the timeline for this has been solidified a little bit better.

---

### Question 2: Ethereum Foundation Strategic Priorities and Beam Chain

**Tomasz**: Wow, very detailed answer. Thank you so much. I'll ask my second question and then I guess we can open it up to the group. This one's targeted to Barnabé. So on **June 2nd we have these new strategic objectives: scale L1, scale blobs, improve UX. And the Beam chain doesn't fall within these short-term objectives. How do you think about where the Beam chain fits and how does this impact funding?**

**Barnabé (EF Protocol Architecture)**: Yeah, great question. Okay, so I think what's helpful maybe is to give a bit of context as to why we went with that model of the reorg.

#### Context: Post-Merge Alignment Challenges

And I would start actually three years ago when we shipped **the Merge**. I think it was like a flashpoint in the ecosystem, but also in the existence of the EF and the way that things were organized at the EF. I feel like that was the place where we had almost the most alignment between our research resources, our engineering resources and the community. Like we were all aligned on "this is the one thing that we really need to ship."

**After the Merge it was a bit less clear**. I think the research resources were keeping on that momentum, so we were more focused perhaps on the consensus layer, trying to deliver more ideas around different consensus models, thinking about that from the consensus layer perspective.

Scaling—we had this narrative that since we have the rollup-centric roadmap, rollups are the ones that are supposed to scale, so we're going to try and provide them with a bit more resources in the form of blobs.

And over time I feel also that the community was raising more and more issues that we weren't really paying attention to. So the alignment in general felt a little bit off.

#### The Beam Chain Vision vs. Community Priorities

And I guess the first idea that came to fix that problem was: well, the Merge was so successful at aligning everyone. So **let's try to synthesize again what the Merge could look like**. I think this was one of the first pitches of the Beam chain as Justin was proposing it. I feel like it has evolved a little bit from that. So I'm not saying that this is still the case today.

But yeah, you was really to say let's try to rally behind another big project that aligns research, aligns engineering, aligns the community. And I think it's very powerful in principle and does have positive things.

But part of what the community told us after that was: "okay yeah this is good, we want fascinating—we want all the things that are on the Beam chain bucket list. But also we've seen the Merge taking so long. There's a bunch of things that we also really really want that we think you guys need to focus on that are equally important perhaps as everything that's on the Beam chain docket **but more urgent to us to deliver**."

And these things are **scaling the L1, scaling the blobs and improving the UX** as I understand it.

#### The June 2nd Strategic Announcement

And so with our announcement on June 2nd, I think the idea for us was to say: "**We've heard that. We've heard how urgent, how important, how we need to deliver and execute perfectly on the path of shipping these three things**."

And that seemed to say, "well what about the things that are not these three things?" And that's another part which I think is interesting—which is the EF has never been extremely explicit, and sometimes we don't really say exactly what we're working on or not working on. And with this step that we took in June, it was kind of unprecedented to say—to make extremely explicit the things that we are like "okay this is extremely urgent for us to do" and leaving implicit the rest.

But it almost actually shined a light on the things that were not in that announcement, such as Beam chain.

#### Framework for Understanding Priorities

And so now to me the right way to think about it is to say: **we have our first-priority strategic priorities**. Another thing that we have heard that was kind of missing from the announcement—and that we are working on how to fix—is to say "what about security of the network in that context?"

So the first thing we want to say is: these three strategic priorities, we always operate within the framework where we never destroy the network—**we always have 100% uptime**. So that's the first thing we need to make clear.

And the second thing is: **these three strategic priorities doesn't mean that these are the only things that are important to us**. And thinking about the transition to post-quantum security, thinking about next-gen censorship resistance, thinking about next-gen consensus—these are things that are also important and that we need to be able to keep allocating resources to at the EF and outside of the EF.

And so I think that to me that's kind of the direction that we're heading. And in **mid-2026** we have an announcement that I hope will go more into details around this thinking.

#### Balancing Urgency and Innovation

But yeah, so sorry for the stress that the initial announcement caused. I think we still realize that these issues are important, but we don't really want to emphasize that these three things—scaling L1, scaling blobs, improving UX—we've heard so much that they are **extremely urgent**. They are top of mind for so many people that are users of Ethereum, programmers, businesses building on Ethereum. And we really need to show that we are going to deliver on these three things.

---

### Question 3: Client Business Model and Expectations

**Tomasz**: Thank you, another very detailed answer. Any questions from the audience? Okay, I guess I'll ask my third question then. The first one's for Pat who is the CEO of Pier Two. From my perspective you're pretty unique in the sense that you really have an existing business and you've been funding so far completely out of pocket development of your client, Lamb. I was wondering like **what are your expectations as a client from the future and what is your take on like some of the answers that we're given by Cheeky and Barnabé?**

**Pat (Pier Two CEO)**: Sure, so maybe just a quick introduction. So **Pier Two** we have been doing a lot of staking as a service, so working with exchanges and funds and custody providers. Possibly the staking model—if Justin has his way won't be too viable in the future—so we might be looking at a few other things.

But in terms of looking at the future and looking at the Beam chain, we're really fortunate that we have a team that just really loves Ethereum. So contributing in the open source space has been a pleasure in that sense, but as well as that it also has the benefit of being able to become really informed on the topic of staking and sort of providing the customers that use our services with a lot of security within thinking about the future of Ethereum and the future of how we can build out our setups and so forth.

So I think that's an added benefit when it comes to things like Protocol Guild and client funding and **client diversity**. I think it's really important that the Ethereum Foundation and the ecosystem isn't wasting time and isn't wasting money generally. There's a lot of incredible teams that are contributing to the space in an incredible way, but **we can't have 20 clients**—there's the argument that you don't need too many more than three or four or five possibly.

So making sure that you really strike that balance has been something that I think Ethereum today has done quite well. And I don't envy being in Cheeky's position of having to sort of choose who is actually really valuably contributing to Ethereum and sort of setting those standards for what funding looks like.

So yeah, interested to see how it all evolves, but **striking that balance between appropriate resource allocation** and having the appropriate decentralization properties that you get from client diversity is a challenge, and I look forward to seeing how it's solved.

#### Follow-up on Optimal Client Count

**Tomasz**: Actually I have a question for you first, but then I'll go to Mario. So I was actually curious on your take on this point which is: **what do you think is a good number of clients and do you think that the clients are competing with each other** where at some point there's negative externalities? Or do you see that the design space for Ethereum is extremely vast, especially if Ethereum becomes the internet of value, and there's all sorts of opportunities for businesses around the clients and that maybe 15 clients is not so crazy after all?

**Pat**: [Response appears to be cut off in transcript]

---

### Question 4: Ethereum Protocol Fellowship as Bootstrapping Mechanism

**Tomasz**: Let's go to Mario. Let's ask a question for you. So you're the head of the **Protocol Fellowship program** and I guess we have a few people here in the room from the fellowship program. Do you think the program can be used as a **bootstrapping and onboarding mechanism** for people to eventually become devs or maybe even for existing devs that need financial support in the short term?

**Mario (EPF)**: Yes, thank you, and it's exactly the goal. So **EPF**—I hope that most of you are familiar with it—and yeah there are a bunch of fellows right here in the room so that's great, it works.

And yeah, our goal is to **empower individuals but also help the client teams**. So we are making it easier for people to start working on core development by financially supporting them but also creating community, connecting them to the right people, providing guidance to work generally on core, which means all client teams.

#### Beam Chain as a Catalyst

But actually, like Justin—so I've been doing this for a couple of years now, going around these conferences inviting people to come through to the protocol fellowship. But **you've done the best job when you announced Beam chain**. There were so many people excited, interested. The Twitter and the DMs—I believe your email that you've published really got so many people now interested in working on the core protocol.

So I think that's interesting—much easier that Beam being so novel, so exciting, really brings more people to start working on the protocol. Maybe they end up working on the Beam chain probably, but they also start working on more of the core things.

#### Current Cohort and Beam Chain Interest

And right now we started **Cohort 6** like two weeks ago. There are 15 fellows or something like that here. I can see a couple of them right here from the current cohort, and we have **maybe five people looking into Beam projects** because they're exciting.

And I think it's especially with new clients—new Beam clients starting—it's a great way to help people, to support them on the journey to start contributing to these, to start building them. And it's also because it's in this space where it's sort of prototyping—it's not too critical. For example, like if you said "okay, or something that's not a place for somebody being new to work on," that's much more critical, it is much more priority.

But **Beam chain being in this design space where people are still prototyping and figuring out how it works**—I think it's a great place to learn, to actually try their skills and contribute.

#### EPF vs Protocol Guild

Yeah, and we provide some financial support for people, but I would say that we are in contrast with Protocol Guild in a sense that **PG is for established core developers** who have been contributing for some time—six months, a year or so—and we are on the other spectrum. If you want to get started and you have the—we believe that you have the talent, the actual skill to get into this—we will guide you on the journey and provide the help.

And it helps the client teams themselves because then it's much easier to get contributors. It's not just the financial part of it, but the whole **onboarding process**. Especially a new team which doesn't have a clear onboarding process at all—just having a contributor who starts working with them and gets the guidance, and eventually it's much easier for them to become part of the client team.

---

## Community Questions

### Question 5: LibP2P Funding and Sustainability

**Audience Member (LibP2P)**: So in the interest of sustainability talks, the **LibP2P project** has been a growing element in the Ethereum ecosystem. It was a little bit in the beacon chain—the Beam chain is all about it. The energy coming into the LibP2P project has gone through the roof since the Beam chain project has launched.

So my question is: **how do you guys see us funding something so critical but also not owned by anybody?** Now that it's an independent open source project, the grants programs, the education programs, everything like that is all funded from the community, and it's not exactly clear how we plan to sustain that going forward.

Right now it seems to be "go get a job in one of the client companies so you can get paid to work on P2P"—that seems to be our only answer at this point. But as Kamil pointed out, the success of the Beam chain is requiring going so deep into the stack that we're doing scaling experiments on GossipSub, all that kind of stuff.

So I always ask this question every time I come to these conferences because in the end I'm usually the person who hands out the money on the grants. So **how do you guys see us funding LibP2P in the long term?**

#### Protocol Guild Perspective

**Cheeky**: I'm not technical, but I can speak briefly to this. From Protocol Guild's perspective, if we ask people about **LibP2P eligibility for Protocol Guild** and they deeply know LibP2P, it seems like a **50-50 split** if they should be part of it or not, right?

And so that's difficult because we operate as a DAO—how are we going to vote on that kind of split? However, it is indicative of a larger issue that we at Protocol Guild have, which is that there's a lot of future changes coming with Beam chain. And Beam chain is introducing—because it's not incremental—it's kind of stuffing a bunch of **new stakeholders** who could be eligible, maybe not, depending on things like block production.

Think about all the stakeholders involved in that today versus four years from now—could be completely different. And so we need to think deeply about all of these things. And this is again—this is not something that myself or Trent are experts in. We need to have input from people like you, people from the membership already, to see how can we make Protocol Guild more future-proof to account for these things.

So that's the core dilemma—they're doing amazing work that we want to fund, that really are part of the core protocol in a very meaningful way, that they should be getting funding from Protocol Guild. But yeah, I think there's no easy answer as it stands right now.

#### Current Funding Reality

**LibP2P Representative**: We just compete with you directly. We go in and sign up for the same grant programs you do. The big one was Optimism—our RetroPGF 3—we were the fifth largest vote getters, you guys were like number one or two.

So yeah, there's like a dev tooling category which has spun up. And do you fit better into that? So far the only people been able to get funding for LibP2P now since they have a broader scope is specifically **client applications**. The only group that is not tied directly to a client would be the IP Shipyard team, and they because they were the core maintainers from Protocol Labs they were able to get some funding.

But the **community function in general**—like myself where I run the LibP2P days and coordinate a lot of the research amongst the teams and keep things going—I was told repeatedly "maybe the next round, maybe the next round, no wait you're not eligible at all."

So at the end of 2025 we're looking at basically like **there is no community function, there's no meetings, there's no gathering, there's no central watering hole** for all the developers working on this stuff. And we actually are kind of right in line with the whole diversification stuff because we have what, 10 implementations in LibP2P now with the new C LibP2P coming up, which is awesome.

#### The Coordination Challenge

Yeah, so it's very unclear how this is going to continue. For myself, it's really just going to have to fall onto several of the companies that have money coming in for other reasons.

**Cheeky**: If you were to give us something in writing that helps break this down—how you see it—and then we can disseminate amongst the membership for consideration, that would be very helpful.

**LibP2P Representative**: Absolutely, absolutely.

#### EF Networking Team Response

**Barnabé**: Yeah, I don't have a specific answer on funding, but the **networking stack** I think has been increasing in importance in our view, not just through Beam chain but even for these scale L1, scale blobs, improve UX things. We realized just how far behind we were in terms of networking and how much multiples of improvements we could get from just having better networking.

So the first investment that we made was to have this **new networking team at the EF** which is now three people if I'm not wrong, and Marcos is one of them, right? So he's one of the OG LibP2P guys, which maybe is an example of what you were describing before—people just needing to get hired.

But I'm probably not the person to have this conversation with because I'm not directly working on this. But I think if you feel like a very critical part of our stack is in danger of not being maintained, that we should have a conversation. I would be happy to route to the right people.

#### Clarifying the Risk

**LibP2P Representative**: Let me be clear—**it's not in danger** because the core maintainers all work for the various companies that are funded in other ways. You know like Alex—he does .NET LibP2P, and Marcos now is over at EF who was Zig and Go and JS. Now he's just doing Go and JS now that we have the Zig team over at Zig. And we've got Mike over at Sigma Prime.

So like most of the maintainers are employed in other ways, so **is maintainership going to go away? No.** But **overall community coordination is what's in danger**. And that is a hard thing to put in a category because it's like: is there infrastructure for meetings? Who's running the GitHub organization? I'm one of the few people that have commit access—I shouldn't admit that in public, but people like me need to exist.

I guess it could go to someone like Mike over at Sigma Prime—that's fine, but I don't think he wants to maintain like... Is everybody taking notes of the meetings? Are we having LibP2P day? Are we getting sponsors? Are we handing out education grants? How are we training up new engineers who are coming into the Ethereum community and learning how the networking stack works?

So there's a whole lot of other things that is like **one or two full-time jobs** that don't fit cleanly into "oh you can just go work for Nethermind" or "you can just go work for" whatever.

**Barnabé**: Yeah, thanks. You see what I mean. So there's like—we don't have a total solution. Is it in danger? No. But it sounds like because maybe the ownership is unclear and the maintainers are so decentralized, it makes it hard to have a center of gravity.

**LibP2P Representative**: It's really just the frosting on the cake—it's the last 10 percent that make it an awesome community. But it isn't categorized or funded directly.

**Tomasz**: I'd be happy to chat offline.

**LibP2P Representative**: That'd be awesome, thank you.

---

## Closing

**Tomasz**: Thanks for the final questions. I guess we'll leave it there.

---

*[End of panel discussion]*

*Note: Transcript generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*