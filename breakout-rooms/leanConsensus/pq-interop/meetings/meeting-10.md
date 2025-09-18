# PQ Interop - Meeting 10 Minutes

## Meeting Overview
**Date:** September 17, 2025  
**Agenda:** https://github.com/ethereum/pm/issues/1728
**Recording:** https://youtu.be/JeeFrwmdX8I?si=UqmpqyarMh9-p3Ev

---

## Meeting Notes
- **Standardized Metrics and Dashboard**:
  - Discussion on creating a standardized dashboard for leanConsensus clients (Ream, Zeam, qlean) to ensure consistent metric tracking across DevNets.
  - Proposal to define standard metrics (e.g., state transition, attestations, fork choice) and a unified dashboard in the new `leanMetrics` repo.
  - Katya confirmed that dashboards can be built once metrics are standardized, similar to Ethereum consensus client dashboards in the Ethereum package repo.
  - Guillaume reported progress on Grafana integration: metrics intake (VictoriaMetrics) is functional, awaiting dashboard creation and team access confirmation.
- **Client Interop Readiness**:
  - **Zeam (Gajinder)**: Consumed PK’s genesis generator output; local interop testing underway. Identified housekeeping tasks (e.g., clearing justifications map post-finalization) to prevent state bloat. Ready for spec tests and interop after minor PR updates.
  - **Ream (Jun)**: Consumed PK’s genesis generator; running four-node local DevNet with finalization events. Completing core optimization tasks (memory, DB integration) within 3-4 days.
  - **qlean (Kamil)**: Finalizing 3SF and state transition function (STF) implementation; working on genesis file support and Docker integration. Aiming for interop testing by end of week.
- **Lean SSZ and Spec Updates (Tamaghna)**:
  - Converted all markdown specs (state transition, fork choice, SSZ types) to Python with unit tests in the `test` folder.
  - SSZ types and lists/vectors under review by Felipe; some design concerns with lists/vectors to be addressed.
  - Tests follow remerkleable format (SSZ read/write, hash tree roots, state transition); more complex fork choice tests included.
- **Lighthouse Interest (Manas)**: Expressed interest in developing for PQ DevNets; starting with boilerplate code but may not join DevNet zero due to other commitments.
- **DevNet Zero Spec Summary**:
  - Will to push PRs for meeting minutes (calls 7-10) and a DevNet zero spec sheet to consolidate specs, PRs, and success criteria.
  - Success criteria to be updated iteratively as interop testing progresses.
- **Broader Outcomes**:
  - Proposed a Cambridge workshop session to finalize dashboard UX and metrics.
  - Need for standardized metrics to focus on client overlap (e.g., attestations, fork choice) for interop validation.

**Action Items**:
- Katya: Define initial set of standardized metrics based on client interop readiness table; propose dashboard structure in `leanMetrics` repo.
- Guillaume: Create Grafana dashboard; confirm team access and metric intake functionality.
- Jun: Complete Ream optimization tasks (memory, DB) within 3-4 days; prepare Docker images for interop.
- Gajinder: Submit PRs for housekeeping tasks (e.g., justifications map clearing); coordinate local interop with Ream and qlean.
- Kamil: Finalize 3SF/STF and Docker support for qlean; test genesis file integration.
- Tamaghna, Felipe: Complete SSZ type review; restructure list/vector designs; share test folder format with teams.
- Will: Push PRs for meeting minutes and DevNet zero spec sheet; add Cambridge workshop session for dashboard UX.
- Manas: Share Lighthouse development plans for future DevNets.
- All Teams: Review client interop readiness metrics; provide feedback to Katya for standardization.

---

## Meeting Transcript

### Introduction and Agenda Overview
**Will:** Everyone, welcome to PQ Interop Call #10. We've got a short agenda, but we'll see where it goes. Katya, you're the sole contributor to the agenda, noting standardized dashboards for clients. You also reached out about creating a leanMetrics repo, which I set up this morning with access granted. Hopefully, these efforts align. Katya, want to kick us off?

### Standardized Metrics and Dashboard Discussion
**Katya:** Last Zeam call, we discussed creating a standard dashboard where clients expose a set of metrics for consistency and reliable performance tracking. We want to continue that discussion here with a broader audience.

