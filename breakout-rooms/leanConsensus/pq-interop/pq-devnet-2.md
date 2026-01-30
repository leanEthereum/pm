# pq-devnet-2: High Level Plan

## Objectives

1. Implement [leanMultisig](https://github.com/leanEthereum/leanMultisig) PQ signature aggregation
2. Establish baseline performance metrics for PQ signature aggregation under a consensus setting

## Key targets

- **Slot duration:** 4 seconds
- **Consensus mechanism:** Modified [3SF-mini](https://github.com/ethereum/research/tree/master/3sf-mini)
- **Validator count:** 400 validators (assuming [current benchmark](https://github.com/leanEthereum/leanMultisig?tab=readme-ov-file#xmss-aggregation) of almost 400 XMSS/s)
- **Signature aggregation:** [leanMultisig](https://github.com/leanEthereum/leanMultisig)
- **Sync:** Out-of-sync clients are able to sync to chain head by requesting BlocksByRoot from connected peers
- **Completion target:** 2026-01-28

## Specification targets

| Specification | Target | Remarks |
| ------------- | ------ | ------- |
| leanSpec      | [leanEthereum/leanSpec@4edcf7b](https://github.com/leanEthereum/leanSpec/tree/4edcf7bc9271e6a70ded8aff17710d68beac4266) | - For specification-related changes, see [all pq-devnet-2_spec_ PRs](https://github.com/leanEthereum/leanSpec/pulls?q=is%3Apr+is%3Amerged+label%3Aspecs+milestone%3Apq-devnet-2) <br />- For all changes including tests and framework, see [all pq-devnet-2 PRs](https://github.com/leanEthereum/leanSpec/pulls?q=is%3Apr+is%3Amerged+label%3Aspecs+milestone%3Apq-devnet-2) |
| leanSig | [leanEthereum/leanSig@73bedc2](https://github.com/leanEthereum/leanSig/tree/73bedc26ed961b110df7ac2e234dc11361a4bf25) | |
| leanMultisig | [leanEthereum/leanMultisig@e447413](https://github.com/leanEthereum/leanMultisig/tree/e4474138487eeb1ed7c2e1013674fe80ac9f3165) |  |
| leanMetrics   | [leanEthereum/leanMetrics@3b32b30](https://github.com/leanEthereum/leanMetrics/tree/3b32b300cca5ed7a7a2b3f142273fae9dbc171bf) | |

## Configurations

- `validator-config.yaml`: [blockblaz/lean-quickstart@main/validator-config.yaml](https://github.com/blockblaz/lean-quickstart/blob/main/local-devnet/genesis/validator-config.yaml) (TBC)

## Interop toolings

| Tool | Link |
| ---- | ---- |
| lean-quickstart | [blockblaz/lean-quickstart@main](https://github.com/blockblaz/lean-quickstart/tree/main) (TBC) |

## Client support status

| Client | Implementation | Spec tests | Interop | Code      | Docker |
| ------ | -------------- | ---------- | ------- | --------- | ------ |
| Ream   | ⏳ | ⏳ | ⏳ | | |
| Zeam   | ⏳ | ⏳ | ⏳ | | |
| Qlean  | ⏳ | ⏳ | ⏳ | | |
| Lantern | ⏳ | ⏳ | ⏳ | | |
| Lighthouse | ⏳ | ⏳ | ⏳ | | |

## Benchmarks

- Hardware specs: 8 cores / 16 GB memory / 150 GB disk / Debian 13
- Configurations: 5 machines (1 machine per client)

## Summary and learnings

- https://hackmd.io/@katya-blockchain-dev/lean-devnet-1-retrospective
