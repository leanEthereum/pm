# Lean Day - Post-Quantum Signatures

| Item | Details |
| ---- | ------- |
| **Session** |  Post-Quantum Signatures for Beam Chain (remote presentation) |
| **Speaker(s)** | Dmitri Khovratovich (DK), Ethereum Foundation - Cryptography |
| **Slides** | [link](https://docs.google.com/presentation/d/1yeZBv6dLwU2A7O_lLRdBEKfhpADEMS4ftqD2c9bWxN0/edit?usp=sharing) |
| **YouTube** | [link](https://youtu.be/-4WTyy9t73M?feature=shared) |

>back to: [Lean Day Overview Page](/workshops-and-interops/2025/lean-day-EthCC/00-index.md)

---

## Post-Quantum Signatures for Beam Chain

### Introduction and Apologies

**DK**: So I'll start. Yeah. Okay. Hello, everyone. Sorry I can't be there at Beam Day. Unfortunately, I had some complications due to my recent brown heights, but hope that my presentation will be informative enough. So please tell me immediately if there is any connection problem. Otherwise, I'll proceed.

So this is kind of a little technical talk about post-quantum signatures for the Beam chain. This is a joint work with Justin, with Mike, Dankrad, and Ferenc Wagner, and others from the cryptography team. So I'll try to make it less technical, but still interesting enough for participants.

---

## Requirements for Post-Quantum Signatures

**DK**: So basically how we started—we needed a simple signature scheme, which would be easy to implement, to test, to analyze. We need the minimum number of hardness assumptions. I mean, cryptographic hardness assumptions. I mean, the assumptions that are necessary to prove the security of the scheme.

We need the scheme to be resistant to quantum attacks. The signature should be reasonably small size. Well, we know that for post-quantum signatures, they cannot be like super small, but still reasonably small. We need fast signing and verification because all this is under the pressure of slot timing. And we need those signatures to be aggregation-friendly currently, because we don't plan to have them all in the blockchain, but we have to have some aggregation of these signatures because there are like many validators per slot and having all the signatures in plain text would just seem too much overhead.

---

## Our Solution: XMSS (Hash-Based Signatures)

**DK**: And so with these requirements, we have come up with a simple solution. Shortly, it's called XMSS. It's a hash-based signature scheme. And its advantage is that it uses just a collision-resistant and preimage-resistant hash function. Nothing more. As long as this hash function is resistant to collision, preimage attacks, and to quantum attacks as well—meaning that the hash is not based on any, for example, elliptic curve machinery—then this scheme is plausibly post-quantum secure.

It's also reasonably small size. It's between two and three kilobytes. I will have some tradeoff between verification time and signature size. It's reasonably fast to sign and verify—really fast. It just takes milliseconds to sign and to verify.

And it's aggregation-friendly. By aggregation, I mean that it doesn't have any internal algebraic structure to make it aggregation-friendly, but we can aggregate it to prove the knowledge of sufficient number of signatures. And to give you some concrete numbers: in order to prove the knowledge of a thousand signatures, it's equivalent to prove just little more than 10,000 Poseidon hashes. And we know that verifying 10,000 Poseidon hashes can be proven in reasonably small time. This will be covered in the next talks.

---

## How the Signatures Work: Toy Example

**DK**: And now I will try to explain how the signatures work. So I'll start with a toy example. In this toy example, I first explain how keys are generated.

### Key Generation

So in order to generate a public key, we create five secret inputs. So this five is just a toy example number—in practice it would be a little bigger. So here the ones are S1, S2, S3, S4, S5. They are random. This is so-called chain beginnings. Then we hash each of those four times, like one, two, three, four, reaching level zero. This forms a chain. And then the resulting hash outputs, we hash them again with a little bigger hash and we get the public key. And that's it. So that's the key generation.

### Signing Process

Now how to sign and what can be signed in this toy example scheme. In this toy example scheme, we sign messages. And these messages are five-tuples, meaning like there are five numbers. And each number is between zero and four (integers). But not all combinations are possible—not all five to the fifth combinations are possible, but only those whose sum is eight.

What that means: here is an example. For example, we can sign a message consisting of five numbers: one, three, two, zero, two. It's easy to see that they sum to eight. And so this one, three, two, zero, two, they give us the positions in these chains. Meaning like here at level one, here at level three, here at level two, at level zero, at level two. The actual signature is the array of values—not the positions, but of values. So these hash outputs. Again, since we have five so-called chains, the signature is composed of five hash values.

And with this scheme, we can sign several messages. For example, we can also sign the message which consists of again, five numbers. But now they are four, zero, zero, zero, four. And then again, I would have these five orange... balls are signature values.

### Verification Process

In order to verify this signature, it's enough to hash all the chains till their end. And it's easy to see that since the sum of positions is eight, then for every message, we have to apply the hash exactly eight times. So now then we get chain ends. And then we hash them again and check if they match the public key.

It's also easy to see that from any actual signature, you cannot modify it to get a signature for another message with sum of positions also eight, because in one of the chains, you would have to kind of step back in the hash chain to find the preimage, which is impossible.

### Scaling to Practical Security

And then for this, for example, since we have only five chains and each chain has five entries, we can only sign 330 messages with the sum requirement of eight. But if we modify this a little bit—and this is actually very close to the construction we have—then for 64 chains of length eight, and if we require that the sum of positions is 76, then we can sign almost exactly 2^128 messages. Well, a little more actually because we need this little bigger number for security proofs, but around that. And this actually gives us 120 bits of security.

So the actual scheme will be very close to this. We'll have 64 chains, meaning that the actual signature will consist of 64 values.

---

## Converting to Multiple-Time Signatures

**DK**: So this was a one-time signature. And for how to convert it to multiple-time signature, that's actually very easy because in this particular Beam chain application, we know that each validator signs for a particular slot and all the signatures are enumerated. For one slot, there can be only one signature, and then it's relatively easy to have multiple signatures from one key.

What we do: we actually just create a one-time signature for every slot. If we have like 2^32 or 2^24 slots, we create an appropriate number of one-time signatures and then we make a public key out of them by just making a Merkle tree out of all of them.

This means that in order to verify a signature—a one-time signature—it's enough to verify it up until the leaf on the tree. And then we need another Merkle proof part to verify it after the very end of the public key. And this part can actually be amortized in practice.

---

## Actual Signature Generation Process

**DK**: So this is how actual signatures are generated. So suppose a validator wants to sign a block B at some slot number V. Then it hashes B and V and gets positions from W1 to W64—positions on this chain structure, which we also call a hypercube. And these positions determine which signature values have to be provided.

So how does the validator make them up? It recalls basically how the chain beginnings were generated. It iterates up to the orange positions and it outputs them as a signature. So 64 values—64 chain values are the signature, well plus the Merkle tree part up above.

### Verification Process

And in order to verify the signature again, you just have to compute the hash chains up until the ends and then to the Merkle root. In order to do this, you just have to compute about 136 Poseidon hashes and that's it. Well, this number 136 varies a little bit. If you need many more slots, then if you just double the number of slots, the number of hashes and the Merkle proof increased by just one.

### Aggregation Process

And in order to aggregate this—in order to prove the existence of signatures—you just put all the signature verification process into the circuit. Basically, what you do is to prove the existence of a signature: you hash block and slot number and you get the positions, and then the signature exists if there exist chain entries from chain one to chain 64 such that if you hash them till the end an appropriate number of times—and this number of times is determined by the hash of block and slot number—and then there exists the Merkle proof till then, then that means that the signature exists. And that's it basically.

---

## Academic Work and Future Specs

**DK**: Basically, the theoretical part behind this construction is in two papers that are just published on arXiv at these links. They don't contain the full scheme that will be actually used—it will be in the final report which is just to be finalized. We're kind of wrapping up with some security proofs and some concrete parameters. Based on this, it will be published very soon. Based on this, we'll write the spec, which will be good enough to make code out of this.

That's it. Thank you. Are there questions?

---

## Q&A Session

### Question 1: Alternative Design Choices

**Audience Member 1**: What were the other choices in the designs and why did you choose this one as opposed to...

**DK**: The question is what were the choices when designing post-quantum signatures and why was the hash-based construction selected.

Right. So we of course are aware of a number of post-quantum constructions, particularly lattice-based. So what we were afraid of is that while the analysis of lattice-based schemes still continues, there are a lot of discussions about security of these schemes, how to interpret the security.

Also, what we do like is that we really have the smallest number of hardness assumptions—particularly just the security of a hash function. So if we already have to have a secure hash function, then it means that we can get a secure signature scheme without any additional assumption. So if we use a lattice-based scheme, then we need to assume something lattice-specific as a hardness assumption. If we just use a secure hash, we don't need anything else.

And also the hash-based signature scheme is much easier to verify in circuits because it's better suited to the domain where proof systems are built. Of course, lattice schemes are still competitive and we think that it's a viable alternative, but simplicity of this scheme and its minimal requirements and ease of implementation, I think give it a little advantage. That's it.

### Question 2: Key Rotation and Size

**Audience Member 2**: How often do you think we should rotate the public keys and how big are they?

**DK**: How often should we rotate the public keys and how big are they? Yeah, so for multiple signatures, the number of public keys determines how big the Merkle tree is. So I think we can support up to 2^24 slots easily. I think we can even support 2^32 slots, which should be sufficient for all practical purposes.

Well, this will make the key generation a little more expensive—maybe several days because you have to basically calculate all these hash chains. So that means 2^32 times you have to calculate all these, like 64 chains, 500 hash calls or something. So in total, yeah, hundreds of billions of hashes, which is feasible, but takes some hours. But overall I think it's possible.

So there shouldn't be some aggressive variants where you have like really small trees and you rotate the keys pretty often, but I think we can avoid it. So maybe we should aim for bigger trees, but cache this part, the upper part in some smart ways to reuse it in proofs and other parts because actually the outer part is not confidential at all in contrast to these blue things below.

Any number between 2^24 and 2^32, which is required, I think we can support.

**DK (Interjection)**: So one of the strategies that we've taken recently is to introduce two different concepts. There's the concept of a maximum lifetime, which is, for example, 2^32 slots, which would be a century, even if we have one-second slots. And then there's the actual lifetime that you would generate practically speaking as a validator, and here what we would recommend to the clients is maybe only generate eight years of lifetime, which is 2^24 keys, and that would take about three hours on your laptop. But yeah, if you want to generate 100 years of lifetime, then that's up to you to just spend more time on your own.

### Question 3: Hash Mapping to Target Sum

**Audience Member 3**: The technical question is: when you hash the block and slot, how do you make sure that that maps to one of these particular arrays that you need? I missed that part.

**DK**: Yeah, great question. So the question is when you're hashing the message and the slot, how do you make sure that you have the target sum.

So what we actually have here—we hash the block and slot into some kind of long string. Then we can convert this long string into an integer. And this integer, we—well, simply saying we take it modulo the number of possible output positions. So here, like how many positions so that they sum to 76 on 64 chains exist? There are about 2^125 or something. So we know this number. This is just a combinatorial thing. So we take hash modulo 2^something basically, and then based on the result we figure out in which position we are by applying again recursively combinatorial things.

So it's basically the question how to generate all the... it doesn't require grinding. The grinding allows us to hit the target sum. Oh, well. Yes and no, so we can do this with grinding, and grinding allows us to use like target sum, which is a little smaller than the needed, but I think it's too complex to explain this.

So we just map a big integer into the positions. So it's the question of enumerating all arrays of integers whose sum is equal to some constant. And this is a combinatorial thing—how to generate all possible enumerations. And in our second paper, there is an exact algorithm that does that. It's a kind of simple combinatorial thing with modular reductions and that's it.

There is indeed a little grinding over it, but I think it's not required to explain how to do it.

### Question 4: Position Generation Clarification

**Audience Member 4**: One last question. Why do you need to generate all the positions? Why not just the one that represents your...

**DK**: No, I meant that in order to convert a big integer into the positions, we need an algorithm that maps one-to-one. And this algorithm is sort of similar to the algorithm that generates all possible positions.

And actually, the algorithm that maps is like—well, for example, if you have like a big integer, how do you write it in base 10 or base 16 or base 5? You do some modular reduction, subtractions, divisions, modular reduction, subtractions, divisions. Here is about the same, just a little more sophisticated because of this structure of this hypercube, but the principle is about the same.

**DK**: Thank you.

**Speaker 1**: Next up, we have Emil who will talk about recursive aggregation.

---

*[End of DK's presentation]*

*Note: Transcript generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*