# pq-devnet-3: Readiness Checklist

> **Target Date:** 2026-02-25
> **High Level Plan:** [pq-devnet-3.md](./pq-devnet-3.md)
> **Source of Truth:** [HackMD](https://hackmd.io/@bomanaps/rJjEPTjBWx)

## Specification Readiness

- [ ] High-level plan [PR #56](https://github.com/leanEthereum/pm/pull/56) merged
- [ ] leanSpec: Committee aggregation [PR #282](https://github.com/leanEthereum/leanSpec/pull/282) merged
- [ ] compute_subnet_id() function [PR #58](https://github.com/leanEthereum/pm/pull/58) merged
- [ ] Aggregator role fully specified
- [ ] New gossipsub topics defined (`attestation_{subnet_id}`, `aggregated_attestation`)
- [ ] GossipSub propagation predicates for topic message acceptance/rejection

## PR #282 Remaining Work

- [ ] Handle aggregators not observing sufficient signatures by interval 2
- [ ] Add gossipsub propagation predicates
- [ ] Rebase to latest main

## Client Implementation Readiness

| Client     | Aggregator Role | Subnet Topics | Interop |
| ---------- | --------------- | ------------- | ------- |
| Lantern    | ⏳              | ⏳            | ⏳      |
| Ream       | ⏳              | ⏳            | ⏳      |
| Zeam       | ⏳              | ⏳            | ⏳      |
| Qlean      | ⏳              | ⏳            | ⏳      |
| Lighthouse | ⏳              | ⏳            | ⏳      |
| Grandine   | ⏳              | ⏳            | ⏳      |
| EthLambda  | ⏳              | ⏳            | ⏳      |
| Gean       | ⏳              | ⏳            | ⏳      |

## New Functionality

### Aggregator Role

- [ ] Collect attestations from subnet topic
- [ ] Aggregate signatures (no recursive aggregation)
- [ ] Propagate to `aggregated_attestation` topic

### Proposer Updates

- [ ] Listen to `aggregated_attestation` topic
- [ ] Include aggregations in block

## Infrastructure Readiness

- [ ] validator-config.yaml with `is_aggregator` field
- [ ] lean-quickstart updated for subnet topology
- [ ] Grafana dashboards for subnets

## Go/No-Go Criteria

- [ ] PR #282 merged
- [ ] All clients implement aggregator role
- [ ] Aggregated signatures included in blocks
- [ ] No regression in finality

## Out of Scope

- Multiple aggregation subnets → DevNet 4
- Recursive aggregation → Future
- Random validator-to-subnet assignment → Future
- Dynamic validator set (deposit/exit) → DevNet 4+

## Deferred from DevNet 2

- [ ] (To be filled based on DevNet 2 outcome)

## Open Items

| Item | Type | Relevance |
| ---- | ---- | --------- |
| [PR #282](https://github.com/leanEthereum/leanSpec/pull/282) | WIP | Committee aggregation |
| [PR #56](https://github.com/leanEthereum/pm/pull/56), [PR #58](https://github.com/leanEthereum/pm/pull/58) | Merged | High-level DevNet 3 plan |
