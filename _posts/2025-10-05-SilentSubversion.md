---
title: "Silent Subversion Completed!"
date: 2025-10-05
permalink: /posts/2025/10/silent-subversion/
tags:
  - Research
  - Security
  - Satellites
---

The second major milestone in my small-satellite cybersecurity work is complete — **Silent Subversion**, now submitted to the **2026 IEEE Aerospace Conference**.  
This project builds directly on *SpyChain* but focuses on an even subtler problem: when a satellite’s own sensors can no longer be trusted.

### Background
Spacecraft depend entirely on remote telemetry to understand their state.  
If that telemetry is falsified, operators can be misled into making mission-ending decisions.  
While past research has explored ground-based spoofing (e.g., falsified uplinks or radio jamming), *Silent Subversion* exposes a deeper, internal threat — **onboard spoofing originating from compromised supply-chain components**.

### Implementation
The paper presents the first end-to-end simulation of an internal sensor-spoofing implant within NASA’s **NOS3** satellite environment.  
A malicious cFS component, **SOLO**, masquerades as a legitimate **star tracker**, publishing schema-valid telemetry packets that mimic real sensor cadence and formatting.  
Using standard cFS message identifiers and bus APIs, SOLO passes integration, survives pre-launch testing, and replaces genuine telemetry during flight simulation.

Two deception modes were demonstrated:
* **Bias Mode** – Injects small, drift-like perturbations that gradually bias estimation without triggering alarms.  
* **Replacement Mode** – Disables the genuine sensor and transmits falsified data indistinguishable from the real stream.

Both modes were accepted as legitimate by **COSMOS**, confirming that spoofed telemetry can deceive ground operators and onboard estimators alike.

### Findings
Silent Subversion highlights three systemic weaknesses:
1. **Implicit trust in telemetry** — packets are accepted if structurally valid.  
2. **Lack of runtime monitoring** — no verification of message provenance.  
3. **Opaque supply chains** — vendor modules integrate as black boxes with full privileges.

These weaknesses allow internal spoofing to compromise both **integrity** and **availability**, echoing industrial control-system attacks like *Stuxnet* but transposed to orbital platforms.

### Countermeasures
The study maps realistic mitigations to **MITRE SPARTA EX-0014.03 (Sensor Data Spoofing)**:
- **Authenticated telemetry** and **lightweight bus encryption** to verify data origin.  
- **On-board anomaly detection** to flag suspicious message rates.  
- **Cross-sensor verification** before activating safe-mode responses.  
- **Model-based validation** comparing telemetry to physical consistency checks.

### Reflection
Where *SpyChain* demonstrated covert exfiltration, *Silent Subversion* focuses on **deceptive integrity loss**—showing how trusted components can falsify reality itself.  
Together, these works reveal that modern small satellites face not only denial and disruption, but silent manipulation from within their own supply chains.  
Ongoing research will extend these results toward hardware-in-the-loop verification and automated countermeasure testing inside NOS3.

---

*Up for submission!*
