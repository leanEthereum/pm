# CL/EL Metrics - Meeting 01


## References
- [Execution Layer Metrics Repository](https://github.com/ethereum/execution-metrics)
- [Consensus Layer Metrics Repository](https://github.com/ethereum/consensus-metrics)
- [HackMD Notes on EL Metrics](https://hackmd.io/@carlos-perez/el-metrics-notes)
- [Ethereum Improvement Proposals](https://eips.ethereum.org/)
- [Sunnyside Labs Projects](https://sunnysidelabs.io/)
- [Blobnet Project Overview](https://hackmd.io/@carlos-perez/blobnet)
- [Perfnet Project Overview](https://hackmd.io/@pari/perfnet)

## Meeting Overview
**Date:** July 25, 2025  
**Recording:** Not available

---

## Meeting Notes
- **Introduction and Greetings**: Participants joined early, with casual introductions. Katya, a former EPF fellow, outlined the initiative to standardize metrics across EL and CL clients, starting from Pure Das testing to improve research and testing. The goal is to have uniform metrics for better comparability and to gather researcher feedback on needed metrics.
- **Repo Demo and Structure**: Katya and others demonstrated the existing repos for EL and CL metrics. The structure includes lists of metrics organized by categories (e.g., beacon metrics), with columns for metric name, usage, type, collection event, and client implementation status. Links were shared for examples, including updates for Saka.
- **Researcher Perspectives on EL Metrics**: Carlos discussed EL-specific needs for projects like Blobnet (high gas limit Testnet stressing nodes with large state). Emphasized metrics tied to hardware and sub-processes (e.g., database compaction, pruning, write amplification) rather than specific EIPs. Highlighted variability in client metrics exposure (e.g., Geth detailed, others less so) and the need for similar metrics where applicable.
- **Metric Collection and Standardization**: Metrics are collected via Prometheus/Grafana. Discussion on aligning common metrics across clients, acknowledging differences in architectures (e.g., no compaction in some databases). Proposal to collaborate by compiling lists from teams like Geth, then consensualizing with others.
- **Collaboration and Processes**: Suggested creating dedicated MD files per project (e.g., Blobnet) in repos, starting from existing lists (e.g., Geth's), adding descriptions and uniform names. Plan to open PRs in the execution-metrics repo, share with clients, and involve contributors for implementation. Coordination via ACD calls, nudging teams, and outsourcing to communities.
- **Organization Suggestions**: Group metrics by functionality (e.g., network, opcodes, state size) rather than forks/EIPs initially, to avoid scattering. Separate "in progress" folders for research/dev. Distinguish metrics relevant to projects (e.g., ignore networking for Blobnet). Propose lifecycle: research/dev folders for early stages, move to main list once mature.
- **Challenges and Insights**: Noted difficulties in pushing clients due to time constraints; focus on high-value metrics (syncing, compaction, pruning, IO). Sunnyside Labs shared experiences with similar testing (e.g., high gas transactions) and interest in reports. Differentiate Blobnet (state/compute focus) from Perfnet.
- **Broader Outcomes**: Agreement to iterate on repo structure (e.g., by forks/features for dev, functionality for main). Avoid EIP-based organization due to future forks. Plan monthly reports for projects. Schedule follow-up calls to refine structure and engage core devs.
**Action Items**:
- Carlos to compile and separate existing/desired EL metrics list (syncing, compaction, pruning, IO) in next two weeks, then open issue/PR in execution-metrics repo.
- Katya to sketch repo structure (group by functionality, separate dev/research folders) over weekend and share for feedback.
- Jay/Minhyhuk to provide input on CL structure and organization.
- All to discuss repo organization in next call (e.g., next Friday), potentially invite EL/CL core devs.
- Katya/Jay to keep current PRs open until structure feedback is synthesized.
- Carlos to share Blobnet reports/scenarios once available, including monthly updates.
- Group to form user feedback sessions with researchers, core devs, testing teams for broader consensus before ACD presentation.
- Minhyhuk to explore contributor outsourcing for metric implementations.

---

## Meeting Transcript
### Introduction and Greetings
**Jay:** I gotcha.  
**Katya:** Hello. Good.  
**Jay:** Are you? Good.  
**Katya:** Good. How are you doing? Where do you based in US?  
**Jay:** Brooklyn, New York New York City.  
**Katya:** Okay.  
**Jay:** How about you?  
**Katya:** Nice.  
**Jay:** Oh, wow.  
**Katya:** I'm in Spain. Madrid.  
**Jay:** What part?  
**Katya:** Hello.  
**Jay:** Hey, Carlos. How are you?  
**Carlos:** Hello.  
**Jay:** You too.  
**Carlos:** All good. Good to see you. I think Raul is not coming. Right?  
**Jay:** Yeah. I don't believe Rahul's gonna make it. Minhyhuk and Jay from the Sunnyside Labs team. Should be here in a second. So we're we're pretty pretty early to this. So we're still  
**Carlos:** Yeah. I think we can just  
**Jay:** a little ragtag group of rebels.  
**Carlos:** yeah. I think we can just introduce our ideas to  
**Jay:** Yeah.  
**Katya:** Hey, guys. Hey, Jay.  
**Jay:** Hey there.  
**Katya:** Hi. Sorry. That's right.  
**Jay:** Yeah. So I mean, Katya, I don't know if you wanna introduce  
**Katya:** Yeah.  
**Jay:** You kinda, like, started the the whole effort, and I'm just trying to help  
**Katya:** Yeah.  
**Jay:** push things forward.  

### Initiative Overview and Repo Introduction
**Katya:** So I'm, like, ex EPF fellow last year. Fellow, and I've started my EPF project just helping teams to standardize metrics across clients. It started from Pure Das because it was in the testing phase and still in in into it. So the idea is to have the same metrics across clients, so to make testing better for research teams as well. And we would like to hear we will show you what we have now and we'd like we would like to hear from you how it would be like, better for researchers So how do you see the like, which metrics you need, when, and just in general, so why and yeah. So currently, we created two repos execution layer and for consensus layer. And I will share the link quickly. Maybe not that quickly.  
**Jay:** I got a handy. I can actually  
**Katya:** Yeah. Yeah. Please. Please share. So, yeah, if if you show this example, I pushed today  
**Jay:** Mhmm. That was on the beacon side.  
**Katya:** it's it's like yeah, it's like  
**Jay:** Right.  
**Katya:** for in for Saka, EIP, No. No. Just in your repo. For Saka. Yes.  
**Jay:** There we go.  
**Katya:** This. This one. Yeah. So yeah, so what we're gonna try to do is just  
**Jay:** Sorry.  
**Katya:** to have the list of metrics maybe organized in some way I don't know, like, beacon and metrics and and match and mark what client has these metrics like, so you can easily find it and whatever you need. Yeah. And we would like to hear from you you need metrics, what kind of metrics, what do you do with them, So yeah, how do you see it? If it if this repo will be useful for for researchers, How do you see it?  

### EL Metrics Needs and Challenges
**Carlos:** So from I mean, I'm a lot more on the EL than in the CL side. So I think for specifically or some other EIPs, it it makes sense to have metrics specifically tied to them. In our case, though, I think the the story's a bit different in the sense what we are trying to do is have Testnet which has let's say, 100 to a 100,000,000 gas limit and two to four x maintenance state So it's a much bigger test net. And so what we're trying to do there is to try to stress notes as much as possible. Stressing with transactions, of course, but also trying to target the specific processes that they do. So for instance, Geth, for example, needs to do compaction for the database. Right? There are some other clients that need to do pruning. There are some others that have a particular choice database architecture that might make them have right amplification if you figure certain types types of transactions. Like, there's all sorts of of stuff. Right? So what we are trying to do is we're trying to hit specifically the the most weird and tricky edge cases. Are unlikely to happen But they can happen. And so the goal is to be sure that if we bump the gas limit up to a 150,000,000 by the end of the year, We're a 100% sure that all clients handle, like, crazy scenarios. Imagine imagine you are under 10 or 20 straight block, which is really unlikely, but 20 straight blocks 150,000,000 gas, each of them changing as much state as possible. While your client is doing compaction, and then you hit a reorg. So that is highly likely, but chances of some of the clients just getting destroyed by one of the things are decent. So when I say it's not exactly the same, it's because the metric that we need are not actually tied to any EIP, in particular, are more tied to these kind of sub processes that happen So for us, it's like, things like pages of the database and how they are structure, how many compaction processes active do we have at the same time, how much time compaction is taking? Like, all of these things are a lot more important than specific things that target really specific EIP metrics or like that. So it's a lot more tied to hardware, I would say, than than any other thing. Yeah. But do we do you collect metrics from the clients? Or or Yeah. So what we are using is all of the all of the metrics that they exported through Prometheus, Grafana, all that stuff. And one of the first things that we realized is that everyone is just reporting whatever they want, basically. Like, there's metrics that are common sense, of course. And all of them report them. But then you see, for example, get which does a brilliant job at reporting everything related to the database And then you have other clients like Chris, which I mean, they don't give you much, let's say, or at least not with such great detail. Right? So this is one of the times when we realized that it it would be okay Of course, not all clients do the same. Like, there's no compaction with empty MDDX. So any gonna invest on the compaction at all. And, of course, they don't need to expose matrix But at least the ones that share metrics, we should try to have them to expose as close similar things as as possible, I would say. So that's the effort that I was trying to bring in. And in regards of that, what I had is this document so far. Yeah. Your hacking d r two I s, like, notes. Yeah. I think this yeah. So I expanded it a bit more This is, like, not sure how to push for this because it's hard and clients have tons of things to do, and this is time consuming and doesn't really give them much directly. It gives more to us. But the idea that I have is to progressively sit down with real teams try to speak with the people that know the deep insights the best, like Gary on on the guest team, and be like, okay. If we want to test syncing and compaction to the most amount of detail possible, we are the metrics that makes sense to have? Get a list of all of them, and then try to consensual it with other clients whether we can get them or not.  

### Collaboration Proposals
**Katya:** That that is This is where we can collaborate. For example, you have a list of metrics. We have some structure We have repo. So just not to lose them in the Hakindi nodes. So every client will know that, okay. We have a report for metrics and we can maybe organize it, for example, feature What's the EIP work on or the name of research? So the project doesn't even have name. I'm calling it Blobnet for now, but, I mean, Okay. Let's say Blobnet. Yeah. For example, we'll have, like, I don't know, MD file especially for Blobnet. And yeah, so that's where we can collaborate. So if, for example, if one of the clients has, like, perfect list, as you said, guest. So, for example, we can start from guest list. But what we need yeah. Probably, we can start from this list. And maybe later to collaborate with the core developers We need some description, like, a little bit like, what does this metric mean and the name of the metrics. So all the clients may have technically the same name, So, yeah, you can just use the name and you see the client. So, is what would be a voice on the ops. Yeah. I think yeah. It sounds at least that we can collaborate on that. I'm mostly on the CL part. If someone from the execution layer joins and it just helps, to understand how to group metrics, etcetera, etcetera, that would be great. Yeah. Probably, we may have another call. If, for example, we can or maybe you can open a PR or some of us open to execution, metrics repo. And put this list so we can share this PR with other clients and yeah, like this, so we have something more organized. Does it sound good? Yeah? Yeah. So there's I'm not sure what format do you specifically want this on or if there is any format. If I need to change something. Wow.  
**Jay:** If you see the right now, we're basically just doing like, essentially four columns, the metric name, the metric usage, the metric type, which is just coming from Prometheus standards. And then the sample collection event. And then really we'll be tracking each of the different client implementation status here.  
**Katya:** Alright.  
**Jay:** And that's yeah.  
**Katya:** Could you share the link, Will, in the the chat?  
**Jay:** Me go to  
**Katya:** Or share the screen. Yes.  
**Jay:** oh, sorry.  
**Katya:** So for me to understand  
**Jay:** Thought I was sharing my screen.  
**Katya:** these  
**Jay:** There you go.  
**Katya:** these repo, the execution metrics repo,  
**Jay:** Yeah.  
**Katya:** We also have it. We also have it for execution layer. So this is just an example we currently have metrics for CL and my my question is, in here, the winning put the metrics that we want the ones that actually exist? So our idea is you can create an issue. You want this list of metrics. And we make a PR, so we organize it, like this. For example, and, you can just mark just as open source project in the mark, which or every client can mark Oh, we have this metric, and we build this metric. Or we also help But at least you have, yeah, some Okay. Source of truth, kind of. Like, metrics you need.  
**Jay:** Yeah.  
**Minhyhuk:** Just one one quick thing to add. Well, initially, when we were proposing for the get blobs metrics for EOS side, because we knew EO teams won't be that collaborate collaborative in terms of implementing it. So just go ahead and did some initial work So yeah, if we need it, we can just implement it and then ask them to just release it. So metrics on that. I'm taking that long to implement anyway. So can just make the metrics on those text we put, and then just implemented for them and then use it directly without waiting them to implement it. I mean, even, like, open source, like, contributors of of different clients can do this. Example, we have repo and they can share it with their communities. Like, we need help. It's like, could you please build this metric for us? So this is what contributors also can do. So just some process we need to build, like, how it can look like This is what we're trying to do.  
**Jay:** Yeah.  
**Katya:** Like,  
**Jay:** That kind of coordination overhead and reminding people and nudging people, that's where I come in. So don't don't feel like I think once we get the a process in mind and understand what needs to be done,  
**Katya:** Yeah. We will bring it to to the calls. Right?  
**Jay:** you know, I I don't I don't yeah.  
**Katya:** Like, to ACD calls and  
**Jay:** Basically.  
**Katya:** just to notify clients. Yeah. We have currently this repose and going to use them. Let's collaborate on this. And whenever teams you work on, you can appeal to this repose like, okay. I need this one. And maybe for even for for the next project. So you have a next project on your mind and you can go through the metrics and to check, like, who has what and For yours new project, Yeah. Makes sense.  

### Project Focus and Organization
**Carlos:** So I will need some time to tie the up, let's say, all of the metrics. So this is a list of what exists now and what I would like to have. Kind of mixed together. So I will next two weeks, my plan is to sit down with all EL teams one by one and try to get out of all of these aside from other stuff. Like, the final list of all of the metrics that we will either want or have for syncing, compaction, and pruning, which are the three most important processes database wise. And also read, write, and IO performance. So this is overall what I would try to do. Once I have this, I will I'm happy to make the PR. But I don't think it will make sense to PR what I have right now because it's it's just a mix of what do we have, what do we want, and and ideally Yeah. May maybe not PR, but issue is is issue workers, actually. Like, yeah. Okay. So, yeah, just to finalize, from our side in Blobnet or whatever this project will be called, We are focusing especially measuring opcodes and measuring, like, the behavior of the chain related to state size and gas limit, that means there are a bunch of metrics that we just don't care. Peer amount of peers connected, like, all of the networking stuff probably, we don't get at all. So there's some metrics that are EL related, but we will actually not not track whatsoever. So I'm not sure if you also need some help on this or Yes. It's not a priority? I don't know. Like, how how do you plan that? We need this track like, for every layer, like, the preferable this is just what you were talking about, like, network, how just to group them better. So not only my idea, which we didn't discuss yet, but my idea, my new idea, how to organize metrics, So we can have the list of metrics which are already exist, for example, may not. And we group them, like, by I don't know, network or codes for EL. Etcetera. And we probably will have some folders or files which are in progress, like this API, just to find them easier. And when they go to main it, maybe we'll we'll just bring them to the main list. So it's still something we can discuss, but Because for developers, when they build something, they need, like, limited scope Okay. I'm working on this, and I'm not going to all the list of is, like, too much. I don't see which is like even even through PR. I don't know. We'll see. We'll see. But, but the researchers for example, they need the whole list of metrics. For example, you are working on a new project, you need to check what we have and open a new PR. Okay. We need this one. Something like this. Yeah. So we we will we will need your help, like, how to organize it on the EL level. Just the structure, how can we group Yeah. Sure. I I I I can definitely help with that. Maybe it would be interesting for you to chat with Patty because he's in charge of the 100,000,000 gas performance project. I'm not entirely sure what's the name. Mhmm. Do you have any published reports? Because I'm I'm from Sunnyside Labs. Part of Optimism Collective helping PROS testing. And we've also been looking at increasing sending regular transactions and blocked transactions at at the same time and we've been sending, like, 50% of the cost limit, those kind of things. So if I have a look at the report and see, like, what kind of you are using and what kind of metrics we are using. And then find some useful metrics from there. For report about what exactly? The project from Paris, mine? Yours. So from mine, we don't have any report, but because the project just started, basically. So far, we've I mean, I said, we're mostly focused on trying to break clients in in databases and stuff like that. So far, we broke Ethereum. They are currently working on a fix. But yeah, the the fun part hasn't started yet. We need to quote a bunch of scenarios, put them on eels. And once we have them on then we can start seeing what happens and generate some reports out of that. What I'm why I was working on all of the metrics is because I want to work in parallel coding all of the yields and spam on the scenarios, at the same time, on the metrics such that when I have both, I can get useful metrics from running these scenarios instead of one or the other. And, hopefully, I can get some help from from the EOS team also to to implement the scenarios because there's a bunch of them. But yeah, as soon as I have something, I will I will forward. Also, since this will be a project, I think I will need to build a monthly report and and all that stuff. So you will likely see a bunch of stuff about it soon. Okay. Okay. Sounds good.  
**Jay:** So, Carlos, is your effort called bloat net and what Pari is working on is perf net?  
**Carlos:** Yeah. I mean, I think they want to change the names, like, yesterday team. I'm I'm not sure if he wants to change it or not, but the we I mean, we were quite close because we're the very similar things with the his only compute and only state related. So that's that's more or less the summary here. Okay. I I already see the structure like research which is in progress, the IPs or something like this. And mainnet something. It's just yeah. General. Okay. Great. So further, maybe, don't know if you wanna stay with us or or just you have a lot of your stuff. We we we need to discuss what we have already about this repos and what structure we may build. Yeah. Yeah. Sure. Happy to listen.  

### Repo Structure Discussion
**Katya:** Okay. So so as Will shared the screen, maybe Jay you would also like to share. So my idea, I I didn't put it into some notes or anything, but my idea because you were against of EIPs. Right? So, and I kind of agree with that. So we may have the whole list of metrics divided by scope, like, grouped by scope or, like, network or beacon, etcetera. It depends on the layer. Or execution for consensus. It will be different. Maybe a network, yes, but others are layer specific. And we may have this list like what we have currently and the separate folders I don't know. In in the Beacon specs, it is called, like, features, for example. But we may have research I don't know, development or dev or something like this. Dev is like Sified piece or we can already build. So it's just to think over. Maybe you have your own ideas for the next call. So how can we organize it? And research is even like, not that in which is, like, currently in the research. So they need? Yeah. The main reason that I that we are against of having it in terms of bulk and within bulk EIPs is that because there will be so many folks in near future. So within five years, the number of folks will double.  
**Jay:** Mhmm.  
**Minhyhuk:** Right? You have to look at 10 different books to find out the exact metric that you want to see. Say, for the newcomers or normal devs who hasn't been working with on that part? Has to look up everywhere to find them. So I did it might be more intuitive to divide it by functionality rather than different books. That was one reason. But Katya's, your your idea makes I think it's kind of middle ground that if when you are finished, then you move on to the biphonximality part. I think that's quite good idea that if you're working on some EIPs, then make some metrics relate related to them And then once it once the EIP matures, then moving into the functionalities. I think that's good idea. Mhmm. Yeah. They kind of it's kind of double work but it's just better for I don't know, for everyone. So because yeah. And we will think of the structure, so how to organize by by maybe by forks because for some of the clients, it just for for developers. It's just, like, native structure. Maybe we will have archive folder when we move, like, up or Fusaka folder, I mean, by forks. And it it will be, like, development folder and then we have here for Saka or something. So we just need yeah. I need just to of it how to structure and ideas as well welcomed. And yeah. So later, I will maybe on the week on the weekend. I will try to structure it a little bit and share with you and you also, like, give your feedback on what can we structure maybe as well. Right?  
**Jay:** Sounds great. So the open PRs for now, just keep them open until we have a chance to review this and sort of make make a like, synthesize all of the feedback so far and maybe try and try creating a framework, you know, based off of the the the ideas that you're gonna sketch out.  
**Katya:** Yeah. Because, definitely, I think before we introduce somewhere on the CD, we need to have, like, more mature structure. So and we need to discuss it with different parties.  
**Jay:** Yeah.  
**Katya:** Like, core developers. So, for example, for next call, can do it on the background or I can invite, like, core devs from CL or in EL and just how they see, for example, for CL, how can they would like to be structured.  
**Jay:** Yeah.  
**Katya:** Yeah. Or I can discuss it and I'll meet my team.  
**Jay:** Think having different representatives for that You know, we're we're talking about, like, kind of a life cycle  
**Katya:** Mhmm.  
**Jay:** You know, you've got, like, researchers and, you know, early prototypers.  
**Katya:** Yes.  
**Jay:** And you've got core devs, You've got you know, kind of PandaOps or, you know, testing individuals. So, like, that would probably like, it sound based off from what I'm hearing from you, you're thinking about, like, you know, having having a structure that sort of off of, like, where you're at in that life cycle. And then once it's adopted, having more of a functional like, like, the canonical list. Broken up by functionality like Jay was saying. To yeah, it it sort of would help for early adoption. And some of those, just like a lot of VIPs, don't they don't get, you know, actually added to the hard fork. They get SFI'd or sorry. They get CFI, but they don't make it all the way in. And so that would probably happen with some of these metrics. Then they would you know, once they get SFI ed and added to a hard fork, then they would make it into that canonical list.  
**Katya:** So we just need to hear from different sides, like, from researchers from Corcoran. How is it for people who will use this repo? To have to organize it there. Yeah. Anything else we we need to discuss  
**Jay:** No.  
**Katya:** ideas. Now?  
**Jay:** Not from my side. Yeah. I think we could maybe j u and Katya and I, we could catch up next week, talk through Katya, your proposal and, like, work to come up with, you know, like, formalize that something that we could then pull together a call with maybe a a swath of individuals and, you know, like, a user feedback group And get that feedback and just keep iterating on it until we feel like it represents a lot of different voices and has some, like, broad consensus. And then socialize it at ACDT and just try to start working on the implementation part. Yeah.  
**Katya:** Yeah. Exactly. Because the ACDs are busy with the class master doc currently. So we have at least maybe a month. Just to recap everything. We'll just start it, like, on the background. Just building the structure and the processes. Okay. Sounds good. So yeah. We will I know, arrange the next call in the chat, I guess. Alright. Maybe  
**Jay:** Perfect.  
**Katya:** next Friday as well. Yeah. Thank you, everyone.  
**Jay:** Alright.  
**Katya:** Yep. You set set it for an hour, it only takes half an hour.  
**Jay:** Care. Yeah. Last time was a half hour. We're like, we need an hour.  
**Katya:** I was our our first So, yeah, I don't want to discuss.  
**Jay:** Alright. Thanks all. Have a good weekend.  
**Katya:** Thank you. Bye bye. Bye, guys.  
**Jay:** Bye.