# PQ Devnet-0 Specs


### Devnet Change Log
- No changes, initial devnet 


### Active Discussions

- **Sig Parameters:**
    - Hash chains: 64
    - Chain length: 8
    - Max lifetime: 2^32
    - Activation time: 2^18
- **Slot Duration:** 4 seconds
- **Networking Transport:** QUIC
- **Gossip:** Gossipsub v1.2
- **Validator Count:** Start with 512 keys (meeting consensus) and scale up to 1024 validators
- **Consensus Mechanism:** Minimal fork choice rule (to be spec'ed)
- **Infrastructure Setup:** Should we engage ethPandaOps to:
    - Set up and host a shared Grafana and Prometheus instance for this effort?
    - Fork Kurtosis to support leanEthereum devnets (and patch in Genesis Generator)?
    - Use ethPandaOps Docker image builder and tracker to build/publish CL images?

## Interop Objectives, by Devnet
| Devnet-# | Objective(s) | Aspirational Ship Date |
| --------- | ------------ | ----------------------- |
| **`pq-devnet-0`** |  - Signature chaining<br>-Minimal fork choice | End of August |
| **`pq-devnet-1`** | - PQ-Signatures | Mid-October |
| **`pq-devnet-2`** | - Single Subnet Aggregation | TBD |


## Sub-Spec List for PQ Devnet-0
>The list below links to reference papers, specs, and implementations. Versions to be finalized by August 31, 2025.


### Test Releases

**Consensus Specs:** link to consensus spec release (Felipe from STEEL will set this up the week of July 28)
**Execution Spec Tests:** Not applicable

### Spec Versions Required & Open PRs

- List of links to spec PR's **Merged** :heavy_check_mark: 
- List of links to spec PR's **Open** :exclamation:


### Client Support Status
>develop list of testing scenarios (link here)

| Testing Scenario | Ream | Zeam | [TBD: Third Client?] |
|------------------|------|------|----------------------|
| 1a | :question: | :question: | :question: |
| 2a | :question: | :question: | :question: |
| 3a | :question: | :question: | :question: |
| 4a | :question: | :question: | :question: |
| 5a | :question: | :question: | :question: |
| 6a | :question: | :question: | :question: |

## Kurtosis Interop Config (Pre-Devnet Testing)

>yaml below is AI slop

```yaml
participants_matrix:
  cl:
    - cl_type: ream
      cl_image: reamlabs/ream:pq-devnet-0
    - cl_type: zeam
      cl_image: zeam/zeam:pq-devnet-0
    - cl_type: [TBD - third client, e.g., lighthouse-pq]
      cl_image: [TBD]:pq-devnet-0

network_params:
  max_validators: 1000  # Initial keys, scale to 10k+
  slot_time_seconds: [TBD - 4 or 12]

additional_services:
  - metrics_dashboard  # Shared Prometheus/Grafana
  - genesis_generator

metrics_params:
  image: [TBD - ethpandaops/metrics:master]
  dashboards:
    - scenario: pq_signing
      config:
        throughput: 100  # Signatures per slot
    - scenario: propagation
      config:
        throughput: 50  # Blocks/attestations
```

### Testing:

>testing ideas below is AI slop

#### PQ Signature Viability
  * Key generation and signing/verification timings - kurtosis - generate keys for 1,000 validators and measure sub-second targets.
  * Adversarial testing: Inject bad signatures via custom actors; verify rejection.
  * EIP-7870 compliance: Monitor CPU/memory/bandwidth during runs.

#### Multi-Client Interop
  * Block-by-root: Use for recovery to catch-up to chain head when nodes break; include chianing to pull parent chains for latest gossip blocks.
  * Attestation correctness: Assertoor-like test for ≥98% (Zeam) or local tally (Ream).

#### Resource Requirements
  * Run with 1,000 validators (devnet-0); scale by 10x validators for each iteration to 10,000; measure EIP-7870 limits.
  * Hardware metrics: CPU load, memory, throughput every second.
  * Bonus: Inject adversaries (signature collisions, censoring); monitor resilience.

#### Success Criteria Validation
  * Network runs for at least 300 slots * num_clients without crashing for local testing; for cluster devnet, aim for longer durations (e.g., 10 days), subject to test infrastructure capabilities (Extended runs will be beneficial for incorporating features like 3SF in subsequent devnet iterations).
  * Full block pipeline <6s (Ream) or on-time with 4s slots (Zeam).
  * `num_attestations_received / num_validators  >= 0.098` for every slot (i.e. 98% attestation arrival)
  * `num_valid_signatures == num_attestations_received` for every slot (i.e. 100% attestation correctness)

## Client Interop Readiness

>develop list of testing scenarios (link here)

| Scenario | Ream | Zeam | TBD: Third Client |
| -------- | ---- | ---- | ------------------- |
| 1a (Basic Signing) | :question: | :question: | :question: |
| 1b (Attestation Prop) | :question: | :question: | :question: |
| 2a (Sync from Finalized) | :question: | :question: | :question: |
| 2b (Block-by-Root) | :question: | :question: | :question: |
| 3a (Justification) | :question: | :question: | :question: |
| 3b (Adversarial Sig) | :question: | :question: | :question: |
| 4 (Scale to 10k Vals) | :question: | :question: | :question: |

## Reference Spec for Previous Devnet: 
None. This is the initial devnet

### Open Future Questions:
  - Question 1
  - Question 2
  - Question 3
