# Spoof Resistance Models

A behavioral accountability platform is only as credible as its proof verification. If users can fabricate evidence of compliance -- spoofing a workout, forging a financial record, submitting a recycled video -- the entire incentive structure collapses. This document formalizes the adversarial model and the multi-layered defenses that ensure behavioral proofs reflect genuine action.

## The Adversarial Model

The fundamental assumption is that the user's mobile device is a compromised environment. Any data manually entered or locally processed is potentially spoofed. The system must therefore abstract trust away from the human layer and place the burden of proof on three independent verification channels:

1. **Hardware-attested sensor data** -- biometric and activity data from wearable devices, filtered to exclude manual entries
2. **Peer audit consensus** -- human verification through the Fury network, incentivized by game-theoretic mechanisms (see game-theoretic-accountability.md)
3. **Cryptographic media verification** -- perceptual hashing and metadata analysis of submitted photo/video proofs

A successful spoof must simultaneously defeat all three channels. The security model is defense-in-depth: each layer is individually imperfect, but their conjunction raises the cost of deception beyond the economic value of any recoverable stake.

## Hardware Immutability Constraints

### The Metadata Filtering Principle

Wearable health ecosystems (Apple HealthKit, Android Health Connect, Fitbit, Whoop) aggregate data from both automated hardware sensors and manual user inputs. The critical architectural requirement is to isolate hardware-generated data by filtering on source metadata.

**Apple HealthKit**: The `HKMetadataKeyWasUserEntered` flag distinguishes manual entries from hardware-sourced samples. A naive boolean filter (`metadata.key == NO`) fails because legitimate hardware data often has `nil` for this key rather than `FALSE`. The correct approach uses a compound predicate that accepts any sample *not explicitly flagged* as user-entered. This is a subtle but critical distinction -- the wrong predicate silently excludes valid hardware data.

**Android Health Connect**: The `recordingMethod` field in the metadata dictionary flags manual inputs. The Feature Availability API (`getFeatureStatus()`) further validates device capability, preventing legacy devices with unpatchable vulnerabilities from injecting unverified data into the system.

**Fitbit**: Two distinct API endpoints exist -- `/activity/` (aggregated, includes manual entries) and `/activity/tracker/` (hardware-only). The system must exclusively use the tracker endpoint for verification, treating the standard endpoint as untrusted.

### Hardware Trust Boundaries

The trust model assigns confidence levels to data sources:

| Source | Trust Level | Spoofing Difficulty |
|--------|------------|-------------------|
| Apple Watch (hardware-attested) | High | Requires physical device manipulation |
| Android wearable (Health Connect, hardware flag) | High | Requires OS-level exploit |
| Fitbit tracker endpoint | Medium-High | Requires API interception |
| Manual health app entry | Zero | Trivial |
| Third-party app sync (no metadata) | Low | Moderate (app-level spoofing) |

Only High and Medium-High trust sources are accepted for stake-bearing proof verification. Lower-trust sources may supplement but never substitute for hardware-attested data.

## Multi-Sensor Correlation

Single-sensor verification is vulnerable to targeted spoofing. Multi-sensor correlation raises the attack surface by requiring consistency across independent data streams. A claimed 10-mile run must correlate across:

- **GPS trace** (continuous path with plausible velocity profile)
- **Heart rate** (elevated and variable, consistent with claimed exercise intensity)
- **Step count** (cadence consistent with running gait)
- **Accelerometer pattern** (impact signatures distinguishable from driving or cycling)

The correlation engine computes a consistency score across available sensors. Inconsistencies -- such as elevated step count without corresponding heart rate elevation, or GPS movement without accelerometer impact patterns -- trigger fraud flags that escalate the proof to manual Fury review.

### The Sensor Corruption Problem

The behavioral physics manifesto identifies "sensor corruption" as the fundamental spoofing risk: if any single sensor can be compromised, the entire verification chain is only as strong as the weakest correlated signal. The architectural response is multi-sensor correlation with minimum sensor count thresholds. A proof backed by only one sensor type receives a lower confidence score than one backed by three or more correlated sensors.

