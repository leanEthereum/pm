# PQ Interop - Meeting 06

## Meeting Overview
**Date:** August 20, 2025  
**Agenda:** https://github.com/ethereum/pm/issues/1690  
**Recording:** https://youtu.be/Bmn9lY90UGw  

---

## Meeting Notes

- **Lean Specs Discussion Recap**: Follow-up from the August 19 call with Leap AI, Steel, and security researchers (Sam, Dan). Group agreed to continue using markdown specs while integrating directly into Python specs. Sam to work on SSZ type definitions as a dependency for 3SF mini specs.
- **3SF Mini Specifications**: Gajinder reported that the current PR in the leanSpec repo includes 3SF mini types. A new PR for state transition functions and header processing (to support parent-child chaining) will be submitted by August 22. Ream’s implementation is progressing, with alignment on using zero hashes for SSZ.
- **GossipSub Integration**: Group confirmed using GossipSub v1 for DevNet-0, as it doesn’t require peer scoring yet. Discussions on interop with GossipSub to start once 3SF specs are finalized.
- **Genesis Generation**: PK’s recent PRs provide clarity on Genesis generation requirements. Once specs are finalized, changes will propagate to these PRs. Gajinder and team to update forks of Ethereum Genesis generator and eth-beacon-genesis based on PK’s work.
- **Python Specs Ownership**: Thomas volunteered to take responsibility for maintaining Python specs, starting with small items. Will noted the option to hire a dedicated maintainer if the workload grows. Thomas to assess and report if additional support is needed.
- **SSZ Signature Types**: Debate on signature container size for 3SF mini. Gajinder proposed a variable-length signature (max 4000 bytes, SSZ-serialized) to allow flexibility for future changes (e.g., XMSS vs. Falcon). Justin confirmed XMSS leaf signatures are fixed-size (~3KB), while aggregate signatures (SNARK proofs) are variable. Group agreed to use variable-length signatures with a 4000-byte max for now to freeze the format and support interop testing.
- **Key Generation Strategy**: Mercy presented key generation benchmarks (https://hackmd.io/@bomanaps/HkoenFfYll), but results showed inconsistencies (4.2 keys/minute for 2^11 lifetime, ~14.42 seconds/key). Jun suggested optimizations (e.g., using `RUSTFLAGS='-C target-cpu=native'` and release profile). Mercy shared the repository (https://github.com/bomanaps/hash-sig.git) for review. Group agreed to standardize key generation API, with Benedikt proposing a hashed-layer approach (storing ~100KB per key) for just-in-time generation. Decision to pre-generate keys for DevNet-0 to avoid delays, with further optimization discussions planned for the Cambridge workshop.
- **DevNet-0 Timeline**: August 31 deadline deemed unrealistic. Group proposed September 15 for a single-client network with 3SF mini (no PQ signatures) to test fork choice, with DevNet-0 interop (including PQ signatures) targeted for late September. Plan to iterate for DevNet-1 before the October interop gathering.
- **Project Board**: Will created a GitHub project board (https://github.com/orgs/leanEthereum/projects/1/views/2) to track tasks across repos. Currently private; Will to make it public post-call. Group encouraged to review and prioritize tasks (e.g., SSZ typing system as a blocker for spectests).

**Action Items**:  
- Submit PR for 3SF mini state transition and header processing (Gajinder, by August 22).  
- Update and review PK’s Genesis generation PRs after spec finalization (Gajinder/Camille, by August 27).  
- Optimize key generation benchmarks with release profile and share updated results (Mercy, by August 25).  
- Standardize key generation API, propose parameters for hashed-layer approach (Benedikt/Justin, by August 27).  
- Take ownership of Python specs and report on workload capacity (Thomas, by August 27).  
- Make GitHub project board public and share for review (Will, by August 21).  
- Circulate meeting minutes and recording (Will, by August 22).  

---

## Meeting Transcript

### Introduction and Greetings
**Will:** Welcome to PQ Interop Meeting 06. The goal of this call series is to serve as a touchpoint for researchers and client devs during the iterative process of developing, deploying, testing, and improving a series of devnets focused on aggregating post-quantum signatures. This breakout room supports the longer-term lean consensus vision. The agenda is at https://github.com/ethereum/pm/issues/1690. We had a poll yesterday on lean specs and best practices with Leap AI, Steel, and security folks Sam and Dan. Any high-level takeaways from client devs or researchers?

### Lean Specs Discussion
**Gajinder:** From yesterday’s call, we’ll continue with markdown specs while integrating directly into Python. Sam is working on SSZ type definitions for 3SF mini. Our current PR in leanSpec includes 3SF mini types, and I’ll submit a PR for state transition and header processing by Friday to handle parent-child chaining. Ream’s implementation is on track. We’re using GossipSub v1 for now, as it doesn’t need peer scoring. PK’s PRs give us a clear path for Genesis generation, and we’ll update those once specs are finalized. We need someone dedicated to Python specs—Thomas volunteered, but we may need more support later.

**Will:** Thanks, Gajinder. I’ve added a GitHub project board at https://github.com/orgs/leanEthereum/projects/1/views/2 to track tasks across repos. It’s private now, but I’ll make it public after the call. Thoughts on using this to drive progress?

**Ladislaus:** It’s a great board. Yesterday, we discussed with Ladislaus prioritizing and setting timelines to hit a signature DevNet by early October. We should assign priorities, like SSZ typing, to unblock specs.

**Ream:** Zeam and Ream aren’t blocked on Python SSZ types since we’re coding independently, but we’ll need Python-generated spectests for interop. I spoke with Justin in Australia post-call. We’re using the remerkleable library as a workaround for SSZ types, though it’s unmaintained. It works for now, but we need advice from Zeam on maintaining it long-term.

**Thomas:** I’ll take responsibility for Python specs, starting with small items. If it grows too much, I’ll let you know, Will. Hiring a dedicated maintainer is an option.

**Will:** Sounds good, Thomas. Let me know if it becomes a burden. We can revisit hiring someone.

### SSZ Signature Types
**Ream:** On the container specs, I proposed bytes32 for signatures for simplicity, but Partha and others suggested a proper signature type. Should we mimic PQ signatures now or stick with bytes32 to test fork choice first, then add PQ signatures later?

**Gajinder:** Bytes32 is fine, but a variable-length signature (max 4000 bytes, SSZ-serialized) gives flexibility for future changes, like XMSS to Falcon. It clarifies parameters and lets signature teams start early without changing the block structure later.

**Justin:** XMSS leaf signatures are fixed at ~3KB, but aggregate SNARK signatures are variable unless we enforce constant size. A variable-length signature with a 4000-byte max makes sense for now. We can compute worst-case sizes and adjust parameters later.

**Thomas:** The current XMSS parameters are solid for the short to medium term. Long-term, we might explore Falcon, but no changes are planned now. Variable-length signatures are safer.

**Will:** Okay, let’s go with variable-length signatures, max 4000 bytes, to freeze the format.

### Key Generation Strategy
**Will:** Last week, we debated pre-generating keys versus on-demand. Any consensus?

**Mercy:** I ran benchmarks at https://hackmd.io/@bomanaps/HkoenFfYll. For a batch of 10 keys at 2^11 lifetime, it took 144 seconds, averaging 14.42 seconds/key, or ~4.2 keys/minute. The numbers seem off, possibly due to running in dev profile on Gitpod.

**Jun:** Mercy, you likely ran `cargo run` without `RUSTFLAGS='-C target-cpu=native'` or release profile, missing parallelization. Share your repo so we can check optimizations.

**Mercy:** It’s at https://github.com/bomanaps/hash-sig.git. I’ll rerun with release profile and share results.

**Benedikt:** Key generation shouldn’t be a bottleneck. With a hashed-layer approach, we store ~100KB per key and regenerate portions just-in-time. For DevNet-0, we’ll pre-generate keys to avoid delays. We need a standardized API—maybe discuss at the Cambridge workshop.

**Justin:** Agreed, it’s about standardizing parameters. The Rust library needs an API for key count, lifetime, and storage location. It’s a small effort if we align on specs.

**Will:** Let’s pre-generate keys for DevNet-0 and refine the API by next week.

### DevNet-0 Timeline and Wrap-Up
**Ream:** August 31 is too aggressive. I propose September 15 for a single-client network with 3SF mini to test fork choice, then late September for DevNet-0 interop with PQ signatures. We can iterate for DevNet-1 before October.

**Gajinder:** Zeam is okay with September 15. We’ll finalize specs and tooling to support that.

**Will:** Sounds good. Let’s target September 15 for the single-client network and late September for DevNet-0. I’ll make the project board public today and share minutes tomorrow. DM me for board access. Anything else?

**Mercy:** I’ll rerun benchmarks with optimizations by Monday.

**Will:** Great. Thanks, everyone. Have a nice day!

**Participants:** Bye!