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
        - New field: `validator.attestation_subnet_id` - The subnet that the validator propagates the attestation to, assigned to all validators
        - New field: `validator.aggregator_subnet_id` - The subnet that the validator will perform aggregator duty, assigned to a small subset of validators
        - Note that with above, the aggregator re-uses the same ENR as the validator (same instance)
    - **Networking:**
        - New gossipsub topic: `attestation_{subnet_id}` for propagating `SignedAttestation`
        - New gossipsub topic: `aggregated_attestation` for propagating `SignedAggregatedAttestation`
    - **Attester role:**
        - Attesters propagate their individual attestations to `attestation_{subnet_id}` gossipsub topic (in addition to existing `attestation`)
    - **Aggregator role (new):**
        - Aggregators collect attestations from `attestation_{subnet_id}` gossipsub topic and aggregates them into aggregated signatures
        - Aggregators propagate their aggregated signatures to `aggregated_attestation` gossipsub topic
    - **Proposer role:**
        - Proposer listens to `aggregated_attestation` gossipsub topic
        - Proposer MAY listen to `attestation_{subnet_id}` topics and aggregate gossiped signatures if the received aggregated signatures don't cover all desired votes
        - Proposer puts aggregated signatures across subnets into a block (basic concatenation, no recursion)
    - **Aggregation committee size:**
        - **Initial:** 1 aggregation committee with minimum validators, to verify interop
        - **North star:** 1 aggregation committee with 128 validators
    - **Validator count:**
        - **Initial:** 5 validators (1 per client implementation)
        - **North star:** 128 validators (to accommodate a full aggregation committee)

## External configurations & assumptions

- **PQ signature**
    - [Production signature scheme](https://github.com/leanEthereum/leanSpec/blob/main/src/lean_spec/subspecs/xmss/constants.py#L109-L125)
    - Signature size: ~3100 KiB
- **Signature aggregation**
    - Proof size: ~350 KiB
    - Expecting proof size to go down to ~256 KiB in the near term, possibly closer to 128 KiB in the future

## Notable exclusions

- Multiple aggregation committees - planned for the next devnet to derisk complexity
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