This creates a natural tension with accessibility. Not all users own wearable devices with full sensor suites. The tier system accommodates this: lower-stakes commitments accept fewer verification channels, while high-stakes vaults require comprehensive multi-sensor coverage.

## The Cybernetic Feedback Model (HVCS)

At the systems level, Styx operates as a Human Vice Control System (HVCS) -- a negative feedback control surface that restores "consequence density" to behavioral intentions. In cybernetic terms:

- **Setpoint**: The user's declared behavioral goal (the oath)
- **Sensor**: The multi-channel verification system (hardware + peer + media)
- **Comparator**: The proof evaluation engine (does submitted evidence meet the oath criteria?)
- **Actuator**: The financial consequence (stake forfeiture on failure, return on success)
- **Feedback delay (tau)**: The time between behavior and consequence

Classical control theory dictates that feedback effectiveness degrades as delay tau increases. The system minimizes tau through real-time vault deduction visualization -- users see their stake status update immediately upon proof submission and verification, rather than waiting for end-of-contract settlement. This exploits myopic loss aversion (Benartzi & Thaler, 1995): frequent, immediate feedback amplifies the psychological force of the financial consequence.

The Fury network functions as the *systemic gain* on the negative feedback path. Higher Fury participation and accuracy increase the system's gain -- the ratio between behavioral deviation and corrective consequence. Low gain (few auditors, slow verification) allows spoofing to persist undetected. High gain (dense audit coverage, rapid consensus) ensures deviations are caught and penalized quickly.

### Equilibrium and Stability

A well-tuned HVCS converges to a behavioral equilibrium where the user's actual behavior matches their declared intentions. The EquilibriumScore metric tracks this convergence over time. Instability manifests as oscillation -- the user alternates between compliance and violation -- which the Aegis system detects via velocity caps and escalation pattern analysis.

The system must avoid over-correction. Excessive penalty severity or verification intrusiveness can produce learned helplessness (the user abandons the commitment entirely) rather than behavioral convergence. The Aegis safety constraints -- maximum stake caps, grace days, cool-off periods -- function as gain limiters that prevent the control loop from driving users into failure spirals.

## Perceptual Media Verification

For proof categories that require photo or video evidence (creative work, environmental actions, physical activities without wearable coverage), the system employs perceptual hashing to detect recycled or manipulated media.

Perceptual hashes (pHash, dHash) generate compact fingerprints that are robust to minor transformations (cropping, compression, color adjustment) while remaining sensitive to semantic content changes. A submitted proof is compared against the user's historical proof submissions; matches above a similarity threshold flag the submission as recycled.

EXIF metadata analysis provides a secondary verification channel: GPS coordinates, timestamps, and device identifiers in image metadata must be consistent with the claimed proof context. Stripped or inconsistent metadata triggers escalation to manual review.

## Economic Analysis of Spoofing

The defense model is ultimately economic. A rational attacker will spoof only if the expected value of spoofing exceeds its cost:

    E[spoof] = P(undetected) * stake_recovered - P(detected) * penalty - cost_of_spoofing

The system drives E[spoof] negative through three levers:

1. **Reduce P(undetected)**: Multi-sensor correlation, Fury audit, honeypot injection
2. **Increase penalty**: 3x false accusation weight, integrity score destruction, account suspension
3. **Increase cost_of_spoofing**: Hardware attestation requirements, perceptual hashing, EXIF validation

When E[spoof] < 0 for all achievable spoofing strategies, honest compliance becomes the dominant strategy -- not because users are virtuous, but because cheating is economically irrational. This is the spoof resistance guarantee: the system does not rely on trust, but on incentive alignment.

## References

- Benartzi, S., & Thaler, R. H. (1995). Myopic loss aversion and the equity premium puzzle. *Quarterly Journal of Economics*, 110(1), 73-92.
- Resnick, P., Kuwabara, K., Zeckhauser, R., & Friedman, E. (2000). Reputation systems. *Communications of the ACM*, 43(12), 45-48.
- Wiener, N. (1948). *Cybernetics: Or Control and Communication in the Animal and the Machine*. MIT Press.
