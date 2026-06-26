# pq-devnet-5: High Level Plan

## Objectives

1. Validate block-level aggregation that enables each block to contain exactly one proof covering all per-message `attestation_data` aggregates
2. Validate proof decomposability — per-message proofs remain recoverable from the block-level proof without re-aggregation

## Key functionalities & targets

- **Existing**
  - **Slot duration:** 4 seconds
  - **Slot interval:** 5 intervals of 800ms each
  - **PQ signature:** [leanSig](https://github.com/leanEthereum/leanSig)
  - **Signature aggregation base:** [leanVM](https://github.com/leanEthereum/leanVM)
  - **Per-message aggregation:** Recursive aggregation per `attestation_data` via `leanVM`
  - **Validator keys:** Each validator maintains separate attestation and proposer keys

- **Changes**
  - **Multi-message aggregation:**
    - Proposers include exactly one aggregation proof per block, covering all `attestation_data` messages in that block, and the proposer signature.
    - Aggregation is performed via `leanVM`'s multi-message aggregation: a single `Proof([message_0, slot_0], …, [message_n, slot_n])` is produced from the per-message aggregates.

  - **Proof recomposition:**
    - The multi-message aggregation proof is decomposable. An aggregator may recover an individual `Proof([message_i, slot_i])` from a multi-message proof in a block to aggregate more signatures for a specific message.

  - **Role behavior updates:**
    - **Aggregator:** Continues per-message coalescing. Forwards per-message aggregates to the proposer for block-level merging.
    - **Proposer:** Performs the final multi-message aggregation across all `attestation_data` in the block to produce the single block-level proof.
    - **Verifier:** Validates that each block contains exactly one block-level proof and that it verifies against the union of participant sets across all included messages.

## Notable exclusions

- Goldfish and other consensus algorithm modifications. They were deferred for later devnets to focus on PQ signature aggregation.

## Completion target

TBD

## Specification targets

| Specification | Target | Remarks |
| ------------- | ------ | ------- |
| leanSpec      | `main` branch | - For specification-related changes, see [all pq-devnet-5 spec PRs](https://github.com/leanEthereum/leanSpec/pulls?q=is%3Apr+is%3Amerged+label%3Aspecs+milestone%3Apq-devnet-5) <br />- For all changes including tests and framework, see [all pq-devnet-5 PRs](https://github.com/leanEthereum/leanSpec/pulls?q=is%3Apr+is%3Amerged+milestone%3Apq-devnet-5) |
| leanSig       | TBD | |
| leanVM        | `devnet5` branch | |
| leanMetrics   | TBD | |

## Benchmarks

- Hardware specs: TBD (based on [EIP-7870](https://eips.ethereum.org/EIPS/eip-7870) full node specs)
- Configurations: TBD
- Results
  - [leanMetrics](https://github.com/leanEthereum/leanMetrics) collected and analyzed. Links to results to be added here once completed.

## Summary of found problemas and learnings

### Block proof aggregation taking too long

- Block proof aggregation during block proposal is the bottleneck. We see it takes multiple slot intervals, and sometimes it takes more than one slot.

#### Proposed solution

* Optimizations to try on the proposer side:
  * Limit the number of attestations during block building to bound the block aggregation time (time budgeting)
  * Start building during the last slot interval, to double the time budget
  * Move the proposer signature out of the block proof. This reduces the number of recursions during block aggregation, practically for free, since the signature is small and quick to verify. We expect this to halve proposal latency for blocks with a small number of attestations (1 or 2). This can be done at the client level (and we suggest so) or inside leanVM as a prover optimization.

### Divergence from latest spec

- We've seen client interop problems due to divergence from the latest spec.

#### Proposed solution

Have spec releases. That way clients have an easy way to notice consensus breaking changes.

### Hive tests failing

Hive fails silently because of harness errors. We have been fixing them as they appear, but haven't yet seen a succesful run.
