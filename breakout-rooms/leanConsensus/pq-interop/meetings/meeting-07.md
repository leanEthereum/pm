# PQ Interop - Meeting 7 Minutes

**Date:** August 27, 2025  
**Agenda:** https://github.com/ethereum/pm/issues/1698
**Recording:** https://youtu.be/coCfDEnAzSo?si=z4CTv-DyYLRAYiA3


## Agenda
- Review project board updates and specification progress
- Discuss state transition function (STF) specification
- Plan genesis tooling and DevNet zero timelines
- Address signature handling and post-quantum key types
- Discuss infrastructure and dashboard setup for DevNet zero

## Key Discussion Points
1. **Project Board Updates**
   - **O’s Container PR**: Merged successfully.
   - **STF PR**: Gajinder’s state transition function PR discussed with O and Jun; mostly complete (98% covered), needs minor refinements (e.g., defining helper functions like `get_justification` and `set_justifications`).
   - **Fork Choice PR**: Gajinder and O to collaborate on finalizing this PR with follow-up discussions.

2. **State Transition Function (STF) Walkthrough**
   - **Features**:
     - Chaining, 3SF mini justification, and finalization.
     - No signatures (empty signatures), no aggregation, round-robin proposals, simplified validators (weight of 1, no deposit/activation/withdrawal/slashing).
   - **Genesis Generation**:
     - Requires only `genesis_time` and `num_validators`.
     - Simplified genesis generation based on beacon chain model: sets `genesis_time`, `num_validators`, and empty block header with body root.
   - **STF Details**:
     - Signature validation is external to keep STF lightweight for provers (temporary due to ZKVM considerations).
     - Process slots similar to beacon chain, without epoch transitions.
     - Process block includes header and operations (attestations/votes only).
     - 3SF mini-specific: Genesis at slot 0, block 1 as first block; justification map tracks validators justifying roots.
     - Helper functions (`get_justification`, `set_justifications`) to be defined for SSZ state flattening.

3. **Genesis Tooling**
   - **Decision**: No need for an ETH-2 genesis generator; clients can generate genesis state from config (`genesis_time`, `num_validators`).
   - **Ethereum Genesis Generator**: Generates a single standard config for all clients, eliminating client-specific configs.
   - **Key Pairs for DevNet 1**: Need a CLI to generate public/private key pairs for validators; to be added to the signature library for programmatic use.

4. **Timelines for DevNet Zero and One**
   - **August 31**: DevNet zero spec completion.
   - **September 5**: Genesis tooling ready (config generation).
   - **September 12**: Python-based spec tests ready.
   - **September 19**: Pass all spec tests.
   - **September 26**: Successful interop for DevNet zero.
   - **October 3**: Tooling ready for DevNet one.
   - **Note**: Strict deadlines to ensure client interop.

5. **Signature Handling**
   - **Current PR**: Updates container specs for XMSSS signatures (path, randomness, hashes); simplified for DevNet zero (no signatures implemented).
   - **Proposal**: Flatten signature container to a byte vector (e.g., max 4KB) to avoid client handling of internal structures; simplifies implementation for DevNet zero and one.
   - **Open Questions**:
     - Key types for attestation and block signatures (post-quantum group discussion).
     - Exact byte sizes for signatures and public keys (e.g., Merkle root size for hash scheme).
   - **Hashing Decision**: Use SHA-256 for SSZ in DevNet zero; consider Poseidon 2 for later DevNets to optimize proving cycles.

6. **Infrastructure and Dashboard**
   - **DevNet Infrastructure**: Handled by ethPandaOps (Gom); Ream team on standby.
   - **Docker Compatibility**: Ream to ensure compatibility; Quadrivium to be added.
   - **Grafana Dashboard**:
     - Guillaume tasked with coordinating dashboard setup with DevOps.
     - Current YML file (Mercy’s) for graphite dashboard to be reviewed by Ream and other teams.
     - Goal: Unified dashboard for monitoring head slots, justified slots, finalized slots, and block processing metrics.
     - Quadrivium contact established for Docker image building.
   - **Forkmon**: Preferred over Dora for DevNet zero to monitor fork status and client agreement; simpler alternative.

7. **Client Participation**
   - **Kamil’s Team**: Plans to participate in DevNet series but limited by resources (two people); finishing sub-SSZ integration.

8. **Action Items**
   - **Gajinder**: Finalize STF PR refinements, verify correctness in Zeam code, collaborate with O on fork choice PR.
   - **O**: Work on Python spec tests (by September 12), collaborate on fork choice PR.
   - **Guillaume**: Coordinate with DevOps on Grafana dashboard, get Ream and Quadrivium feedback on YML file, create issue for dashboard agreement.
   - **PK/GM**: Own Ethereum genesis generator for config files and ENR file; CLI for PQ key pair generation.
   - **Mercy**: Review CLI for key pair generation, discuss with Benedict for signature library integration.
   - **All Teams**: Review dashboard YML, provide feedback to Guillaume; prepare for DevNet zero interop (September 26).

9. **Open Questions**
   - Exact byte sizes for signatures and public keys.
   - Key store format for post-quantum keys (EIP discussion).
   - Poseidon 2 integration timeline (DevNet one or later).
   - Final dashboard format and metrics for DevNet zero.

## Next Steps
- **Spec Completion**: Finalize DevNet zero spec by August 31; start DevNet one spec discussions (target September 5).
- **Spec Tests**: O to lead Python spec test development by September 12.
- **Dashboard**: Guillaume to finalize dashboard YML with team feedback and coordinate with DevOps.
- **Interop Preparation**: All teams to align on config generation and test passing by September 19 for interop on September 26.
- **Next Call**: Multi-IG spec discussion (Emil and Thomasr presenting); review Reamaining DevNet zero tasks.