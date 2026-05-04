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
  - **Per-message aggregation:** Recursive aggregation per `attestation_data` via `leanMultisig`
  - **Validator keys:** Each validator maintains separate attestation and proposer keys

- **Changes**
  - **Finality:**
    - 3SF-mini mechanism is removed entirely.
    - Finality gadget integration is deferred to a future devnet.

  - **PQ heartbeat & fork choice:**
    - Head selection follows [Goldfish](https://ethresear.ch/t/unblocking-faster-finality-with-decoupled-consensus/24527#p-59290-goldfish-1), an LMD-GHOST variant with vote expiry and view-merge.
    - For each slot, an `X`-validator committee is sampled. Sampling method is a protocol-level configuration option.
      - TBD: random sampling, or fixed per-client allotment (each client gets a configured number of committee seats).
    - The slot's proposer (drawn from the committee) builds and publishes a block.
    - Each attester collects committee votes from the latest block's aggregation proof, from the `aggregation` gossipsub topic, and from its own `attestation_{subnet_id}` topic, applies Goldfish (vote expiry + view-merge) to pick the canonical head, and publishes its own vote.

  - **Gossipsub topics:**
    - Attesters now subscribe to (a) the `aggregation` topic and (b) their own `attestation_{subnet_id}` topic to feed Goldfish (in pq-devnet-3 attesters only published, never subscribed). No new topics are introduced.

  - **Interim finality voting:**
    - The full validator set publishes finality votes targeting the latest heartbeat tip; votes propagate and aggregate through the same `leanMultisig` pipeline used for committee attestations.
    - Votes are not consumed by any gadget — they do not finalize blocks or influence fork-choice. Purpose is to stress-test PQ signature aggregation at full-validator-set scale.

  - **Multi-message aggregation:**
    - Proposers include exactly one aggregation proof per block, covering all `attestation_data` messages in that block.
    - Aggregation is performed via `leanMultisig`'s multi-message aggregation: a single `Proof([message_0, slot_0], …, [message_n, slot_n])` is produced from the per-message aggregates.

  - **Proof recomposition:**
    - The multi-message aggregation proof is decomposable. An aggregator may recover an individual `Proof([message_i, slot_i])` from a multi-message proof in a block to aggregate more signatures for a specific message.

  - **Role behavior updates:**
    - **Aggregator:** Continues per-message coalescing. Forwards per-message aggregates to the proposer for block-level merging.
    - **Proposer:** Performs the final multi-message aggregation across all `attestation_data` in the block to produce the single block-level proof.
    - **Verifier:** Validates that each block contains exactly one block-level proof and that it verifies against the union of participant sets across all included messages.

## Notable exclusions

- Finality gadget integration (Minnimit, Simplex, or other). Goldfish runs without a finality gadget in this devnet.
- Goldfish ↔ finality-gadget interaction semantics.

## Completion target

TBD

## Specification targets

| Specification | Target | Remarks |
| ------------- | ------ | ------- |
| leanSpec      | TBD | - For specification-related changes, see [all pq-devnet-5 spec PRs](https://github.com/leanEthereum/leanSpec/pulls?q=is%3Apr+is%3Amerged+label%3Aspecs+milestone%3Apq-devnet-5) <br />- For all changes including tests and framework, see [all pq-devnet-5 PRs](https://github.com/leanEthereum/leanSpec/pulls?q=is%3Apr+is%3Amerged+milestone%3Apq-devnet-5) |
| leanSig       | TBD | |
| leanMultisig  | TBD | |
| leanMetrics   | TBD | |

## Benchmarks

- Hardware specs: TBD (based on [EIP-7870](https://eips.ethereum.org/EIPS/eip-7870) full node specs)
- Configurations: TBD
- Results
  - [leanMetrics](https://github.com/leanEthereum/leanMetrics) collected and analyzed. Links to results to be added here once completed.

## Summary and learnings

TBD once devnet is complete.
