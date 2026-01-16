# pq-devnet-3: High Level Plan

## Objectives

1. Decouple aggregation from block production by introducing a separate aggregator role
2. Establish protocol for propagating aggregated signatures
3. Lay foundation for exploring different aggregation and propagation strategies

## Key functionalities & targets

- **Existing**
    - **Slot duration:** 4 seconds
    - **Consensus mechanism:** Modified [3SF-mini](https://github.com/ethereum/research/tree/master/3sf-mini)
    - **PQ Signature:** [leanSig](https://github.com/leanEthereum/leanSig)
    - **Signature aggregation:** [leanMultisig](https://github.com/leanEthereum/leanMultisig)
- **Changes**
    - **validator-config.yaml:**
      - New field: `is_aggregator` - A boolean ENR record indicating whether the validator has the aggregator duty in its attestation subnet. Each attestation subnet must have 1 validator assigned as the aggregator. If field is missing we assume the node is not an aggregator.
      - Example new validator config for aggregator:
    ```yaml
    - name: node_0
        privkey: 0000000000000000010000000000000002000000000000000300000000000000
        enrFields:
          ip: 10.0.0.0
          quic: 10000
          is_aggregator: true
    ```

    - **Networking:**
        - New ENR metadata field: `is_aggregator`, follows previous explanation
        - Every validator now belongs to one of the attestation subnets. `subnet_id` is defined by `validator_id % subnets_count` formula to ease debugging. In future devnets it will be replaced by the random assignment.
        - New gossipsub topic: `attestation_{subnet_id}` for propagating `SignedAttestation`
        - New gossipsub topic: `aggregated_attestation` for propagating `SignedAggregatedAttestation`
        - Note: attesters still publish and subscribe to the global `attestation` topic for safe target computation
    - **Attester role:**
        - Attesters propagate their individual attestations to new `attestation_{subnet_id}` gossipsub topic and existing `attestation` topic
        - Attesters do not need to subscribe to `attestation_{subnet_id}` topic if they are not aggregators, i.e. they only publish into the topic. This is to save some bandwidth by not receiving the same attestation both in the subnet and global attestation topics.
    - **Aggregator role (new):**
        - Aggregators collect individual attestations from `attestation_{subnet_id}` gossipsub topic and aggregates them into aggregated signatures
        - Aggregators propagate their aggregated signatures to `aggregated_attestation` gossipsub topic
        - Aggregators do not perform recursive aggregation in this devnet
    - **Proposer role:**
        - Proposer listens to `aggregated_attestation` gossipsub topic
        - Proposer puts aggregated signatures across subnets into a block (basic concatenation, no recursion)
        - Proposer MAY additionally aggregate any attestation that is not yet aggregated (and not equivocating) to a block
    - **Slot intervals:**
        - Intervals
            | Interval | Proposer | Attester | Aggregator |
            |----------|----------|----------|------------|
            | 0 | - Publishes a block | - Immediately accept new attestations from published block |  |
            | 1 |  | - Create and gossip attestations | - Propagate valid attestation to attestations topic corresponding to the committee of aggregator |
            | 2 | - Compute safe target with 2/3+ majority | - Compute safe target with 2/3+ majority | - If >X% of attestations from the committee is collected, compute & propagate an aggregation |
            | 3 | - Accept accumulated attestations (new → known) <br />- Update head based on new attestation weights | - Accept accumulated attestations (new → known) <br />- Update head based on new attestation weights | - If >X% of attestations was not collected and no other aggregation proving >X% of attestations from the same committee was observed, compute & propagate an aggregation from existing attestations |
        - New attestations are collected throughout all intervals by all roles
    - **Aggregation subnet size:**
        - **Initial:** 1 aggregation subnet with minimum validators, to verify interop
        - **North star:** 1 aggregation subnet with 128 validators
    - **Validator count:**
        - **Initial:** 5 validators (1 per client implementation)
        - **North star:** 128 validators (to accommodate a full aggregation subnet)

## External configurations & assumptions

- **PQ signature**
    - [Production signature scheme](https://github.com/leanEthereum/leanSpec/blob/main/src/lean_spec/subspecs/xmss/constants.py#L109-L125)
    - Signature size: ~3100 KiB
- **Signature aggregation**
    - Proof size: ~350 KiB
    - Expecting proof size to go down to ~256 KiB in the near term, possibly closer to 128 KiB in the future

## Notable exclusions

- Multiple aggregation subnets & committees - planned for the next devnet to derisk complexity
- Recursive aggregation - deferred to future devnets

## Completion target

2026-02-25 (TBC)

## Specification targets

| Specification | Target | Remarks |
| ------------- | ------ | ------- |
| leanSpec      | TBD | |
| leanSig       | TBD | |
| leanMultisig  | TBD | |
| leanMetrics   | TBD | |

## Configurations

- `validator-config.yaml`: TBD

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
