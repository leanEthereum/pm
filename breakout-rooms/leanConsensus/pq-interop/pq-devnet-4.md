# pq-devnet-4: High Level Plan

## Objectives

1. Introduce proposer keys for validators (attestation key + proposer key), enabling block signing
2. Enable recursive aggregation per message using `leanVm`
3. In-block aggregation by proposers — aggregate all proof payloads for a given `attestation_data` into a single aggregated payload
4. Enforce a `MAX_ATTESTATION_DATA` limit of 16 unique `attestation_data` per block

## Key functionalities & targets

- **Existing**
  - **Slot duration:** 4 seconds
  - **Consensus mechanism:** Modified [3SF-mini](https://github.com/ethereum/research/tree/master/3sf-mini)
  - **PQ signature:** [leanSig](https://github.com/leanEthereum/leanSig)
  - **Signature aggregation base:** [leanMultisig](https://github.com/leanEthereum/leanMultisig)

- **Changes**
  - **`log_inv_rate` (protocol-level config):**
    - `log_inv_rate` is a protocol-level configuration option, not a per-validator setting.
    - Valid values: `1` to `4`.
      - `1`: biggest proof size.
      - `4`: smallest proof size.
    - All nodes use the protocol-defined value.

  - **Recursive aggregation via `leanVm`:**
    - `leanVm` performs recursive aggregation over partial aggregates that correspond to the same message.
    - Aggregates are progressively merged until a single aggregate remains per message.

  - **Aggregate coalescing rules:**
    - Multiple aggregates with different `participantValidators` sets for the same message are merged into one aggregate.
    - The final aggregate MUST represent the union of all non-equivocating participant validators included in valid partial aggregates.

  - **Block contents:**
    - Proposers include exactly one aggregate per message in block body.
    - Duplicate aggregates for the same message MUST be rejected at block validation time.

  - **Networking and mempool behavior:**
    - Nodes MAY receive multiple partial aggregates for the same message over gossip.
    - Aggregators SHOULD coalesce compatible partial aggregates before forwarding and propagate.
    - Conflicting or invalid aggregates are ignored according to existing attestation validity rules.

  - **Role behavior updates:**
    - **Aggregator:** Continues collecting attestations/partial aggregates and recursively merges them.
    - **Proposer:** Prioritizes the best available coalesced aggregate per message for inclusion.
    - **Verifier:** Validates that each message has at most one aggregate in block and that the aggregate verifies against its participant set.

## Notable exclusions

- Dynamic committee assignment changes
- Changes to validator churn/activation logic
- Multi-message proof batching beyond one aggregate-per-message semantics

## Completion target

2026-03-25 (TBC)

## Specification targets

| Specification | Target | Remarks |
| ------------- | ------ | ------- |
| leanSpec      | [`0c9528a`](https://github.com/leanEthereum/leanSpec/commit/0c9528ac6f403f913caf6d9c450879c36d361742) | - For specification-related changes, see [all pq-devnet-4 spec PRs](https://github.com/leanEthereum/leanSpec/pulls?q=is%3Apr+is%3Amerged+label%3Aspecs+milestone%3Apq-devnet-4) <br />- For all changes including tests and framework, see [all pq-devnet-4 PRs](https://github.com/leanEthereum/leanSpec/pulls?q=is%3Apr+is%3Amerged+label%3Aspecs+milestone%3Apq-devnet-4) |
| leanSig       | [`5cc7e37`](https://github.com/leanEthereum/leanSig/commit/5cc7e37480362f94e86695428a9ceb9a96b66b97) | |
| leanMultisig  | [`2eb4b9d`](https://github.com/leanEthereum/leanMultisig/commit/2eb4b9d983171139af36749f127dd9890c9109e6) | |
| leanMetrics   | TBD | |

## Configurations

- `validator-config.yaml`: TBD
  - Example aggregator entry:

    ```yaml
    - name: node_0
      attestation_privkey: 0000000000000000010000000000000002000000000000000300000000000000
      proposer_privkey: 0000000000000000040000000000000005000000000000000600000000000000
      enrFields:
        ip: 10.0.0.0
        quic: 10000
        is_aggregator: true
    ```

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

- **Proposer keys:** Each validator now maintains two distinct keys (attestation key and proposer key), enabling block signing.
- **Recursive aggregation per message:** Proofs for a single `attestation_data` can be recursively aggregated, producing a single aggregated payload per `attestation_data`.
- **In-block aggregation by proposers:** During block construction, proposers aggregate all proof payloads for a given `attestation_data` and produce a single aggregated payload. A constant `MAX_ATTESTATION_DATA` limits the number of unique `attestation_data` included in a block to 8.
- **Reduced block size and bandwidth:** With one aggregated payload per `attestation_data` instead of multiple proofs, the number of payloads per block and overall block size are reduced, decreasing network bandwidth usage and improving block propagation efficiency.
- **Remaining limitation:** A block still contains multiple aggregated payloads (one per `attestation_data`), and proofs remain the largest component of the block. This motivates further compression via block-level proof aggregation (multi-message aggregation) in devnet-5.
