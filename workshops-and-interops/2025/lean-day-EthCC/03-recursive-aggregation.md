# Lean Day - Recursive Signature Aggregation

| Item | Details |
| ---- | ------- |
| **Session** |  Post-Quantum Recursive Signature Aggregation for Beam Chain |
| **Speaker(s)** | EHe Hautefeuille (EH), Ethereum Foundation |
| **Slides** | [link](https://docs.google.com/presentation/d/1xI_Js_F2bkiK3lZTgODBQrpO5YYtLO7jzpAvLTCHywk/edit?usp=drive_link) |
| **YouTube** | [link](https://youtu.be/jd4JzX4xDvU?feature=shared) |

>back to: [Lean Day Overview Page](/workshops-and-interops/2025/lean-day-EthCC/00-index.md)

---

## Post-Quantum Recursive Signature Aggregation for Beam Chain

### Introduction and Context

**EH**: Hello everyone. I'm going to give you a talk about post-quantum recursive signature aggregation for the Beam chain. So first of all, part of it is partially opinionated and reflects my current opinion.

As Dmitri explains, Poseidon-based XMSS signatures is a good candidate to replace BLS, which had very nice aggregation properties, which unfortunately does not resist quantum computers. The question is then how to aggregate those XMSS—so hash-based signatures—because we need this aggregation property for Ethereum consensus system.

And there are multiple ways to do it, and in this talk we're going to focus on SNARKs, particularly hash-based SNARKs to make it plausibly post-quantum.

---

## Algorithms to SNARKify

**EH**: What algorithms do we want to SNARKify? Two of them actually:

### 1. Aggregate Algorithm
First, **aggregate**, which basically proves that you have knowledge of some signatures corresponding to ones in a bitfield and that you have sequentially verified all these signatures. And when you create a SNARK proof of this algorithm, you end up with a small aggregation proof.

### 2. Merge Algorithm  
And second algorithm, **merge**, which basically consists in aggregating the aggregates. So you can re-aggregate already aggregated signatures. But doing this, we'll have to verify a SNARK inside a SNARK because we need to verify the first SNARK proof of the aggregate algorithm, which is recursion.

---

## SNARK Design Considerations

**EH**: The question is then how to design an efficient yet simple SNARK. First possibility would be to have constraint systems, but doing this for, in particular, the merge algorithm is cumbersome and not possible. So probably something more like a minimal zkVM—we can think of something like Cairo, but optimized for our use case, which is aggregating signatures and which actually consists in verifying a lot of hashes.

### Polynomial Commitment Scheme (PCS) Choice

Which PCS to use? Probably FRI because we have shorter proofs than PLONK and we want to target short proofs around 128 kilobytes. And additional advantage of FRI is that it's a multilinear polynomial commitment scheme, which is useful because if you can—in a FRI-based STARK, you have to commit to each column as a separate univariate polynomial. Using a multilinear PCS, you can commit to the whole air table as a single multilinear polynomial. And this will additionally help to reduce the proof size.

### Arithmetization Choice

Which arithmetization to use? Probably AIR. So I'll be working on the AIR because the transition constraints are very natural to represent the execution of an opcode.

### Field Choice

Which field to use? Probably again a small field because this will lead to fewer rounds in Poseidon. And because we would use FRI—a multilinear PCS—we would not use the classical univariate coefficient ring in STARKs. And we can replace this with a sumcheck-based PIOP. And with the help of the univariate skip from LogUp, we can totally remove the embedding that you previously had using small fields in the sumcheck that you had like another hash that appears after extension. And you can totally remove this embedded descent overhead using the univariate skip.

And which field in particular? Probably **Goldilocks** because the 2^64 map is an automorphism of the multiplicative group. And this means that Poseidon is more efficient and particularly easier to prove.

### Goldilocks Field Considerations

The only drawback of Goldilocks is that it has a low 2-adicity, meaning that you can only commit to less data. So you cannot prove a SNARK on a big statement. But if you use an extension degree which is a power of two, you can increase this 2-adicity. And for instance, if we use a degree-8 extension field, we gain 2-adicity up to 2^37, which is enough to prove validity of two million Poseidon permutations.

And additional advantage of degree-8 extension field would be the security level because with a degree-4, it's impossible—even though you have a field of the size of the order of 128 bits, it's impossible to get this level of security 128 bits. You only get approximately 100 bits of security even with the conjectural security for the DEEP-ALI soundness in the random oracle model. Actually you could use the STARK conjectures but that's something you want to achieve in a concrete probability.

---

## Instruction Set Architecture (ISA)

**EH**: In terms of ISA, probably not RISC-V—we don't need all these opcodes, we only prove Poseidon. Something much simpler, probably closer to Cairo. So being able to add, multiply, and to manipulate memory. Very few registers—for instance, Cairo only has three registers: PC, AP, and FP.

### Memory Model Debate

And there is a debate around the memory. There are two types of memory: read-only and read-write memory. Read-write makes it easier to write a program but is more expensive to prove. So personally I would argue for read-only—and I'm 100% sure, I think read-only would offer better performance.

### Precompiles

We would also need precompiles for our computationally intensive tasks. Of course, Poseidon precompile, but we could also potentially need precompiles to do the operations in the extension field because in the merge algorithm we need to verify the SNARK inside the SNARK. And verifying a SNARK happens in the extension field for some of the reasons, and so that's why we would have in this part a lot of extension field operations. And that's why it's potentially useful to have precompiles for the extension field add and multiply.

---

## Potential Architecture

**EH**: Here is a sketch of a potential architecture: both fancy main table for the execution, tables for precompiles, and memory with memory buses (aka lookups) to communicate information.

---

## Index Lookup Arguments

**EH**: To conclude, I want to highlight a paper by Lasso, which is an index lookup argument. What is an index lookup argument basically? You have a column containing data and you have another column containing indexes, and you want to prove that this third column corresponds to correct indexes.

So for instance, one here—it corresponds. But in previous lookups, for instance LogUp GKR, you have to commit to these three columns and then you prove that the lookup is correct. But with this recent paper, you can avoid committing to this column and only commit to a column of this size T, which is particularly useful when the column T is much smaller than the column of the indexes, which is the case for the bytecode because each opcode will be read/accessed multiple times because of the loops.

So for instance, if this column represents the bytecode, the columns of the program counter and the output code will be much longer than the bytecode itself. So avoiding committing to this one may be interesting for us. Additionally in Cairo, you have to commit to numerous boolean columns that represent the logical bits of the opcode, and so you would avoid committing to lots of columns using this index lookup argument.

That's it for me. And here is a link to some implementations related to this topic. Happy to take questions again.

---

## Q&A Session

### Question 1: Custom Instruction Set

**Audience Member 1**: You said you were exploring your custom instruction set—do you think it would be something like Windows doing or like a computer?

**EH**: Yeah, I think something much simpler probably. I think we need to take into account the SNARK optimizations away from Cairo. Cairo is very, very simple to understand and to prove. We already have this part. We already have a PCS implemented with the Goldilocks field. We have PCS, the multilinear PCS. We have Poseidon, we have the PIOP. And we already have multilinear PIOP able to prove validity of Poseidons. We can currently do more than 100—actually we could do, I don't know how—we could almost reach 200 Poseidons on a CPU, maybe a medium with CPU.

The thing that remains to be done is this part with the execution table and memory and the lookups. So we have to add this.

### Question 2: Field-Specific Poseidon

**Audience Member 2**: I have a very good question about Poseidon. When you talk about PIOP, does it mean that Poseidon is PIOP-friendly?

**EH**: Yes.

**Audience Member 2**: So does it mean that in some sense it means it's Goldilocks-specific Poseidon? Because if someone uses Baby Bear or Mersenne prime, they would need to emulate PIOP and then they would sacrifice all the others.

**EH**: In this system we would have Poseidon over Goldilocks and the proof system over Goldilocks. Every single one. Everything.

**Audience Member 2**: So in some sense it's... which makes sense. It's okay that it gives advantage to one specific prime.

**EH**: Exactly. We would have to choose that. For instance, in Baby Bear, the 2^32 map which is a bijection and an automorphism—Baby Bear is 2^31, so it's much more expensive than Goldilocks.

### Question 3: Alternative Approaches

**Audience Member 3**: How about Circle STARKs? Did you check all other alternatives?

**EH**: I don't know for Circle STARKs, but I think if we could avoid the conceptual complexity of Circle STARKs, it would be great, I guess. If you can get enough performance with a classic Goldilocks...

**Audience Member 3**: How about Plonky3?

**EH**: I think some people left talking. So this is shared confidentially, but the RISC Zero people have pivoted just a few days ago and they are no longer going to be pushing Plonky3 as hard.

---

*[End of EH's presentation]*

*Note: Transcript generated in part with the help of AI so names, technical terms, etc may not be 100% accurate.*