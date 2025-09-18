# PQ Interop - Meeting 9 Minutes

**Date:** September 10, 2025  
**Agenda:** https://github.com/ethereum/pm/issues/1722
**Recording:** https://youtu.be/Oa1cIj8av4M?si=OiMuGb9KJJ4_ITu3

## Agenda
- Demo leanView tool for DevNet visualization
- Update on lean SSZ implementation
- Quadrium client progress for DevNet
- Discuss DevNet zero spec status and interop preparation
- Review DevNet zero spec summary document

## Key Discussion Points
1. **leanView Tool Demo (Jun)**
   - **Overview**: Jun (Ream team) presented leanView, a full-stack web app for visualizing PQ DevNet activity, built to aid developers and researchers.
   - **Components**:
     - **Backend**: Written in Go, includes an indexer fetching data every slot (4 seconds in current setup).
     - **Frontend**: Built with React, TypeScript, and AI-generated UI; displays live node status (head slot, block number, block root) and historical data in a table.
     - **Dockerized**: Uses Docker Compose for deployment.
   - **Inspiration**: Combines forkmon-like live monitoring and blockchain explorer features (e.g., Dora, Etherscan).
   - **Functionality**:
     - Monitors node health via RPC endpoints; flags unhealthy nodes if endpoints return empty/invalid responses.
     - Displays head slot and historical block data; plans to add fork detection by comparing block headers.
   - **Demo**: Ran a Ream node with four validators using 3SF mini-consensus, feeding data to leanView.
   - **Feedback Requests**:
     - Suggestions for naming (leanView open to change).
     - Feature ideas for PQ-specific scenarios (e.g., signatures in DevNet 1, aggregation in DevNet 2).
     - Optimization for scaling to 20+ nodes (currently handles three clients; needs separation of monitoring pages).
   - **Zeam Integration (Gajinder)**: Requested adding Zeam nodes to leanView. Jun requires RPC endpoints compatible with Ream’s structure; empty JSON responses will mark nodes as unhealthy.
   - **Fork Detection**: Gajinder suggested comparing head block headers to detect forks without needing 3SF logic in leanView. Jun to share endpoint definitions for implementation.

2. **Lean SSZ Implementation (Thomas)**
   - **Purpose**: Address outdated SSZ implementations (e.g., EF Python, Nethermind) with a modern Python version using strong typing (Pydantic, Pedantic).
   - **Progress**:
     - Defined all SSZ types (e.g., Boolean, uint) with clear, maintainable code.
     - Core SSZ logic in `ssz` folder; under review by Felipe.
     - Open-sourced for community contributions and Python best practices.
   - **Hashing**: Confirmed SHA-256 for SSZ merklization in DevNet zero.
   - **Next Steps**: Post to lean consensus group for external feedback; iterate based on reviews.

3. **Quadrium Client Update (Kamil)**
   - **Status**: Open-sourced `qlean-mini` client in C++, using C++ libp2p (contrasts with Ream’s Rust libp2p).
   - **Progress**:
     - Block production implemented; nodes can produce and sync blocks.
     - Working on 3SF and state transition function (STF) integration.
     - Gossipsub topics for block announcements implemented.
     - Docker support in progress (not ready yet).
   - **Challenges**: Reuses off-chain labs’ C++ SSZ (unmaintained for over a year); may face issues during interop.
   - **Next Steps**: Test interop with other clients; finalize Docker integration.

4. **DevNet Zero Spec and Interop Preparation**
   - **Spec Status**:
     - Fork choice PR (Gajinder) under review; conceptually sound but needs syntax cleanup and minor modifications. Zeam implementation aligns with PR; targeting merge by Friday, September 19.
     - Ream (O) nearly complete with spec implementation; focusing on memory optimization and DB integration, aiming for completion next week.
   - **Genesis Generation (Gajinder, PK)**:
     - PK developed a genesis generator producing necessary files (config, ENR, secret keys); SSZ genesis file included for client verification, though not required.
     - Clients can read genesis files directly to avoid cross-client issues.
   - **Interop Strategy**:
     - Gajinder proposed a local interop test next week (Ream and Zeam) using a shell tool to spin up Dockerized nodes, bypassing kurtosis due to its complexity (spins up execution clients, unnecessary code).
     - Shell tool (inspired by Lodestar’s quick-start) to generate genesis and start network; Ream and Zeam to provide Docker images.
     - Kurtosis preparation (2-3 weeks) to continue in parallel for future DevNets.
   - **Shadow Compatibility (Kamil)**: Quadrium suggests shadow framework for network simulation (controls bandwidth, latency); not a kurtosis replacement but useful for testing. Challenges include IPv6 incompatibility and Grafana integration issues due to faster-than-real-time simulation.

5. **DevNet Zero Spec Summary Document**
   - **Purpose**: Consolidate all DevNet zero specs, PRs, and test scenarios into a single markdown (following ETH Panda Ops PM style).
   - **Current State**: Will Corcoran to lead updates; links to subspec sections, active PRs, and test scenarios needed.
   - **Action Plan**:
     - Client teams (Ream, Zeam, Quadrium) and spec writers to provide links to relevant PRs and spec sections.
     - Document to track client interoperability and test success criteria.
     - Gajinder suggested including functionality details from lean spec repo; Will to update weekly as central hub.

6. **Grafana Instance Update (Guillaume)**
   - **Status**: DevOps spun up a Grafana instance with VictoriaMetrics DB intake (compatible with Prometheus protocol).
   - **Next Steps**: Guillaume to test by September 15; report any issues.

7. **Action Items**
   - **Jun**:
     - Share leanView RPC endpoint definitions with Zeam team (via Telegram).
     - Add fork detection by comparing block headers; explore optimization for 20+ nodes.
     - Collect feature/naming feedback via chat/DM.
   - **Gajinder**:
     - Finalize fork choice PR syntax and merge by September 19.
     - Coordinate with PK on shell tool for local interop (Ream, Zeam) next week.
     - Provide DevNet zero spec/PR links for summary document.
   - **O**:
     - Complete Ream spec implementation (memory optimization, DB integration) by next week.
     - Prepare Docker images for local interop.
   - **Thomas, Felipe**:
     - Complete lean SSZ review; post to lean consensus group for feedback.
   - **Kamil**:
     - Finalize 3SF/STF integration and Docker support for Quadrium client.
     - Explore shadow framework for network simulation.
   - **Guillaume**:
     - Test Grafana instance by September 15; report issues.
   - **Will Corcoran**:
     - Collect PR/spec links from teams for DevNet zero summary document.
     - Update document weekly as central hub.
   - **PK**:
     - Confirm genesis generator readiness; provide files to teams.

8. **Open Questions**
   - How to optimize leanView for large-scale networks (20+ nodes)?
   - Specific RPC endpoints needed for leanView (beyond block, header, version, state)?
   - Feasibility of shadow vs. kurtosis for interop testing.
   - Final test scenarios for DevNet zero interop.

## Next Steps
- **Local Interop**: Ream and Zeam to test interop next week using shell tool and Docker images.
- **Spec Finalization**: Merge fork choice PR by September 19; complete Ream optimizations.
- **leanView**: Jun to share endpoint specs and iterate based on feedback.
- **Lean SSZ**: Finalize review and open for community contributions.
- **Grafana**: Guillaume to confirm instance functionality by September 15.
- **DevNet Zero Summary**: Will to consolidate spec/PR links; teams to provide inputs.
- **Next Call**: Continue DevNet zero interop planning; review DevNet one spec progress.