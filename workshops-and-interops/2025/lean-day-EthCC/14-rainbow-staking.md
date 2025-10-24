# Lean Day - Rainbow Staking Framework

| Item | Details |
| ---- | ------- |
| **Session** |  Post-Quantum Recursive Signature Aggregation for Beam Chain |
| **Speaker(s)** | Barnabé Monnot, Ethereum Foundation - Team Lead, Protocol Architecture |
| **Slides** | [link](https://docs.google.com/presentation/d/1BWCxs9KehnuTUSy8Q81aaMRW3YwSlarDs-UZYl5sF_0/edit?usp=sharing) |
| **YouTube** | [link](https://youtu.be/qTrXG3Z-aTQ?feature=shared) |

>back to: [Lean Day Overview Page](/workshops-and-interops/2025/lean-day-EthCC/00-index.md)

---

## Rainbow Staking Framework

### Introduction and Context

**Moderator**: [The discussion is about] speed of light, first principle, minimal slot durations versus the pragmatic implementers approach or just picking the low hanging fruit and not necessarily aiming for the optimal stuff. So we're going to get started with rainbow staking.

**Barnabé**: Thank you. OK, yeah. So I'll talk a bit about **rainbow staking**. It's a framework that I wrote about last year in a post that was trying to make sense of many different ideas that have been published before by David, Dan Craft, Justin, as well. Yeah, I'm by the way, from the **protocol architecture team**. I'm happy to be here.

---

## The Central Question: Future of Staking

**Barnabé**: So I would say the leading question that's kind of hiding behind all of this is: **what will staking look like in five years?**

So there are many layers to staking—there is the consensus mechanism, there is the leader selection, we have the validators, there's the economics of it. And the staking experience that we have today could be very different, let's say, five years from now.

### Current Staking Model

So today we have the **single entry point**. You have to stake 32 ETH. That means you have to become a network operator, a validator. And basically you are tasked with doing **pretty much everything** that you can think of that should fit on a machine that works for us.

### Future Vision: Unbundled Services

In the future, we think it makes sense to possibly **unbundle the services that validators are doing**, even further. And that's what underpins the idea of rainbow staking. So having **much more diversity** and many **differentiated services done by different service providers**.

---

## Current Validator Responsibilities

**Barnabé**: So this is a quick map of stuff that is done today by validators:
- **Censorship resistance** by making blocks
- **FFG finality gadget** functionalities 
- **P2P** networking from the beacon chain
- **Syncing the node** and the synchrony
- **Proposing blocks** and being the builders, sometimes if you don't also outsource

### The Economic Security Foundation

But one of the things that is, let's say, at the core of the staking experience for you as a validator, is that **your stake is fully slashable**. And this is what we say provides us with this feature that we hope is useful to people who consume Ethereum, which is this **economic security**.

---

## Understanding Economic Security

**Barnabé**: There was a lot of debate, as always, on what economic security means. So I want to go a bit into it, because I think it's also important to understand why unbundling makes sense.

### Definition and Challenges

So **economic security is really the ability for the network to say X ETH will be lost if Y happens**, for some amount X, and for some outcome Y. So for instance, if we ever had a safety fault of the finality gadget, FFG, we should be able to say, or we will be able to say, **at least a third of the ETH that is currently staked needs to be slashed**.

And so we can basically evaluate the total loss of economic value to the network.

### The Delegation Reality

Another part of the staking experience is the **pressure to delegate**. And I think we've seen now that **delegation is almost inevitable in proof of stake**. It was just a natural consequence of the phenomenon of **separation of capital and labor**.

And in fact, unlike proof of work, where you had to buy machines that are doing the hashing, **in proof of stake, the capital is ETH so it's much more liquid**. Trust is also much easier to delegate. And when you have a smart contract platform that underpins the network, it's even easier, because now you have all this infrastructure to do these programmatic maneuvers that allow people to pool, to delegate and to have pretty sophisticated payout systems.

### Capital Efficiency Pressures

And so one thing that also to note is that **the capital is always seeking yield**. That's usually what capital does. And so they tend to go towards delegating where the capital has the **highest efficiency**. And so where the operators are the most capital efficient.

---

## The Economic Security Problem

**Barnabé**: So when you put these two things together, like the economic security and the delegation, **the story gets a bit unclear**. And in particular, the act of delegating blurs a little bit this story of economic security.

### Unclear Attribution of Loss

Like we say, "X ETH will be lost if Y happens." But **we don't really say whose ETH it is that gets lost**. Like is it directly the ETH of people who attacked? Like when I give my ETH to Lido for instance, to delegate and they are part of the validators that are slashed. Is it me who attacked the network? Is it Lido as a protocol? Is it one operator of Lido? It's not always very clear.

### Principal-Agent Problem

So the delegate could be at fault and lose the stake of their principal, so being the person who stakes with them. And **we have this principal-agent problem**. That means essentially that we can have a faulty agent that can be much cheaper to corrupt than the amount X that is lost and still get a network fault.

**It doesn't mean that economic security is a meme**. Like there's a lot of positive things from having this constraint of X ETH being lost. But it just does not also mean that X is the cost of attack to get outcome Y.

---

## Why Economic Security Still Matters

**Barnabé**: And so why do we still have this idea of economic security? Or why I think we have it? It's because **we want to impose a certain amount of discipline on the network**. We want the principal, so the person, the people who provide the capital, to **choose their agents carefully**, to choose the operators that they delegate to in a way that they are conscious of the risks that the network carries.

### Credibility and Innovation

And they also ask their agents to essentially have a lot to be as credible as possible to provide guarantees that the agents are just not going to get truly compromised or are participating in attacks.

So for instance, what we see now is innovation with the **dual governance of Lido**, distributed validator networks and many other systems that allow us to be more confident that the agents are not going to be compromised.

### Client Diversity Incentive

Another positive effect is having this possibly very bad outcome and the commitment that we will go through with it because it's programmatic means that **we really have an incentive to make client diversity work**. And that prevents against what I would call **"oopsie-slashing"** where we have a consensus bug in a majority client and we finalize something bad and then a lot of stake could be lost.

---

## Impact on Home Operators

**Barnabé**: So having economic security is a good thing but it does impose a lot of constraints and pressures on the network. In particular for **home operators**, it makes the picture a little bit more difficult for them because they are apparently untrusted—they just look like an address on the network.

### Capital Inefficiency Challenge

You don't have an off-chain signal of their credibility, like a brand or a corporation or something. So that makes them **fairly capital inefficient** because they have to convince you of the fact that they are not going to be truly compromised or participate in an attack.

### Lack of Blocking Power

Another thing that's a little sad about home operators today is they are **not really a large enough contingent to block supermajority from capturing finality**. So I believe we have about **five to six percent of solo stakers** on the network and maybe **three and a half percent more participating in Rocket Pool** and alike committees like Diva.

So it's really **far below the thirty-three percent** that would be needed to prevent finality from happening if the other part was captured.

---

## Why Home Operators Matter

**Barnabé**: And so if they are not so effective at that, why are they still needed? In my opinion, for multiple things:

### 1. Censorship Resistance

The first reason why we really want to have home nodes on the network is because we assume that **by being so numerous and by having so distinct preferences from trusted operators**, they are going to be very good at **providing censorship resistance**.

And the way that they do that today is essentially via **local block building**. So if they are proposed the task of building a block, if they decide not to outsource it—they can decide to outsource it. But if they decide not to, they will impose their view of the network via this building.

### 2. Active Monitoring Layer

Another feature that we have is you can think of them as an **active layer of monitoring**. They are paid to be around and to attest their view of the network. So that can come in handy. Let's say when there's a sophisticated attack. Like we know that we have this reserve army that's ready to step up.

But you could make the case that other non-staking but still validating nodes, like full nodes, could also be part of that line of defense. They also follow the network. They also execute the rules and they would also detect if something went wrong.

---

## The Unbundling Proposal

**Barnabé**: So the question is, **can we get the best out of what the home nodes have to offer and maybe make the picture better for them?**

### Current Role Bundling Problem

So today we are doing local block building. I picture **three roles**:
- The role of **attesting** (like that's the envelope or voting)
- The role of **including** (it's the little truck) 
- The role of **building** (with the crane)

So local block building is their way of including transactions. And if they outsource now, they're not really providing the role of including transactions, which is the dotted line. Because now they're not sure that the builder will provide it for them.

### Solution: Role Specialization

So when we outsource, we might lose some of these properties. So we have to be a little bit careful. The one way that we can recover the participation of home nodes in these essential services that they're really good at is to **allow them to specialize in some of these roles**.

And in particular, I believe that **decoupling the pressure that economic security imposes on home nodes** would be a very good direction for making them just more powerful on the network.

---

## Heavy vs Light Services Framework

**Barnabé**: So when we try to fit every type of node, commercial or home node through the same pipe, and this pipe is essentially bound to this idea of economic security being fully slashable having a ton of risk. We don't really put these home nodes in the best footing.

### Service Categories

And for some of these services, we don't actually require this extreme pressure to be efficient, to be trusted, to not get slashed at all. And so **can we separate these two kinds of services**—the ones that don't require economic security from the ones who do?

And so that's pretty much the picture of rainbow staking, saying we can have:
- On one side a category of service which is **fully slashable**, like **heavy services**
- On the other side a category of services which is **not slashable** (**light services**)

### Characteristics of Each Category

The services that are slashable would be:
- **Fairly specialized**
- **Quite sophisticated actors**
- **Capital efficient** providers

And on the other side the non-slashable services could be:
- **Much broader**
- **Much more accessible** 
- **More fault tolerant** than the heavy services

### Horizontal Separation: Operators vs Delegators

So that's the first separation, let's say **vertical**. And then I believe that since we are seeing delegation be such a natural phenomenon in these services, we might as well also **acknowledge that these delegations exist**.

And so we can have this **horizontal separation** where for each type of service we can make the distinction between **operators and delegators**.

---

## Comparing Heavy and Light Services

**Barnabé**: To make a bit of a distinction between the two:

### Purpose of Stake
- **Heavy services are slashable**. The purpose of stake in heavy services is really to **provide economic security**—to make these commitments of saying money will be lost if something bad happens
- For **light services**, the stake is a little different. We can call it stake, but it's more to **provide sybil resistance and leader selection mechanism**

### Intermediation Levels
- For **heavy services** because of the economic security, we expect them to be fairly **intermediated**, meaning that we have agents that are sophisticated that might have longer chains of intermediation—like for instance, Lido protocol, has node operators under it, it's very sophisticated, it's not just a single person with a box
- Whereas for **light services**, we would want them to be **easy to provide and directly provided by operators** as much as possible

### Fungibility and Risk
- For **heavy services**, since there's real risk, it means that the yields of the cash flows that are earned by the operators are **not fungible with one another**. That's one reason why we have Lido, which is essentially a protocol that makes fungible different formats of different node operators
- But for **light services**, since there's no real risk because there's no slashing, we can have **much more fungible provision of yield** or cash flows

### Fault Tolerance and Alignment
- We can also have **much higher fault tolerance** because there's no risk of being slashed and it can have different penalties
- For the **heavy services**, we need **very high alignment** between the principal and the agent because again, there's risk
- Whereas for **light services**, it can be very low. And if the agent is not doing what you want them to do, you can **very frictionlessly delegate away** from them

---

## The Ideal Protocol State

**Barnabé**: And so yeah, that's a picture I presented at a protocol—it's kind of my view of the ideal state of the protocol, which is not one where we have a single type of entity that does everything, but more like **we have groups that do different things and that are keeping each other in check**.

And I think that's the best way to have resilience on the network—to ensure that these different groups, the political economy that results from having separation between different providers who are specialized and very optimized for the role that they do, **makes for a healthy network** where we can just rely on the fact that we're not dependent on a single type of provider.

So in a way, thinking of **plurality at the scale of protocol design**.

---

## FOCIL: A Key Light Service Example

**Barnabé**: So one key light service is **FOCIL**. So FOCIL is a censorship resistance gadget that selects every slot—in the current parameter it's 16 includooooors who propose lists of transactions that must be included into the block.

### FOCIL Characteristics

So it's a service that you only need to **pick 16 people** and they only need to give you these transactions on the network and force that they are included. So that's a very powerful service to augment the censorship resistance of the network.

You select 16 nodes, but actually **the honesty assumption is very weak**. You only really need **one node out of 16** that does a good job for censorship resistance to apply to the network.

### Technology Enablers

Once we have technologies such as **zkVMs, data availability sampling**, hopefully you can even provide that service with **very weak hardware and very low capital**. So much weaker than what you would need for staking and attesting.

### Two FOCIL Variants

And so that means that we can actually have **two different versions of FOCIL**:

1. **Bundled FOCIL**: The one that we're currently developing, where the roles are bundled. So where the validators are the ones who are also the includoors

2. **Light FOCIL**: Which could even be **hidden behind your wallet**. So as long as you have some positive balance, you could sign up to be one of these includoors. You could have the correct view of the chain through zkVMs and data availability sampling is very efficient. You could have a light client to look at the back. You would still be pretty light.

### Fault Tolerance Benefits

And you could just participate as an includoor with this very weak assumption. And maybe sometimes you're offline, but it doesn't really matter. It's a **very fault tolerant service** because again, you really only need one of the people you sample at any given time to give you the correct view and to provide censorship resistance essentially.

And so I think that's a very powerful option for us to have **censorship resistance provided almost like as a swarm at the network level**.

---

## Benefits of Service Separation

**Barnabé**: So if we are able to separate the light services from the heavy services, there are some benefits to having this firewall:

### Better Censorship Resistance

So on the one hand, we can have **better censorship resistance** provided by the light services.

### Stronger Assumptions on Heavy Services

On the other hand, that also means that we can have perhaps **stronger assumptions on the providers** that provide the heavy services. So we can still keep, in my opinion, the attesting layer fairly permissionless, but maybe we could even consider **increasing slightly the minimum effective balance** to participate at that layer, if you could also participate in the other layers more permissionlessly.

### Path to Faster Finality

That might be a good idea, because again, I think **we really need to get to faster finality soon**. The quickest way is to **shrink the validator set**. That doesn't mean kicking people out—again, today we have a million validators, but we really have 8,000 nodes behind them. So even consolidation would be effective.

But we could also be thinking of what is the separation that allows us to make stronger assumptions on the heavy services. And I think that's an important question that we should ask ourselves.

---

## Economics of Light Services

**Barnabé**: Another point on the economics of light services. So I'm saying, let's send people to these light services, but the question that would come is, **are they paid? Are they incentivized?**

### The Incentive Dilemma

Like, this is the part where it's a bit more tricky. We have actually designs for **incentivized FOCIL**, so it's possible. The question is whether it's desirable, because **the minute you put incentives, the minute MEV jumps into the picture**, and can easily derail the mechanism's purpose, which is censorship resistance.

### Alternative Use Cases

There are questions, like for instance: let's say I invested recently in a better setup because mine burnt out. So now I have this mid-size node at home. Can I still use it for something useful, even if I don't want to attest, but I don't want to just do the wallet thing?

We are talking a lot about **partial state provision**. There are a lot of designs that are currently being discussed.

### Future Service Landscape

And so, yeah, the alternative is—beside the home stakers, we provide the distributed work. So in my opinion, in the future, we won't lack options for services that fall under the light category. And we may even have alternatives to incentivize them, such that people are paid for the useful work that they provide to the network.

---

## Implementation Strategy

**Barnabé**: And so **what should we do**, given this picture? I think **shipping and seeing all of these technologies that enable this future** go live will really reshape our perspective in terms of what's possible with these different light services and heavy services.

### Technology Dependencies

In particular, I believe after zkVMs, data availability sampling, and FOCIL ship, we have a very different perspective on what being a node means. And what are the multiple possibilities of providing value to the network, beyond just being a single bundled, staking node?

### Iterative Approach

Then we might have an easier time also articulating the things we really want from our nodes. And so, yeah, I think **we should accelerate on all of these tech trees** and revisit the question every couple of months to see where we are at.

Yeah, thank you.

---

## Q&A Session

### Question 1: One ETH Staking Economics

**Audience Member 1**: Yeah, they're not hard super related, but if staking limit goes down to one ETH, as mentioned sometimes obviously the APY for those that stake would go down, do you think that would be enough incentive for someone to stake one ETH alone?

You mean if more people stake at one ETH, the yield would go down? People should stake proportionally, the gain of someone who's staking one ETH would be much less, so would people actually do it?

**Barnabé**: Yeah, so that's a point about **capital efficiency**. So I have fixed costs to staking, and this cost will go way over. And I believe in the meme of **smartwatch staking** that Justin has, like you might be able to stake with a Raspberry Pi or something very light.

But it's true that **the more capital you put on your fixed cost, the more capital efficient you are**. And compared to staking pools that are able to put thousands of ETH on a single node or single protocol, you're always going to be at a bit of a disadvantage when it comes to the real yield that you get from the node.

So there's a lot of conversations, like last year, around **issuance** and how we should be thinking about the relative earnings of solo stakers versus professional operators. The question is still not settled, but I don't think we should do anything about it until we figure out more about this larger picture of future staking.

But yeah, I think it's a **universal truth of capitalism** that the more capital efficient you are, the better you fare in any system that rewards you according to that metric.

### Question 2: Social Consensus and Coordination

**Audience Member 2**: One question is, if we unbundle all the roles into different specialized algorithms, how does this impact the ability to very simply have social consensus or coordination to fork a chain? Because everything is so simple that there's only one thing to do, which is attest to the chain. And there are 8,000 operators. How do you contact them, what do they need to do?

**Barnabé**: Yes, I agree. So I think the worry that I have is the operators that are effectively in control of the chain now—the attestors today. And as I was saying, the home operators don't really have a blocking minority there. So there's a lot of things that they couldn't really do.

### Enhanced Coordination Tools

My, again, coming back to this picture, which is not an argument, but more like a mental model. I think **once we get to that world, we may also have much more tangible ways of triggering things such as community reaction**.

So let's say, for instance, all these millions of wallets that are possibly doing FOCIL at the back, they might be emitting messages to say, "I've seen these transactions." These messages might have proofs of other wallets saying, "we also seen these messages." Over time, you can have **actual evidence that can be used to programmatically trigger certain actions**.

### Better Signals from Uncorrelated Entities

So today, we're kind of reliant on only having the attestors performing their role. If the attesting layer was fully compromised and was starting to censor, it would be easy, I think, to say something's up. Like we could do it, but there would be a bit more community organization.

So the idea with this is giving us the tools to make **much more automatic and programmatic the reaction and the courses of action** that we would want to take if, let's say, people were making inclusion lists, but they weren't included in blocks because they were still censored by the attestors.

In the model that we would have with heavy FOCIL, it's the same people—the attestors, the other ones were also making include lists. And so now, when you have **two groups and they're both doing different things**, you might be able to have much more **uncorrelated signals from either side**. And then be able to programmatically trigger courses of action based on these signals.

### Question 3: Permissionless Signature Aggregators

**Audience Member 3**: If we have permissionless signature aggregators, where would they fit in the heavy versus light services?

**Barnabé**: I'm not sure that we would need, for instance, economic security behind these aggregators. In the model being proposed, they come with proofs. So you don't actually need any security from them because they will be **provable**.

### Safety vs. Liveness Faults

Yeah, so there's basically **two types of faults** that you want to prevent against:
- **Safety faults**
- **Liveness faults**

So you might still want to have some kind of economic penalty for liveness faults. If someone's not doing their job, in the case of FOCIL, I don't actually think liveness faults should be penalized. Like I think, if I was to point my ETH to Justin because I believe he will do a good job as an includoor, and I see, over time, his performance is degraded, I can just **point my ETH away from him**.

For services that are maybe more critical, like aggregation, perhaps you would want to consider economic penalties for liveness faults. Like if you are signing up to be an aggregator and you're not doing your job, maybe we do actually want to slash you. But it might be **much lower risk than fully slashable**, for instance. We could be just partially slashable.

But if we have permissionless aggregation, then maybe I can dedicate my ETH to Justin. And if his performance goes down, then I'll probably move my ETH to somewhere else. Yeah. That could be a way to do it.

**Moderator**: OK. Thank you, one last question.

**Moderator**: Thank you.

---

*[End of Barnabé's presentation]*

*Note: Transcript generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*