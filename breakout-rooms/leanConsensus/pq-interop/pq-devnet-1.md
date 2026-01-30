# pq-devnet-1: High Level Plan

## Objectives

1. Implement [leanSig](https://github.com/leanEthereum/leanSig) post-quantum signature signing & verification in lean consensus clients
2. Implement naive signature aggregation (concatenating individual signatures)
3. Establish baseline performance metrics for a consensus layer with PQ signatures

## Key targets

- **Slot duration:** 4 seconds
- **Consensus mechanism:** Modified [3SF-mini](https://github.com/ethereum/research/tree/master/3sf-mini)
- **Validator count:** 5 validators (1 validator per client)
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
| leanSig | [leanEthereum/leanSig@f10dcbe](https://github.com/leanEthereum/leanSig/tree/f10dcbefac2502d356d93f686e8b4ecd8dc8840a) | |
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
| Ream   | ✅ | ⏳ | ✅ | [ReamLabs/ream@e68ba0d](https://github.com/ReamLabs/ream/tree/e68ba0d) | [ethpandaops/ream:master-149141b](https://hub.docker.com/r/ethpandaops/ream/tags?name=e68ba0d) |
| Zeam   | ✅ | ⏳ | ✅ | [blockblaz/zeam@devnet1](https://github.com/blockblaz/zeam/tree/devnet1) | [blockblaz/zeam:devnet1](https://hub.docker.com/r/blockblaz/zeam/tags?name=devnet1) |
| Qlean  | ✅ | ⏳ | ✅ | [qdrvm/qlean-mini@c8dc1ec](https://github.com/qdrvm/qlean-mini/tree/c8dc1ec) | [qdrvm/qlean-mini:c8dc1ec](https://hub.docker.com/r/qdrvm/qlean-mini/tags?name=c8dc1ec) |
| Lantern | ✅ | ⏳ | ✅ | [piertwo/lantern@devnet1](https://github.com/Pier-Two/lantern/tree/devnet-1) | [piertwo/lantern:v0.0.1](https://hub.docker.com/r/piertwo/lantern/tags?name=v0.0.1) |
| Grandine | ✅ | ⏳ | ✅ | [grandinetech/lean](https://github.com/grandinetech/lean) | [sifrai/lean:devnet-1](https://hub.docker.com/r/sifrai/lean/tags?name=devnet-1) |

## Infrastructure

- Hardware specs: 8-core/16GB/150GB
- Configurations: 5 machines, 1 machine per client

## Summary and learnings

- https://hackmd.io/@katya-blockchain-dev/lean-devnet-1-retrospective
