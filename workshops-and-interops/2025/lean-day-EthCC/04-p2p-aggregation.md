# Lean Day - P2P Signature Aggregation

| Item | Details |
| ---- | ------- |
| **Session** |  P2P Aspects of Sequential Aggregation for Beam Chain |
| **Speaker(s)** | Kamil Salakhiev (KS), Quadrivium Team |
| **Slides** | [link](https://hackmd.io/@kamilsa/SyLrJPGEgl#/) |
| **YouTube** | [link](https://youtu.be/bI6T3h2HMJ4?feature=shared) |

>back to: [Lean Day Overview Page](/workshops-and-interops/2025/lean-day-EthCC/00-index.md)

---

## P2P Aspects of Sequential Aggregation

### Introduction and Context

**KS**: Today I would speak about the P2P aspect of sequential aggregation. I'm from Quadrivium and I will present some efforts from our team and also, well, there's lots of discussions with the EF research team, so you know, as I started.

So yeah, with post-quantum sequential [aggregation], there's really a lot of assumptions that we have today changing. So basically the size of signatures that we have grows from something like 96 bytes to three kilobytes, also the aggregation size increases to 128 kilobytes. Well, the exact numbers still depend on the scheme that we're going to choose, but approximately in this order of magnitude is how it's going to look like.

Also, the aggregation cost increases, so previously the aggregation cost was pretty low. So basically any attester in the network would perform an aggregation, and now it's relatively high because we have to perform SNARK aggregation. And here for that, what is changing is that we cannot anymore just simply rotate who is the current aggregator within a subnet to perform an aggregation. So there should be some process of selecting the aggregator and they're going to be basically the members of the subnet who's currently performing the aggregation.

---

## Network Scale and Architecture

**KS**: Just some numbers that we consider in our work is that we would have something like 2^15 to 2^16 number of validators, so that's basically the amount of signatures that we need to aggregate. Also we have like now 12-second slots. But the general approach I think would be more or less the same.

So basically we have like a **two-tier architecture** where we first begin with subnet aggregation. So within the subnet we perform first aggregation, but instead of BLS aggregation it's going to be SNARK—equals just SNARK proof. And then there are going to be some global aggregation topic where the peers, the global aggregators who would recursively aggregate these SNARKs into some global SNARK which we're going to put on chain.

Yeah, of course that's still a matter of optimization, so maybe we will come up with a better scheme, but yeah, that's how we've projected so far.

---

## P2P Design Space Considerations

**KS**: So this opens up some P2P design space that we consider:

### 1. Subnet Aggregation Strategy
First is how we perform subnet aggregation, for example whether we wait for some certain threshold number of signatures that we want to collect before we actually perform the aggregation. Or maybe we come up with some kind of aggregators collecting some sort of number of signatures, then they aggregate them to create SNARK, propagate it to other participants in the subnet, then they keep updating this SNARK. So yeah, there might be different strategies for how we actually perform this aggregation.

### 2. Subnet Topology
The next thing is subnet topology, like again here we have to kind of agree on some assumptions that we want to choose before we agree on which topology best suits for these needs.

### 3. Inter-Subnet Communication
And the next thing would be how we communicate between subnets so that once we have a subnet aggregate, how do we work out with global aggregators so that we then create the global SNARK.

---

## Key Questions for Subnet Topology

**KS**: So for the subnet topology, there are some key questions that really affect how we're going to choose the best topology that we can use:

### Question 1: Public Addresses
For example, whether we can have assumption that the validators or aggregators addresses are publicly known. Currently it's not the case, so basically because we're using GossipSub, there is no need to presume that the addresses are known. So validators, they just subscribe to subnet aggregator topic and they broadcast their signatures.

### Question 2: All-to-All Signature Distribution
Then there is a question like whether we need all validators to receive all signatures in the subnet. Well, I mean like if we have validators that only perform attestation and they are not performing aggregation, whether it's really needed for them to actually receive the signatures from other participants within the subnet.

### Question 3: Subnet Aggregation Distribution
And the same question like whether validators that they simply attest actually need to receive the subnet level aggregations. Obviously they need to eventually get the final aggregation, but for the subnet aggregation there is a question whether they really need it or not.

Depending on how we answer these questions, I think the best topology [will be determined].

---

## Topology Options

**KS**: In our work we started with some simple **direct topology** where the same validators would create a signature and then there are local aggregators inside the subnet. Their addresses are known so they just can—so a single validator can just connect to them directly and send their signatures. Then aggregators would propagate it to other aggregators and then once they have enough they prepare—they generate a SNARK.

### GossipSub Topology
The other thing that we obviously need to consider is the **GossipSub**—it's a well-adopted tested topology. Here all the attesters and the aggregators they kind of participate in the same topic, kind of what we have now with BLS signature aggregation. We have mesh connections where we actually send signature messages and non-mesh connections where we send different kind of IHAVE/IWANT messages to optimize gossiping.

With that topology, everyone in the topic would receive signatures, receive aggregation, there is no requirement for anyone to actually expose their public address.

### Grid Topology
Another interesting topology that we think might be also considered is **grid**. Without going just deeply into details, with grid we can basically—it is possible to send any signature from any validator to any other validator within at most two hops. So propagation is really fast. Also because there are only two ways how each individual signature can get from one validator to another, there are really at most one duplicate that any participant can get.

But to make it work, each validator should have the same view of this two-dimensional lattice of nodes and for that we need all validators to be aware of other addresses of other validators in the subnet.

### Hybrid Approaches
We can combine these topologies as building blocks and combine them together. Like for example, attesters validators they can send to aggregators which are connected using grid topology and then they propagate the signatures to each other very fast. That way we only need to expose public addresses of aggregators, not the validators.

So there is a lot of design space here.

---

## Global Aggregation Communication

**KS**: Then once we have the subnet aggregation—that is first SNARK—obviously because they have a weight of 128 kilobytes, it doesn't make sense to just simply gossip them. But instead it makes sense to kind of just announce that the aggregator actually has a SNARK.

This could be done in a form of a **bitfield** so that when global aggregator for example receives this bitfield, he can decide whether he needs this SNARK or not, depending on whether global aggregator already observed the SNARK for this subnet. Maybe they already observed the better SNARK, which aggregates more signatures.

So the pull topology, I think, is the way here to implement this.

---

## BEAMS Simulator

**KS**: So yeah, to test all of these assumptions and to implement them, we have generated a very simple simulator and we just go with the name **BEAMS** for now. It is a discrete event simulator, so that means it allows us to actually have very reproducible results. We just supply it with some seed and then if we want to repeat the test we can do this.

### Technical Features
It simulates real TCP and UDP transport beneath and actually scales very well with the number of CPU cores because MPI has very good MPI support. So each subnet we can put into separate process and run in separate CPU core. And there are not much inter-process communication, so only when the final aggregation is done, only then we communicate with other subnets.

### Availability
So yeah, it is available on GitHub, there are instructions on how to... It is C++, there are instructions on how to actually build the project. But if you are lazy to build it, there is a Docker [image] which you can use if you want. It is not as stable—I just checked yesterday. Unfortunately, MPI is not working very stable with Docker and containers, so you can give it a try if you want.

### Configuration Interface
And once you have a project built, it looks something like this—a Jupyter notebook where you can basically configure some default parameters for your network, like a number of validators, a number of local aggregators. For example if you go with GossipSub topology, GossipSub parameters, different cryptographic constants like size of signatures, of SNARKs, how many signatures do we need to collect before we actually perform an aggregation.

What signatures per second, how many signatures we can verify? How many signatures we can aggregate per second, how many SNARKs we can aggregate? And so on, so on.

### Results and Analysis
And then we choose basically the topology that we want to run. And then we can see how long it took for us to generate the SNARKs, like the first SNARKs and then the global SNARKs. And once we have the global aggregators collected enough local SNARKs that they perform an aggregation. And then we see the exact point in time, like when the very first global SNARK was produced.

And then we can also check what was the total network traffic, what was the peak bandwidth for different types of nodes, like validators, local aggregators, global aggregators.

Yeah, I think, yeah, I really hope this will kind of spark some discussions about the design, like what we actually want to implement and check different assumptions. So yeah, if you have any suggestions, please reach out to me or contact me. Yeah, that's all I had. Thanks.

---

## Q&A Session

### Question 1: Simulation Approach Choice

**Audience Member 1**: Can you talk a little bit about your different options for simulating networks, which ones you considered, why you didn't go the other directions? Because in libp2p, right, we're doing concepts of scaling, I know the others is here too, they're doing simulation stuff. There's several options. Why did you go with discrete event?

**KS**: Because this is not what the other libp2p project is. Yeah, I think that's the most asked question, like why not Shadow?

The thing is that with Shadow, we kind of—Shadow is great for just taking, like for example, a real client implementation and just simply replacing the system calls. And that way we can simulate the network. The thing is that we run the real binary. And so we basically have as many processes in the network as many notes we want to simulate.

In the discrete event approach, we basically run the entire subnet in a single CPU core. So we consider this to be more scalable. And also we don't really need to have a real client implementation to test this. We only testing one particular aspect, one small part of the client, which is the signature aggregation part. So we can really optimize for that.

So yeah, in our test, we were able to simulate like 5,000 nodes on a laptop. Yeah, this might take, I don't know, maybe... It depends on the size of the subnet, but 1,000 nodes could take maybe 30 minutes or so. But for smaller subnets, yeah, it runs faster. But you see what's possible to do on a single laptop.

It doesn't consume as much RAM as it would if you used Shadow for the same number of nodes. So yeah, but you see what we can do.

### Integration with Real-World Data
It is great to be able to, because we listen to suggestions like Shadow for example, has this RIPE Atlas database, which has the latencies between the nodes from real world. And we also passed this data from Shadow and took it. So in our simulator, we also use real world latencies to kind of help recreate real environment in the simulator.

Also, yeah, it still simulates TCP/UDP transport. So things like TCP congestion, if we observe them in real network, like if all participants send their message to one exact same aggregator for example, then we will end up with TCP congestion. And our simulator would actually have the same behavior.

That's kind of in contrast to something like maybe SSF mini. Well, it checks the algorithm part, but it doesn't simulate the network and transport as thoroughly as our discrete event simulator.

So yeah, that's why. But yeah, we're open to suggestion here if we can actually try to use Shadow as well. But for now, we thought discrete event actually makes sense here.

### Question 2: Observed Challenges and Results

**Audience Member 2**: Thank you. So I haven't followed what we studied. But what are the challenges that you've seen in the different topologies or the protocols or the different settings? Any, any, any background and results?

**KS**: Yeah. So basically, what we observe is that—what challenges—but we're basically comparing different topologies. Obviously, the direct topology that I showed previously was the fastest, but again, it has certain assumptions that we need to consider.

We noticed that, for example, of course GossipSub actually runs as fast as grid for a relatively small number of nodes in the subnet. For example, here we have just 64 nodes, but if this number grows, for the grid topology, actually, it runs very fast, signature verification time. Then the time of aggregation stays more or less the same for grid topology in contrast to GossipSub, because GossipSub can start having more hops between nodes for communication.

Yeah, well, there are still a few things that we need to improve here for scalability of the simulation, just... The challenge with implementation was like, if we have, if we put, if we have, for example, if we use MPI, and we run, and we have a lot of inter-process communication, then this really slows down the simulation. But if we can avoid this, for example, we don't need to put all local aggregators into the same topics, so that they don't really talk to each other, but they rather speak directly to global aggregators. Then simulation runs much faster. So we are trying to do this thing, for example.

So, yeah, I don't know, the main challenge is, basically, the thing is that we have to just agree with certain assumptions, like which topology we should use, and then, yeah, with this framework, I believe it's easy to implement, and check these assumptions.

So we just hope that this would give some tool for cryptographers just to check their assumptions, like if we use, for example, this SNARK scheme, with this verification time, with this SNARK proof size, what effect this would have, like, what would be the peak traffic consumption for different nodes, and whether we can afford this for our nodes. Yeah.

### Question 3: Latency Simulation Details

**Audience Member 3**: Basically, do you simulate the latency between all nodes, or just between subnet nodes, like what is between subnet nodes?

**KS**: Yeah, we simulate latency between any pair of nodes for communication, so we just take the RIPE Atlas database, which has the real world data for latency between nodes around the world. It has just around 2,000 nodes, but we extrapolate the data so that we have, for our test scenarios, what we run, for example, 5,000 nodes, we still can use it. But yeah, we simulate latency between any pair of nodes, not only within subnet, but also between subnet to global aggregators as well.

**Moderator**: Okay, if that's all, yeah, we're here, so if you have any problem, we'll have another discussion session, so thanks.

**KS**: Thank you, Kamil.

---

*[End of KS's presentation]*

*Note: Transcript generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*