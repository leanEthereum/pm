# Beam Day - Client Development and Funding Strategies

| Item | Details |
| ---- | ------- |
| **Session** |  Client Development and Funding Strategies - Lessons from Nethermind |
| **Speaker(s)** | Tomasz Stanczak (TS) Ethereum Foundation - Co-ED & Founder, Nethermind |
| **Slides** | [link](https://docs.google.com/presentation/d/1yz53GX8dRNuMo2N9zthhiGk7PZCsdbB5oDoFqy4EC9s/edit?usp=sharing) |
| **YouTube** | [link](https://youtu.be/fyg_f6JNd7I?feature=shared) |

>back to: [Lean Day Overview Page](/workshops-and-interops/2025/lean-day-EthCC/00-index.md)

---

## Client Development and Funding Strategies - Lessons from Nethermind

### Introduction and Disclaimer

**TS**: I have lots of history of how it is to pay for client development, how to grow in the tough times and the better times. The second thing is that some of you may expect that I'll have much more knowledge about the future funding or grants for other clients, but we actually separated that quite clearly in EF—so that's Ana's job and she's not here, so maybe you'll be slightly disappointed that I won't give you that much of an oversight of how other clients are working.

I usually don't ask them too much, so I'll just get a conversation with Greg, but usually don't do that. Just not to create that impression that the clients feel awkward and we'll be sharing it with Ana. So yeah, I'll give you a bit of a story to show that actually building a client and funding the client is actually a bit tougher unless you add a few things.

---

## Early Days: 2017-2018

### Founding and Initial Funding

**TS**: So Nethermind was founded in **2017**, the ETH price was around **$300**. It matters because it was a source of funding for me at the time. I had around $40,000—I don't even remember well, I had around **$500** worth of ETH at that time. Now it's almost zero directly because of developing the client.

So I sold **25% of my ETH** and I thought, okay, I'm building a company that will be around Ethereum. I found it today that in 2020, I was remembering the first days and I just sat down in the office. I rented an office in New York. I thought that's just to keep myself mentally sane—I'll rent a nice office for myself in New York and it had a nice view. And that was my biggest expense for the next one or two years, almost until I hired the first person.

So I was not paying myself any salary from the company, but I had the money from sold ETH to just sustain myself living in London. Living in London wouldn't be the best choice if I didn't have this money—then probably the solution would be to go somewhere to a very cheap living space and work from there for a year or two.

### Technology Choice: .NET

**TS**: What else here? I think this is an important concept. From the very beginning I was thinking of it as a company that will build something. Choosing **.NET** for building the client—it was not the coolest language that does exist. It was a bit like this, but I chose .NET because I knew .NET. I thought it would be good enough to build, and I started building it just to very quickly understand Ethereum and the Yellow Paper and to start building something related to trading.

I wanted to build decentralized exchanges and started thinking that hedge funds and financial industry companies would want to pay in the future for the node. So I was thinking, well, nobody builds enterprise solutions, nobody builds for enterprises because everyone builds for cypherpunks. So if I pick .NET, it's a very enterprise language. It will attract people that normally would not go anywhere else to build other clients. So I can do it. This is where I could ship the fastest because I knew the language very well.

I was thinking at that time, well, **Rust would be better**, but there was very little Rust [ecosystem]. I thought I would have to learn it, but I thought no, I want to ship it as fast as possible. I just want to feel very confident in the language that I was building. It's very different from very often people [who] almost pick a new language to build a client just to learn the language for fun. That might delight you if you think that you're building it, but there are different reasons. So you want to build the company around it, or you want to just experiment and learn as a developer. I think both ways are fine. Maybe they define differently how you approach sustainability.

### Vision and Market Strategy

**TS**: And then at the time I was thinking, okay, so in the future maybe there will be digital assets custody, market making, brokerage, real estate assets, things like that. So the **financialization of this market** because I was under the impression of a book, Hedge Fund Wizards. I was talking about how in the 60s or 70s people were building systems for trading and they would trade based on the TV news because everything was so slow.

And they were talking about the transition, the changes in the 80s and 90s into high-frequency trading and how the market data started flowing in and all the risk management happened with the automated systems, electronic systems, and how it entirely reshaped the industry. And I said, okay, it took them 20 years—probably in blockchain we'll take just a few years, but people will start paying for that.

So there is a vision of making money, and at the same time, I was trying to fundraise for a data marketplace. I saw that selling data would be a solution. It was a bit of a failure, but we were building it on the side for three years. And I was trying to do fundraising from VCs, but never managed to.

---

## The Struggle: 2018-2020

### First Release and Challenges

**TS**: So you see, **2018** at the top, so almost one year after we started, there was the **alpha release 0.9**. And it was able to sync the entire mainnet. And I didn't have fast sync and it didn't have some other very important things. I don't remember what was missing at that time exactly, but it was a big moment. Okay, releasing something for the first time on Reddit. And I was not even on Twitter. So it was very important for me. I missed all the social media. I was not connected and focused a lot. Like I was building 8 hours a day, just client, trying to pass the tests and so on.

Now, I consider this probably on one side as a massive set of mistakes. I'm telling you how to build sustainable clients, but it took **three years to deliver something**—well, one year here, but then to really start making money on it took three years.

### Grants and Survival Mode

**TS**: So **September 2018**, we got the grant from Ethereum Foundation. **$50,000**—made the first hire. **2019**, first ConsenSys grant from POA xDAI supporting—all right, in that amount. So it's looking for opportunities. How to get grants, build something on the side, building some consulting projects. So I started helping some other projects that wanted to build EVM into their chains. Getting jobs where I would almost drop my work on Nethermind client, just get some consulting work that I would be paid directly. Just to survive.

### Financial Reality

**TS**: And here are some numbers. Over two years, we spent **40,000 pounds**. And we could call it—on average like **$3,000 per month**. And it was a team of four. So monthly expenses, like **$6,000** for all this team of four.

And in **January 2020**, like ETH was at **$80** at some point. And to avoid almost bankruptcy, I had to **sell all my ETH**. So remember that $500 worth at the beginning? It just disappeared in 2020. Funny because everyone was suffering at that time. Like so all the teams were not paying grants, but the payments and invoices were delayed, everything hit you at once.

**April 2020** raised from two fantastic angel investors. And then things started going pretty well. Because ETH was going up, people knew that we were building for two years when nobody else was building, so people started getting in touch saying, "Build for us."

---

## Growth Strategy: 2020-2021

### Aggressive Scaling Decision

**TS**: And the one important decision that I made at that time—and I compared to other companies that kind of stayed in place. Like at the same time in 2020, they were at the same size of like a few people building. And I would meet them two, three years later and ask what did you do differently. And the only thing that we did differently was **we assumed that the market was going into a bubble, and we can scale it higher in team size**.

So we **hired 24 people** and we said like, "now we have to take advantage of it." So for three, four years, we were operating at **three, four months runway**. Just always pushing to invest more and more into the business, getting a lot of risk and keep building and keep pushing. **Super stressful**. I mean like generally I don't recommend it to anyone. I still regret that time—it was so stressful.

### Lessons on Spending

**TS**: So, well, if you want to build something sustainable, it's so different from this story in the same way, but you can show you that in the end there is a... Well, sorry, like as you'll see in the moment that later, Nethermind is something like **20 to 40 million dollars revenue per year** from various supporting entities, but... In the end, not spending too much is important.

And if I got the money from either VCs or bigger grants at the very beginning, I would spend it super fast without knowing how to operate a business and how to build something sustainable. But if you have to run for two or three years at very, very low spending levels, that **teaches you a lot**. Like how to really value the development effort, how you value a dollar, how to really think of building and also learn simply to build.

Now, I was already a 15-year development experience person coming from financial services, from Citibank and others. So it's not like I didn't know how to code, right? So I was building with a flow. Like I was learning a lot of cryptography and blockchain systems and P2P, but apart from that, I was coming [along] very fast.

### Major Funding Milestone

**TS**: And then **2021**, you know, there is this Ethereum Foundation client incentive program. It was **$4,000 ETH for all the clients**. Some clients got 50% of it, and that was massive, massive support. And this is like a **$40 million spending plan** for another 14, 24 [months]. So you can see how we just always think of what's the future technology and how to start growing things.

---

## Business Strategy and Sustainability

### Using Client Expertise

**TS**: So we use the client as an expertise and legitimacy for building other things, providing consulting to companies. And also for thinking that people that are at Nethermind should always feel this kind of impression of **constant growth and pressure** to think that they're always building something innovative and going forward.

And I thought that this is the way to keep attracting talent. But if you tell people that you're not just maintaining it forever, but you'll always be thinking of new things, new things, new things, and always aspiring for brilliance, and for the best potential codebase, then it works in attracting talent and you keep the talent. And then you can also use your equity for funding.

---

## Different Client Funding Models

**TS**: So now there are many different models that you see from existing consensus layer clients and execution clients. And then possibly, like if you see this, like Besu clients, Teku, and other solutions, so it's people who build Besu clients, who build, you know, things like MEV solutions. So you always will start it and it will be probably very often the same story.

### Venture Capital Path

Before you get to revenue, either you go **VC route**. And then you might be pressured into a token, or very, very likely you'll be. And then it's really like, I think in many cases, the token solutions are not really synchronizing the alignment—sorry, the incentives for founders—when the token becomes liquid and the incentives for everyone else. So it's pretty tough for you often.

### Public Goods Model

The **public goods** [model]. So this is funding with grants obviously, the Ethereum Foundation and other foundations. You're building the client and you try to think how you can use this expertise to get other grants. Maybe bits of the clients that you're building, you can sell it to other companies. Maybe they want to build something on top of it. And then you have to be really open for accepting grants.

### Consulting and Services

**TS**: Leveraging expertise into **consulting**, which can be security audits, it can be operating infrastructure, like **Lido practice**—you allow all the clients to operate within Lido. So it's practically like a grant, giving 10 client operators the space for being an operator within Lido. It's pretty nice revenue.

And all the other things where people think, "okay, those guys, they know what they're doing. So we can take advantage of it and we can hire them as consultants."

### Corporate Acquisition

And the other option would be, I guess, a company that's already established, thinking like ConsenSys, thinking that they need to enter into this business to be very visible there. And while I'm guessing here a bit, but this would probably also be a behavior of Nethermind thinking, like, why we would build on-chain. Like, to always keep being fresh with latest things and being around the people that continue building some new things.

---

## Risks and Lessons

### Focus Management

**TS**: It can be risky in some situations. We tried to build the consensus layer client a few years ago. We started building it, but we were not involved in all the meetings that much because we didn't have enough funding at the time. And I would just drop it and focus on execution client much more. So I guess it might be the same when you think, oh, there's something novel, but then you spend more money and you risk losing the focus and maybe you lose it from [the main business].

### Learning from Open Source Companies

**TS**: And one thing that was very important for me as well was that even as we were building the consulting systems and thinking of enterprises, there was also realization that in many situations, the model that I was thinking of was very similar to how the **Linux distribution companies** were operating.

When I was comparing the numbers with **Canonical**, it was very similar. However, Canonical was starting from the founder that had already like $500 million made from some other solutions and started investing in the company. And then the company nowadays, after 19 years, is doing **$250 million revenue annually**.

And the workflow, the product, the ideas of how you monetize it, I think that you can learn a lot from the operators of open source projects like this. And **Red Hat** itself was a **$34 billion acquisition** in 2019. So this was my vision of, well, even if we don't go into financial markets, then infrastructure can still make money if you think that you're going to make money.

### Core Business Philosophy

**TS**: And the main assumption was **never assume that you would just always be taking grants**. Always try to make it a business because it will just encourage you to build better companies because you will start delivering companies that people really care about, that professionals will support, will test, and so on.

Okay, thank you so much. Any questions?

---

## Q&A Session

### Question 1: VC Expectations and Control

**Audience Member 1**: You mentioned receiving money from VCs. I wonder what's their expectations on your return and whether that affects your control of the direction of the company.

**TS**: Yeah, I mean, so we never got money from VCs. I said we raised from two angel investors. So angel investors, I think they're much more laid back where they support you and they didn't want any control, which—they asked questions every now and then. Well, one of them ended up joining us as CEO later, and then actually nowadays he's running Gateway. So it's one of the other infrastructure companies—he's CEO there, I think.

So they were super supportive. They were learning with us. And the other one was **George Hallam**, who is one of the early Ethereum builders, one of the Ethereum Foundation people in 2014-15. And so they were amazing. They didn't ask for any control. They just were there.

With VCs, I think they would be totally different. I think it might have been a disaster if we managed to fundraise because I would try to build this data marketplace, which would be a project that was probably much less prepared to build. And then VCs wouldn't want to support a company with an executive's portfolio of ideas and thoughts—this is so hard, it's so risky because you don't build a single project that you can just accelerate. So this wouldn't be a standard investment for VCs; it would be hard.

### Question 2: Insights for Ethereum Foundation

**Audience Member 2**: What are the main insights you are going to bring to EF from your Nethermind experience?

**TS**: Yes. Well, interesting. On one side, I think that's sure, building a client is very, very difficult, but also now I don't want to be biased towards just one model of building a client, right? Thinking, "oh, this is how we built Nethermind, so just put pressure or expect that other clients would build it this way." It might be very, very wrong because look at like Reth's solution now—they fundraised tens of millions of dollars to build products from there. It seems like they're building a different product and it's a totally different model from how Nethermind was operating.

So there's lots of insight that's sure—you can have some builders that are willing to sacrifice themselves for a very long, long period. I think it's pretty random whether the foundation manages to really locate those people and allocate resources. You would have to go very, very wide, very broad, or do a lot of successful scouting.

So this also raises the question: does it only happen in the time of crypto winter, or does it happen also in good markets? I think you can find more of those builders in the crypto winter times because it's like this big filter of who wants to sacrifice and continue building when there's no business. And we kind of have a very different market nowadays.

So it's really hard to take any insights apart from like, sure, I understand the entire mechanism—we're defending these, what you can build, what different companies are there. So lots of insights, but I don't want to just over-index on how Nethermind operates.

**TS**: Feel free to reach out anytime on Telegram or in person, we can have a coffee and tell me how you approach it. It might be that I will learn from you more than you learn from me, but sure.

### Question 3: Revenue Sources and Market Focus

**Audience Member 3**: A bit more on funding sources and finding out where your funding sources [come from]. From your experience building Nethermind and also maybe your outlook after being exposed to more of the EF, where do you see the potential revenue coming from for clients? Let's say whether there's more demand from more Web3 companies or do you think there's going to be more demand from Web2 or institutional clients, and any situation on where it should be focused.

**TS**: I think it'll be both. I mean, now it should be a bit of **golden times** for integrating institutions to blockchain. So stablecoin clients, back-end solutions, and real assets. So things like what Circle is doing, what many of the altcoins are doing. So I think this is where much will be happening—by bringing those systems to blockchain and doing them our way will make sure that maybe this **melting of ossification** happens where the institutions join the blockchain and get melted into the network instead of polarizing them. And so these silos disappear because everything is so easily replaceable.

That's how the permissionless network should work. It should really decentralize every component of traditional systems. If we do it wrong, then maybe even if you have this like very strange collection of independent chains that are very decentralized internally but not very interoperable.

So I would encourage everyone to participate in this transitional work.

---

## Additional Commentary from Greg (Chainsafe)

**Greg (Chainsafe)**: I can probably provide some insights because we had a very similar track to Nethermind with Chainsafe, and when we went more venture, there's big trade-offs, and I think that's something that we should be aware of.

When you do go down venture, you have to start making very stark decisions. It's a kind of a great way to do that soon. We had a very similar track to Nethermind from the perspective of client development, a lot of consulting initially, then client development. We did end up raising venture, but the one thing when you get to that point is you have to start—you have to be very aware of the **trade-offs** and be willing to go down that path.

**Venture wants return**, and the big thing you have to think about is the scale and how you scale internally because you're going to have to make trade-offs in terms of where do you allocate money, how are you protecting the client team moving forward. And fundamentally, you're going to probably have to produce a product of some sort that is returnable unless you get a very interesting deal.

So if you do plan to go down that road, you have to remember that if you don't size up the team correctly and you don't plan and are constantly projecting where you're going, you can very quickly end up in situations that **the client actually starts to lag** because you're pulling resources away from it.

So you have to think very carefully, especially if you're driving the client as the core developer or the core team—where is your time going to get spent? Because it will dramatically push and pull, and that is what eventually happened to myself as well. So happy to also talk to anybody as well if you have questions, but I think it's a very big push and pull that you have to be cognizant of before getting into it.

**Moderator**: Thank you, Greg. Next up we have a panel discussion and then we'll go for lunch.

---

*[End of TS's presentation]*

*Note: Transcript generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*