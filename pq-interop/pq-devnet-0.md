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
- **Networking Transport:** QUIC (Ream) vs. TCP first then QUIC (discussion in meeting)? 
- **Gossip:** Gossipsub v1.2
- **Validator Count:** Start with 1,000 keys (meeting consensus) and scale? 
- **Consensus Mechanism:** Include minimal 3SF or no global consensus? 
- **Infrastructure Setup:** Who handles (e.g., Grafana, Prometheus, Kurtosis-like for interop, Genesis Generator tool )? 


## Sub-Spec List for PQ Devnet-0
>The list below links to reference papers, specs, and implementations. Versions to be finalized by August 31, 2025.


### Test Releases

**Consensus Specs:** link to consensus spec release
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
  * Sync testing: Start nodes at different points; use block-by-root to catch up.
  * Attestation correctness: Assertoor-like test for ≥98% (Zeam) or local tally (Ream).
  * Fork choice: Test with mini 3SF or no consensus; verify chain views align.
  * Networking: Gossip propagation delays <1s; QUIC/TCP comparison.

#### Resource Requirements
  * Run with 1,000 validators; scale to 10,000; measure EIP-7870 limits.
  * Hardware metrics: CPU load, memory, throughput every second.
  * Bonus: Inject adversaries (signature collisions, censoring); monitor resilience.

#### Success Criteria Validation
  * Network runs for 300+ slots * num_clients without crashing.
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
