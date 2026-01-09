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
| leanSpec      | [leanEthereum/leanSpec@main](https://github.com/leanEthereum/leanSpec/tree/main/) (TBC) | - For specification-related changes, see [all pq-devnet-2 _spec_ PRs](https://github.com/leanEthereum/leanSpec/pulls?q=is%3Apr+is%3Amerged+label%3Aspecs+milestone%3Apq-devnet-2) <br />- For all changes including tests and framework, see [all pq-devnet-2 PRs](https://github.com/leanEthereum/leanSpec/pulls?q=is%3Apr+is%3Amerged+label%3Aspecs+milestone%3Apq-devnet-2) |
| leanSig | [leanEthereum/leanSig@adad238](https://github.com/leanEthereum/leanSig/tree/adad23877977b4e992df10d39abc03594a8d63a6) (TBC) | |
| leanMultisig | [leanEthereum/leanMultisig@main](https://github.com/leanEthereum/leanMultisig) (TBC) | 2025-10-11: leanMultisig is currently incompatible with leanSig. Researchers are working to enable full compatibility for devnet 2. |
| leanMetrics   | [leanEthereum/leanMetrics@main](https://github.com/leanEthereum/leanMetrics/) (TBC) | |

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

- Hardware specs
  - CPU: 4 cores / 8 threads with ~1000 ST / 3000 MT CPU rating
  - Memory: 32 GB
  - Bandwidth: 50 Mbps down / 15 Mbps up
  - Assumption: Based on [EIP-7870](https://eips.ethereum.org/EIPS/eip-7870) full node specs
- Configurations (TBC)
  - 5 validators, 1 machine (5 clients x 1) - interop check
  - 100 validators, 1 machine (5 clients x 20 validators/client)
  - 200 validators, 2 machines (5 clients x 20 validators/client x 2 machines)
  - 400 validators, 4 machines (5 clients x 20 validators/client x 4 machines)
  - Assumption: A machine can run up to 100 validators in stable condition
- Results
  - [leanMetrics](https://github.com/leanEthereum/leanMetrics) collected and analyzed. Links to results to be added here once completed.

## Summary and learnings

- To be added once devnet is complete
