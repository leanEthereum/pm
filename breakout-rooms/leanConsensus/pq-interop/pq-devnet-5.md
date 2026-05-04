# pq-devnet-5: High Level Plan

## Objectives

1. Validate block-level aggregation that enables each block to contain exactly one proof covering all per-message `attestation_data` aggregates
2. Validate proof decomposability — per-message proofs remain recoverable from the block-level proof without re-aggregation
3. Validate PQ heartbeat under Goldfish fork-choice replacing 3SF-mini and temporarily operating without an on-chain finality gadget
4. Validate feasibility of using gossip-only intermediate aggregation proofs for PQ heartbeat

## Key functionalities & targets

- **Existing**
  - **Slot duration:** 4 seconds
  - **Slot interval:** 5 intervals of 800ms each
  - **PQ signature:** [leanSig](https://github.com/leanEthereum/leanSig)
  - **Signature aggregation base:** [leanMultisig](https://github.com/leanEthereum/leanMultisig)
  - **Per-message aggregation:** Recursive aggregation per `attestation_data` via `leanVm`
  - **Validator keys:** Each validator maintains attestation and proposer keys

- **Changes**
  - **Consensus mechanism:**
    - 3SF-mini mechanism is removed entirely. Finality gadget integration (e.g. Minnimit, Simplex) is deferred to a future devnet.
    - Head selection follows [Goldfish](https://ethresear.ch/t/unblocking-faster-finality-with-decoupled-consensus/24527), an LMD-GHOST variant with vote expiry and view-merge.

  - **PQ heartbeat (committee-based block production):**
    - For each slot, an `X`-validator committee is sampled. Sampling method is a protocol-level configuration option.
      - Valid options: random sampling, or fixed per-client allotment (each client gets a configured number of committee seats).
    - The slot's proposer (drawn from the committee) builds a block.
    - The committee votes on the proposed block. Goldfish's fork-choice picks the canonical head.
    - Block propagates; the next slot starts. The result is a constant-latency confirmation that is reorg-resilient under honest proposals.

  - **Parallel finality-vote test bed:**
    - In parallel with the committee path, the full validator set casts finality votes targeting the latest heartbeat tip.
    - These votes:
      - Propagate through the p2p network.
      - Are aggregated through the same `leanMultisig`/`leanVm` pipeline used for committee attestations.
      - Are NOT consumed by any gadget. They do not finalize blocks. They do not influence fork-choice.
    - The sole purpose of this path is to stress-test PQ signature aggregation against full-validator-set traffic before any future finality gadget consumes it.

  - **Block-level multi-message aggregation:**
    - Proposers include exactly one block-level aggregation proof per block, covering all `attestation_data` messages in that block.
    - Aggregation is performed via `leanVm`'s multi-message aggregation: a single `Proof([message_0, slot_0], …, [message_n, slot_n])` is produced from the per-message aggregates.
    - The resulting block-level proof is decomposable: any later party MAY recover an individual `Proof([message_i, slot_i])` from the block-level proof, without re-aggregation. No proof tree is required to expose per-message proofs to downstream consumers.

  - **Block contents:**
    - Each block contains exactly one block-level aggregation proof.
    - Multiple block-level proofs in a block MUST be rejected at block validation time.
    - The previous "one aggregate per message" structure (pq-devnet-4) is replaced by this single block-level proof.

  - **Role behavior updates:**
    - **Aggregator:** Continues per-message coalescing. Forwards per-message aggregates to the proposer for block-level merging.
    - **Proposer:** Performs the final multi-message aggregation across all `attestation_data` in the block to produce the single block-level proof.
    - **Verifier:** Validates that each block contains exactly one block-level proof and that it verifies against the union of participant sets across all included messages.

## Notable exclusions

- Finality gadget integration (Minnimit, Simplex, or other). Goldfish runs without a finality gadget in this devnet.
- Goldfish ↔ finality-gadget interaction semantics.
- Dynamic committee assignment changes beyond the configured sampling method.
- Changes to validator churn/activation logic.

## Completion target

TBD

## Specification targets

| Specification | Target | Remarks |
| ------------- | ------ | ------- |
| leanSpec      | TBD | - For specification-related changes, see [all pq-devnet-5 spec PRs](https://github.com/leanEthereum/leanSpec/pulls?q=is%3Apr+is%3Amerged+label%3Aspecs+milestone%3Apq-devnet-5) <br />- For all changes including tests and framework, see [all pq-devnet-5 PRs](https://github.com/leanEthereum/leanSpec/pulls?q=is%3Apr+is%3Amerged+milestone%3Apq-devnet-5) |
| leanSig       | TBD | |
| leanMultisig  | TBD | |
| leanMetrics   | TBD | |

## Configurations

- `validator-config.yaml`: TBD
  - Example committee-eligible aggregator entry:

    ```yaml
    - name: node_0
      attestation_privkey: 0000000000000000010000000000000002000000000000000300000000000000
      proposer_privkey: 0000000000000000040000000000000005000000000000000600000000000000
      enrFields:
        ip: 10.0.0.0
        quic: 10000
        is_aggregator: true
        committee_seats: 1
    ```

- Heartbeat committee parameters (protocol-level, TBD):
  - `COMMITTEE_SIZE`: `X` (TBD)
  - `COMMITTEE_SAMPLING`: `random` | `per_client_fixed` (TBD)

## Interop toolings

| Tool | Link |
| ---- | ---- |
| lean-quickstart | TBD |

## Client support status

| Client | Implementation | Spec tests | Interop | Code      | Docker |
| ------ | -------------- | ---------- | ------- | --------- | ------ |
| Ream   | ⏳ | ⏳ | ⏳ | | |
| Zeam   | ⏳ | ⏳ | ⏳ | | |
| Qlean  | ⏳ | ⏳ | ⏳ | | |
| Lantern | ⏳ | ⏳ | ⏳ | | |
| Lighthouse | ⏳ | ⏳ | ⏳ | | |

## Benchmarks

- Hardware specs: TBD (based on [EIP-7870](https://eips.ethereum.org/EIPS/eip-7870) full node specs)
- Configurations: TBD
- Results
  - [leanMetrics](https://github.com/leanEthereum/leanMetrics) collected and analyzed. Links to results to be added here once completed.

## Summary and learnings

- **Single block-level aggregation proof:** Each block carries exactly one aggregation proof covering all `attestation_data` messages, replacing the per-message-aggregate model of pq-devnet-4. The proof is decomposable, so per-message proofs remain recoverable by later proposers without an explicit aggregation tree.
- **Reduced block size and bandwidth:** Collapsing per-message aggregates into a single block-level proof minimizes the proof component of the block, the largest contributor to block size in pq-devnet-4.
- **PQ heartbeat replaces 3SF-mini:** A small per-slot committee votes on the proposed block, and Goldfish fork-choice picks the canonical head from those votes. The proposer (drawn from the committee) builds the block. 3SF-mini is dropped entirely from the devnet, giving constant-latency, reorg-resilient confirmation under honest proposals — without on-chain finality, which is deferred to a future devnet.
- **Full validator set as PQ aggregation test bed:** Finality votes from the full validator set propagate, aggregate, and are then discarded. They do not finalize anything and do not influence fork-choice. This isolates PQ signature aggregation at full-validator-set scale as a measurable target before any finality gadget consumes the votes.
- **Remaining limitation:** No finality gadget runs in this devnet. Goldfish ↔ finality-gadget interaction (Minnimit, Simplex, or other) is the natural follow-up for a future devnet once those interactions are understood.
