# pq-devnet-0: High Level Plan

## Objectives

While practically a pre-devnet without PQ signatures yet, this phase established critical foundations for multi-client coordination and our iterative development approach:

1. Establish new leanSpec framework
2. Establish first client specs
3. First trial of client interops (and consensus)
4. Initial metrics setup

### Targets

- **Slot duration:** 4 seconds
- **Networking transport:** QUIC
- **Gossip:** Gossipsub v1.0
- **Consensus mechanism:** Modified [3SF-mini](https://github.com/ethereum/research/tree/master/3sf-mini)
- **Infrastructure:** Local machines only

### Key exclusions

- PQ signatures are not implemented

## Specification targets

| Specification | Target | Remarks |
| ------------- | ------ | ------- |
| leanSpec      | [leanEthereum/leanSpec@4b750f2](https://github.com/leanEthereum/leanSpec/tree/4b750f2748a3718fe3e1e9cdb3c65e3a7ddabff5) | |

## Interop toolings

| Tool | Link |
| ---- | ---- |
| local-pq-devnet | [ReamLabs/local-pq-devnet@6152686](https://github.com/ReamLabs/local-pq-devnet/tree/6152686d44e1fcd498ad9b079f244832fc58e521) |
| lean-quickstart | TBC |

## Client support status

| Client | Implementation | Interop | Code      | Docker |
| ------ | -------------- | ------- | --------- | ------ |
| Ream   | ✅ | ✅ | [ReamLabs/ream@0bceaee](https://github.com/ReamLabs/ream/commit/0bceaee39b85482e650b48645c314c4a5dbbbcfd) | [ethpandaops/ream:master-0bceaee](https://hub.docker.com/layers/ethpandaops/ream/master-0bceaee)
| Zeam   | ✅ | ✅ | | |
| Qlean  | ✅ | ✅ | | |
