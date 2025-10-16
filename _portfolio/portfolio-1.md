---
title: "SpyChain implementation in NOS3"
excerpt: "SpyChain demonstrates stealthy, multi-component supply-chain malware for CubeSats simulated in NASA's NOS3. 1<br/><img src='/images/SpyChain1.png'>"
collection: portfolio
---

**SpyChain** studies how malicious functionality embedded in third-party (COTS) components can persist, coordinate, and exfiltrate telemetry from small satellites. Implemented and evaluated in NASA’s NOS3 simulation environment, the project demonstrates five escalating attack scenarios — from a single time-triggered payload to coordinated, FIFO-based multi-component malware that evades ground telemetry monitoring.

---

## Abstract
Small satellites increasingly rely on COTS modules that enjoy privileged access but often lack assurance. SpyChain provides the first end-to-end simulation of colluding supply-chain threats for CubeSats, showing covert coordination (software bus + FIFO), telemetry exfiltration via the radio, and plausible DoS/fault injection — all while remaining stealthy to standard ground monitoring. We map the tactics to SPARTA, propose lightweight onboard defenses, and engaged NASA NOS3 maintainers on mitigation strategies.

---

## Key results & contributions

- **Five simulated attack scenarios** (solo static, solo dynamic, colluding via software bus, colluding dynamic, colluding using FIFO file) demonstrating escalating stealth and coordination.
- **Novel multi-component execution technique** (component collusion) — added to the SPARTA matrix after disclosure.
- **Realistic exfiltration** using the NOS3 Generic Radio path (UDP), making telemetry appear indistinguishable from legitimate downlinks.
- **Demonstrations of persistence & DoS**: null-pointer crashes and message flooding indistinguishable from application bugs.
- **Actionable mitigations**: runtime syscall filtering (e.g., seccomp), software-bus authentication & access control, and lightweight runtime monitoring tailored for cFS/NOS3 constraints.
- **NASA engagement**: NOS3 team acknowledged findings and expressed interest in follow-up testbed enhancements.

---

## Demo (what we built)
- Full NOS3 simulation with cloned cFS apps:
  - **Trigger agent** (sensor/time triggers; writes commands)
  - **Attack agent** (mkfifo-based coordination; reuses radio socket for UDP exfil)
- Timeline: supply-chain compromise → dormancy → GNSS/time trigger → FIFO coordination → exfiltration / DoS / deception.
- Realistic telemetry packets (CCSDS/XTCE) forwarded to COSMOS ground station in the sim.

---

## Why this matters
- Third-party hardware often receives far less assurance than core flight software but retains equivalent runtime privileges.
- SpyChain shows how that structural asymmetry can be weaponized for long-lived reconnaissance or mission disruption — a pressing concern as small-satellite fleets scale.

---

## Implementation notes
- Built on **NOS3** + **cFS** apps; trigger/attack agents follow standard cFS lifecycle (`AppInit()`, `CFE_ES_RunLoop()`).
- FIFO coordination chosen because `mkfifo()` provides process synchronization and reduces forensic traces.
- Exfiltration path: attack agent opens the radio app’s UDP socket and issues `sendto()` with CCSDS payloads; NOS3 radio hardware model forwards to ground endpoint.
- Countermeasure prototypes: seccomp sandbox rules, message-ID manifest checks for software-bus subscriptions, and lightweight syscall/event telemetry.

---