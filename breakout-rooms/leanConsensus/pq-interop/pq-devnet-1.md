# pq-devnet-1: High Level Plan

## Objectives

1. Implement [leanSig](https://github.com/leanEthereum/leanSig) post-quantum signature signing & verification in lean consensus clients
2. Implement naive signature aggregation (concatenating individual signatures)
3. Establish baseline performance metrics for a consensus layer with PQ signatures

## Key targets

- **Slot duration:** 4 seconds
- **Consensus mechanism:** Modified [3SF-mini](https://github.com/ethereum/research/tree/master/3sf-mini)
- **Validator count:** Start with 512 validators and scale up to 1024 validators
- **PQ Signature prameters:**
    - Hash chains: 64
    - Chain length: 8
    - Max lifetime: 2^32
    - Activation time: 2^18
- **Signature aggregation:** Naive aggregation by concatenating individual signatures

### Exclusions

- Key generation: PQ keys are pre-generated
- Dynamic validator set: Validator set is pre-determined and fixed at genesis

## Specification targets

| Specification | Target | Remarks |
| ------------- | ------ | ------- |
| leanSpec      | [leanEthereum/leanSpec@050fa4a](https://github.com/leanEthereum/leanSpec/tree/050fa4a18881d54d7dc07601fe59e34eb20b9630) | - For specification-related changes, see [all pq-devnet-1 _spec_ PRs](https://github.com/leanEthereum/leanSpec/pulls?q=is%3Apr+is%3Amerged+label%3Aspecs+milestone%3Apq-devnet-1) <br />- For all changes including tests and framework, see [all pq-devnet-1 PRs](https://github.com/leanEthereum/leanSpec/pulls?q=is%3Apr+is%3Amerged+label%3Aspecs+milestone%3Apq-devnet-1) |
| leanSig | TBD until ssz PR is merged | |
| leanMetrics   | [leanEthereum/leanMetrics@e077ac2](https://github.com/leanEthereum/leanMetrics/tree/e077ac2a2190a4946e01737b27eb9a5636e6884e) | |

## Configurations

- `validator-config.yaml`: [blockblaz/lean-quickstart@f512c93/validator-config.yaml](https://github.com/blockblaz/lean-quickstart/blob/f512c939201d92be8241169655da05233b5485e5/local-devnet/genesis/validator-config.yaml)

## Interop toolings

| Tool | Link |
| ---- | ---- |
| lean-quickstart | [blockblaz/lean-quickstart@f512c93](https://github.com/blockblaz/lean-quickstart/tree/f512c939201d92be8241169655da05233b5485e5)

## Client support status

| Client | Implementation | Spec tests | Interop | Code      | Docker |
| ------ | -------------- | ---------- | ------- | --------- | ------ |
| Ream   | ✅ | ⏳ | ✅ | [ReamLabs/ream@149141b](https://github.com/ReamLabs/ream/commit/149141bf25fd68ab7cd54e82fdc08989d92071d8) | [ethpandaops/ream:master-149141b](https://hub.docker.com/layers/ethpandaops/ream/master-149141b)
| Zeam   | ✅ | ⏳ | ✅ | |
| Qlean  | ✅ | ⏳ | ✅ | |
| Lantern | ⏳ | ⏳ | ⏳ | |
| Lighthouse | ⏳ | ⏳ | ⏳ | |

## Benchmarks

- Setup
  - Hardware specs: TBD
- Configurations
  - 3 validators, 1 machine
  - 64 validators, 1 machine
  - 512 validators, 10 machines
  - 1024 validators, 10 machines
- Results
  - [leanMetrics](https://github.com/leanEthereum/leanMetrics) collected and analyzed. Links to results to be added here once completed.

## Summary and learnings

- To be added once devnet is complete
