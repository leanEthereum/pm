# PQ Interop - Meeting 8 Minutes

**Date:** September 03, 2025  
**Agenda:** https://github.com/ethereum/pm/issues/1712
**Recording:** https://youtu.be/buacaj50n_Y?si=e0XOoAi8ab4mT-Jq

## Agenda
- Review status of DevNet zero spec (target: August 31)
- Discuss client implementation updates (Ream, Zeam)
- Update on kurtosis configuration and genesis tooling
- Discuss metrics export and Grafana dashboard progress
- Review fork choice PR and multi-sig spec
- Discuss PQ aggregation simulation results

## Key Discussion Points
1. **DevNet Zero Spec Status**
   - **Target**: August 31 (missed internal commitment).
   - **Ream Update (O)**: Waiting for state transition function (STF) specs to be merged into the repo. Ream team plans to integrate these into their codebase within 3-5 days post-merge. Successfully ran 3SF in a local four-node environment for a few hundred slots.
   - **Zeam Update (Guillaume)**: Merged metrics PR; working on kurtosis configuration and genesis generator with PK. Aiming to test kurtosis setup soon, expecting simplicity due to minimal genesis requirements.
   - **Fork Choice PR**: Gajinder, O, and Jun discussed; mostly a straightforward translation from 3SF mini P2P and Zeam code. Needs cleanup and finalization for merging.

2. **Kurtosis and Genesis Tooling**
   - **Kurtosis Configuration**: Guillaume and PK working on setup; not yet ready but targeted for this week. Kurtosis is critical for ETH Panda Ops support, ensuring standardized testnet deployment.
   - **Genesis Generator**: No need for SSZ genesis file; clients generate genesis state from config (`genesis_time`, `num_validators`). Guillaume awaiting PK’s confirmation on readiness.

3. **Metrics and Grafana Dashboard**
   - **Metrics Export**: Ream and Zeam teams can export metrics; focus is on enabling export capability for Grafana integration, not specific metrics yet. Metrics to be defined post-fork choice spec merge, focusing on consensus and fork metrics.
   - **Grafana Dashboard**: Guillaume to work on YML file today; delayed by debugging. Goal is to enable basic debugging for DevNet zero via Grafana.

4. **Multi-Sig Spec Walkthrough (Thomas)**
   - **Spec Migration to Python**:
     - Organized in `spec` repo under `src` folder:
       - `types`: Basic types (e.g., `uint64`, hash output).
       - `chain`: DevNet config constants (e.g., slot duration).
       - `containers`: Classes for block, block header, block body, checkpoint, config, and state; minimal and linked to markdown docs.
       - `networking`: Constants and gossip/messaging configs.
       - `crypto`: Defines prime field (`koala_bear`) and operations (addition, subtraction, etc.), Poseidon implementation, and XMSS signatures.
     - **Poseidon Implementation**: Uses constants from reference paper; exposes `permutation` function for users, with tests comparing to Plumo.txt library.
     - **XMSS Signatures**: Matches Rust implementation; provides `keygen`, `sign`, and `verify` functions. Clients should use provided code for security, not reimplement.
     - **Constants**: Defined for production and test configs; test config uses smaller parameters for faster key generation (not production-ready).
   - **Open Questions**:
     - Signature and public key sizes to be explicitly noted in constants (e.g., ~3000 bytes for production config).
     - Domain separation included in XMSS spec (Thomas to share specific section).
   - **Future Work**: Encoding for recursive aggregation may change for ZKVM optimization (to be discussed at Cambridge workshop in October).

5. **PQ Aggregation Simulation (Emil)**
   - **Current Results**:
     - Standard topology (100 Mbps, 10 snarks/s): 3.9 seconds for aggregation (unacceptable for 4-second slots).
     - Grid topology: 3.2 seconds.
     - Aggregators-only grid: 3.4 seconds (extra hop adds latency).
   - **Optimizations Tested**:
     - Early announcement of snark creation to reduce duplicates.
     - Limiting snark propagation to one per group.
     - Gossipsub 1.2 with “I don’t want” messages showed minimal improvement due to duplicates already in transit.
   - **Proposed Improvements**:
     - Increase bandwidth to 200 Mbps (based on Moore’s Law, ~50% yearly growth): Reduces grid topology to <3 seconds.
     - Increase compute power to 20 snarks/s: Grid topology achieves 2.3 seconds.
     - Replace TCP with WebRTC (requires optimization due to high simulation time).
   - **Privacy Concerns**: Aggregators learning validator IP addresses; mitigation ideas proposed but need further investigation.
   - **Next Steps**: Share report with networking experts (Raul, Yannis) for Cambridge workshop feedback.

6. **Key Generation and Validator Keys**
   - **Activation Time vs. Lifetime**: Lifetime set to 2^32 for DevNet zero/one; smaller activation times reduce key generation time (1-2 hours in Rust, slower in Python). Test config uses smaller parameters for faster testing.
   - **Proposal**: Fixed lifetime (2^32) to avoid key rotation complexity (e.g., slashing complications). Validators can generate fewer keys for shorter activation periods.
   - **Key Types**: One public key per validator, fixed for lifetime. Proposals may implicitly include attestations to avoid separate attestation signatures; alternatively, split keys for proposals and attestations (doubles keygen time).
   - **Slashing and Whistleblowing**: No signatures needed for whistleblowing (just include slashable messages in block). Proposal signatures handle rewards, avoiding whistleblower reward theft.

7. **Action Items**
   - **Gajinder, O, Jun**: Finalize and merge fork choice PR; integrate STF specs into Ream/Zeam codebases.
   - **Guillaume, PK**: Complete kurtosis configuration and test genesis generation; finalize Grafana YML file.
   - **Thomas**: Add signature/public key size comments to constants; share domain separation spec section; prepare multi-sig spec presentation for lean consensus call.
   - **Emil**: Share PQ aggregation report with Raul/Yannis