# pq-devnet-2: Readiness Checklist

> **Target Date:** 2026-01-28
> **High Level Plan:** [pq-devnet-2.md](./pq-devnet-2.md)
> **Source of Truth:** [HackMD](https://hackmd.io/@bomanaps/rJjEPTjBWx)

## Specification Readiness

- [x] leanSpec: All pq-devnet-2 PRs merged [Devnet 2 Commit Hash](https://github.com/leanEthereum/leanSpec/tree/4edcf7bc9271e6a70ded8aff17710d68beac4266)
- [ ] leanSig: Target commit validated and compatible [Devnet 2 Commit Hash](https://github.com/leanEthereum/leanSig/tree/73bedc26ed961b110df7ac2e234dc11361a4bf25)
- [ ] [leanMultisig](https://github.com/leanEthereum/leanMultisig/tree/e4474138487eeb1ed7c2e1013674fe80ac9f3165): Compatible with leanSig (**BLOCKER**)
- [x] [Justification compression PR merged](https://github.com/leanEthereum/leanSpec/pull/273)
- [x] [Checkpoint sync spec finalized](https://github.com/leanEthereum/leanSpec/pull/279)
- [x] [Zero hash in justification roots refactor](https://github.com/leanEthereum/leanSpec/pull/305)
- [ ] Block-by-root syncing spec finalized
- [ ] Lean VM aggregation integrated
- [x] Lean Metrics: [DevNet 2 branch](https://github.com/leanEthereum/leanMetrics/tree/3b32b300cca5ed7a7a2b3f142273fae9dbc171bf)

## Metrics Readiness

- [PR #18](https://github.com/leanEthereum/leanMetrics/pull/18)
- [PR #16](https://github.com/leanEthereum/leanMetrics/pull/16)
- [PR #13](https://github.com/leanEthereum/leanMetrics/pull/13)

## Bug Fixes Required (from DevNet 1)

- [ ] Node syncing (graceful recovery without full restart)
- [ ] Genesis block checkpoint alignment
- [ ] Memory leak fixes
- [ ] Peer connection fixes
- [ ] SSZ implementation alignment

## Client Implementation Readiness

*Status as of Jan 28, 2026*

| Client     | Status   | Docker Tag | Notes |
| ---------- | -------- | ---------- | ----- |
| EthLambda  | ✅ Ready | `ghcr.io/lambdaclass/ethlambda:latest` | Gossip layer done, STF added |
| Grandine   | ✅ Ready | `sifrai/lean:latest` | Open-sourced, DevNet-1 interop ongoing |
| Lantern    | ✅ Ready | `piertwo/lantern:latest` | Full DevNet-2 support, all tests passing |
| Lighthouse | ✅ Ready | `hopinheimer/lighthouse:latest` | - |
| Qlean      | ✅ Ready | `qdrvm/qlean-mini:latest` | OOM fix PR open, GossipSub predicates WIP |
| Ream       | ✅ Ready | `ghcr.io/reamlabs/ream:latest` | Checkpoint sync API pending, DB pruning done |
| Zeam       | ✅ Ready | `blockblaz/zeam:latest` | Block-by-root syncing working |
| Gean       | 🆕 New   | TBD | Go + Lean based client |

## Infrastructure Readiness

- [x] lean-quickstart updated
- [ ] Grafana dashboards ready
- [x] All client Docker images submitted to [readiness issue](https://github.com/blockblaz/lean-quickstart/issues/87)
- [ ] Docker images include [commit hash metadata](https://github.com/blockblaz/lean-quickstart/issues/91)
- [x] Default Docker image configuration ([Issue #94](https://github.com/blockblaz/lean-quickstart/issues/94))

## Go/No-Go Criteria

- [ ] leanMultisig <-> leanSig compatibility resolved
- [ ] All clients pass spec tests
- [ ] Network runs 1+ hour without finality loss
- [ ] Node can rejoin after disconnect

## May Defer to DevNet 3

- [ ] Committee-based aggregation ([PR #282](https://github.com/leanEthereum/leanSpec/pull/282))
- [ ] Multiple aggregation subnets
- [ ] Recursive aggregation

## Open Items

| Item | Type | Relevance |
| ---- | ---- | --------- |
| leanMultisig compatibility | Blocker | leanSig compatibility |
| [PR #299](https://github.com/leanEthereum/leanSpec/pull/299) | Merged | Interop testing |
| [Issue #87](https://github.com/blockblaz/lean-quickstart/issues/87) | Open | Client Docker images submission |
| [Issue #91](https://github.com/blockblaz/lean-quickstart/issues/91) | Open | Docker commit hash metadata |
| [Issue #94](https://github.com/blockblaz/lean-quickstart/issues/94) | Closed | Default Docker image config |
