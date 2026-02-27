# pq-devnet-4: High Level Plan

## Objectives

1. Enable recursive PQ signature aggregation using `leanVm`
2. Coalesce multiple aggregates for the same message into one final aggregate
3. Ensure blocks contain a single aggregate per message instead of multiple aggregates with different `participantValidators`

## Key functionalities & targets

- **Existing**
  - **Slot duration:** 4 seconds
  - **Consensus mechanism:** Modified [3SF-mini](https://github.com/ethereum/research/tree/master/3sf-mini)
  - **PQ signature:** [leanSig](https://github.com/leanEthereum/leanSig)
  - **Signature aggregation base:** [leanMultisig](https://github.com/leanEthereum/leanMultisig)
- **Changes**
  - **`validator-config.yaml`:**
    - New optional field: `log_inv_rate` (integer, aggregator-only).
    - Valid values: `1` to `4`.
    - This field is set only when `is_aggregator: true`.
    - Semantics:
      - `1`: biggest proof size, fastest proof construction.
      - `4`: smallest proof size, slowest proof construction / most compute intensive.
    - If omitted for aggregators, clients use default as 2.
    - Non-aggregator validators MUST ignore this field.
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
| leanSpec      | TBD | - For specification-related changes, see [all pq-devnet-4 spec PRs](https://github.com/leanEthereum/leanSpec/pulls?q=is%3Apr+is%3Amerged+label%3Aspecs+milestone%3Apq-devnet-4) <br />- For all changes including tests and framework, see [all pq-devnet-4 PRs](https://github.com/leanEthereum/leanSpec/pulls?q=is%3Apr+is%3Amerged+label%3Aspecs+milestone%3Apq-devnet-4) |
| leanSig       | TBD | |
| leanMultisig  | TBD | |
| leanVm        | TBD | |
| leanMetrics   | TBD | |

## Configurations

- `validator-config.yaml`: TBD
  - Example aggregator entry:

    ```yaml
    - name: node_0
      privkey: 0000000000000000010000000000000002000000000000000300000000000000
      enrFields:
        ip: 10.0.0.0
        quic: 10000
        is_aggregator: true
      log_inv_rate: 4
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

- To be added once devnet is complete