**Gajinder:** We discussed defining standard metrics and a unified dashboard so clients don’t need their own. They can have custom dashboards for debugging, but a standard one ensures all clients supply the same metrics, making it easy to track node behavior. We’re throwing this out to a larger audience to standardize both metrics and the dashboard.

**Katya:** With standardized metrics, building a dashboard is straightforward. For Ethereum consensus clients, dashboards are in the Ethereum package repo under a dashboards folder. Kurtosis isn’t ready, but we can add dashboards once it is. The key is standardizing metrics, especially overlapping areas like state transition or multi-sigs. That’s what we should discuss.

**Gajinder:** A top-down UX approach with a standard dashboard enforces metric consistency. Guillaume, any update on the dashboard?

**Guillaume:** I confirmed the intake works—metrics are produced, and the VictoriaMetrics agent pushes them to the DevOps Grafana dashboard. I need to create the dashboard and ensure teams have access. Those who shared GitHub accounts should have invites. I’m working on Zeam and will add Ream and qlean. Teams, contact me to list metrics and verify intake.

**Gajinder:** For the dashboard UX, we need to decide what node behavior and DevNet success metrics to display. Once defined, teams can collect these metrics.

**Will:** This could be a great half-hour workshop in Cambridge to define top metrics, sketch the dashboard, and iterate for consensus. I’ll add it to the agenda.

### DevNet Zero Success Criteria and Spec Summary
**Jun:** The success criteria in the DevNet zero spec feel outdated. We’ll update them as we learn from interop and spec tests.

**Will:** I’m pushing a PR today with minutes for calls 7-10 and a separate DevNet zero spec sheet for contributors to comment on. I’ll share the link in the Telegram chat.

### Client Interop Readiness
**Katya:** I checked the client interop readiness table (https://github.com/leanEthereum/pm/blob/main/breakout-rooms/leanConsensus/pq-interop/pq-devnet-0.md#client-interop-readiness). Zeam has attestations; Ream focuses on networking. Ream, do you have attestation metrics implemented? These could be an initial set for standardization.

**Jun:** Yes, Ream has attestations implemented. I’ll review and share details for metric standardization.

**Gajinder:** Zeam consumed PK’s genesis generator output and started local interop. We need housekeeping tasks, like clearing the justifications map post-finalization to avoid state bloat. We’ll propose these in PRs this week. We’re ready for spec tests and interop after these updates.

**Jun:** Ream also consumed PK’s genesis generator (https://github.com/ReamLabs/local-pq-devnet). Our Docker Compose script runs four nodes with finalization events. We’re finishing memory and DB optimizations in 3-4 days, not spec-related but engineering tasks.

**Kamil:** qlean is finalizing 3SF and STF; working on genesis file support and Docker. We’ll test with other clients by week’s end.

### Execution Spec Test Integration
**Carson:** We’re welding execution specs and tests. Any pain points with execution spec tests? If documentation or functionality is unclear, share in the chat, and we’ll prioritize improvements.

**Will:** Thanks, Carson. Add issues to the Telegram chat or agenda, and I’ll ensure they’re noted.

### Lean SSZ and Spec Updates
**Tamaghna:** I converted all markdown specs—state transition, fork choice, SSZ types—to Python with unit tests in the `test` folder (https://github.com/leanEthereum/leanSpec/tree/main/src/lean_spec/subspecs/ssz). Markdowns remain in the docs folder, but new PRs should use Python to avoid errors. Each PR needs unit tests for easier review. Felipe is checking basic types for clarity. I’m happy with SSZ type design but less so with lists/vectors; Felipe is reviewing those.

**Gajinder:** When can we have the first test cases for CI integration? Even partial tests help us stay compliant.

**Tamaghna:** The test folder has basic and complex tests, like SSZ read/write, hash tree roots, and state transition, following remerkleable format. Fork choice tests check head and target blocks. I didn’t request reviews yet due to PR size, but teams should double-check the code and tests. We can restructure tests if needed; share PRs or issues on preferred formats.

### Lighthouse Interest
**Manas:** I’m from Lighthouse, expressing interest in PQ DevNets. I’ve started boilerplate code but may not make DevNet zero due to other commitments. I’ll join calls to stay updated.

**Will:** Welcome, Manas! Excited to have Lighthouse involved.

### Closing and Next Steps
**Will:** No blockers reported, which is great. I’ll push PRs for meeting minutes and the DevNet zero spec sheet. We’re set for interop prep next week. Any final items?

**Participants:** Thanks, bye!