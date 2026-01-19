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
| leanSpec      | [leanEthereum/leanSpec@fbbacbe](https://github.com/leanEthereum/leanSpec/tree/fbbacbea4545be870e25e3c00a90fc69e019c5bb) | |
| leanSig | [leanEthereum/leanSig@ae12a5f](https://github.com/leanEthereum/leanSig/tree/ae12a5feb25d917c42b6466444ebd56ec115a629) | |
| leanMultisig | [leanEthereum/leanMultisig@4543620](https://github.com/leanEthereum/leanMultisig/tree/4543620b8e33c16112dcf1fff7f250344913a729) |  |
| leanMetrics   | [leanEthereum/leanMetrics@7fac4cc](https://github.com/leanEthereum/leanMetrics/tree/7fac4cc3e3d0bcc5e498f6abd74c80c8245f5591) | |

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
