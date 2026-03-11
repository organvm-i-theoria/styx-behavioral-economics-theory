# Game-Theoretic Accountability

The Styx platform delegates behavioral proof verification to a decentralized peer-audit network called the Fury system. This document formalizes the game-theoretic foundations that guarantee the network converges on truthful verdicts rather than degenerating into collusion, laziness, or adversarial manipulation.

## The Revelation Principle and Mechanism Design

Myerson's (2007) revelation principle establishes the theoretical possibility of truthful auditing: any outcome achievable by *any* mechanism can be achieved by an incentive-compatible, direct-revelation mechanism. The question for Styx is not whether truthful peer audit is possible in principle, but which specific mechanism achieves it in practice.

Mechanism design is "reverse game theory" -- rather than analyzing an existing game's equilibria, the designer constructs a game that produces the desired equilibrium as its solution. For the Fury network, the desired equilibrium is one where every auditor reports their honest assessment of proof quality, and dishonest auditors are systematically identified and removed.

Nisan and Ronen (2001) add a computational constraint: polynomial-time mechanisms may sacrifice incentive compatibility, and vice versa. The Fury Router's BullMQ-based proof assignment is a practical algorithmic mechanism that must be both computationally efficient (real-time routing) and incentive-compatible. The current implementation uses random assignment plus weighted consensus -- polynomial time and approximately incentive-compatible.

## Theorem T4: Honest Auditor Dominance

The central game-theoretic result is Theorem T4, which proves that truth-telling is the dominant strategy for Fury auditors under the platform's penalty structure.

### Setup

Model each audit as a single-shot game. Fury auditor v observes proof p and submits verdict r_v in {PASS, FAIL}. The true state s in {PASS, FAIL} is determined by consensus or honeypot ground truth.

### The Fury Accuracy Function

The accuracy function FA: F -> [0, 1] for auditor v with history (a_v, a_bar_v, n_v) is:

    FA(v) = clamp_01( (a_v - omega * a_bar_v) / n_v )    when n_v > 0
    FA(v) = 1.0                                            when n_v = 0

where a_v counts correct audits, a_bar_v counts false accusations (incorrect FAIL verdicts on honest proofs), omega = 3 is the false accusation penalty weight, and n_v is total audit count. The demotion rule fires when FA(v) < 0.8 and n_v >= 10 (burn-in).

### The Dominance Argument

For a single audit, let q = P(s = FAIL | evidence) be the auditor's private posterior belief.

**Expected FA contribution of a FAIL verdict:**
    delta_FA_FAIL = (4q - 3) / n

**Expected FA contribution of a PASS verdict:**
    delta_FA_PASS = (1 - q) / n

FAIL is better than PASS only when q > 0.8. This means the 3x penalty weight creates a *conservative* mechanism: auditors should only report FAIL when they are highly confident (80%+ posterior belief). The system biases toward PASS verdicts, protecting oath-takers from frivolous rejections.

For an honest auditor with error rate epsilon <= 5%, the expected accuracy is FA = 1 - 4*epsilon >= 0.80. They maintain participation indefinitely. A dishonest auditor with false accusation rate r > 5% gets FA = 1 - 4r < 0.80 and is demoted after the 10-audit burn-in. Truth-telling is therefore the weakly dominant strategy for any auditor seeking to maintain participation.

### Comparison with Related Mechanisms

| Mechanism | IC Property | Styx Difference |
|-----------|-------------|-----------------|
| Bayesian Truth Serum (Prelec, 2004) | IC under common prior | Styx uses penalty weighting, no common prior required |
| Peer Prediction (Witkowski & Parkes, 2012) | Strict IC without common prior | Styx approximates with consensus + accuracy weighting |
| Kleros | IC under Schelling focal points | Similar: honeypots test focal convergence |
| TrueBit | IC under computational verification | Styx honeypots serve analogous "forced error" function |

## Peer Prediction Without Ground Truth

Prelec's (2004) Bayesian Truth Serum provides the theoretical gold standard for incentivizing truthful reporting *without* objective ground truth. The mechanism scores answers based on how "surprisingly common" they are relative to prior predictions. BTS achieves incentive compatibility under a common prior assumption.

The challenge for Styx is scale: BTS requires large populations, while Fury auditor panels are small (3-7 per proof). The platform therefore uses a modified approach -- consensus-based scoring with accuracy weighting -- that approximates BTS properties at small panel sizes. Full BTS implementation remains a candidate for high-stakes verdict categories.

Witkowski and Parkes (2012) extended peer prediction to heterogeneous belief settings, achieving strict incentive compatibility without assuming common priors. This is directly relevant because Fury auditors may hold genuinely different beliefs about proof quality, especially for subjective oath categories (creative goals, environmental commitments). The "mirror" scoring rule that rewards correlation between reports provides a theoretical blueprint for future Fury scoring refinements.

## Reputation Systems and the Integrity Score

Resnick et al. (2000) identified three properties of effective reputation systems: long-lived entities, capture of feedback, and visible history. The Styx Integrity Score satisfies all three: persistent user accounts, proof/audit feedback accumulation, and a visible numeric score.

Critically, reputation must be *costly to build and easy to lose*. The Integrity Score is asymmetric by design: +5 points per successful completion, -15 per detected fraud, -20 per strike. This 3:1 to 4:1 asymmetry mirrors loss aversion at the reputation level -- building reputation requires sustained honest behavior, while a single dishonest act produces catastrophic reputational damage.

Resnick et al. (2006) demonstrated in controlled eBay field experiments that high-reputation sellers earn 8.1% price premiums -- reputation has measurable economic value. In Styx, higher Integrity Scores unlock higher staking tiers, creating tangible economic value for maintained reputation. New users face a trust deficit partially addressed by the onboarding bonus.

## Anti-Collusion Mechanisms

Fury verdict collusion is the central adversarial threat. Three theoretical frameworks inform the defense:

**Schelling focal points** (1960): Honest auditors converge on verdicts through focal points -- clear proof criteria and explicit rubrics create "salient" correct answers that honest participants naturally coordinate around. Honeypot proofs test whether auditors coordinate on the correct focal point (the known-fail proof should receive FAIL verdicts).

**Anti-plutocratic weighting** (Buterin, Hitzig & Weyl, 2018): The quadratic funding principle -- many small contributions outweigh a few large ones -- maps to Fury influence. Many low-reputation Furies agreeing should outweigh one high-reputation dissenter. This prevents whale capture of the audit network.

**Anti-collusion infrastructure** (Buterin, 2021): MACI (Minimal Anti-Collusion Infrastructure) uses encrypted votes to prevent vote buying and coordination. Future Fury implementations could adopt encrypted verdicts that are only revealed after consensus, preventing Fury-to-Fury coordination on dishonest outcomes.

## Honeypot Reinforcement

The honeypot system injects known-fail proofs into the audit stream. Correct identification yields +5 integrity bonus; missed honeypots yield -5 penalty. This creates a secondary truth-telling incentive that closes the gap for "lazy but honest" auditors who might default to PASS on every proof without examination.

Honeypots are theoretically analogous to TrueBit's "forced errors" -- deliberate test cases that ensure auditors are actually performing verification rather than rubber-stamping. The injection rate and detection threshold are tunable parameters that balance security against auditor fatigue.

## References

- Buterin, V. (2021). Moving beyond coin voting governance. *vitalik.ca*.
- Buterin, V., Hitzig, Z., & Weyl, E. G. (2018). Liberal radicalism: A flexible design for philanthropic matching funds. *SSRN Working Paper*.
- Myerson, R. B. (2007). Perspectives on mechanism design in economic theory (Nobel Prize lecture). *American Economic Review*, 98(3), 586-603.
- Nisan, N., & Ronen, A. (2001). Algorithmic mechanism design. *Games and Economic Behavior*, 35(1-2), 166-196.
- Posner, E. A., & Weyl, E. G. (2018). *Radical Markets*. Princeton University Press.
- Prelec, D. (2004). A Bayesian truth serum for subjective data. *Science*, 306(5695), 462-466.
- Resnick, P., Kuwabara, K., Zeckhauser, R., & Friedman, E. (2000). Reputation systems. *Communications of the ACM*, 43(12), 45-48.
- Resnick, P., Zeckhauser, R., Swanson, J., & Lockwood, K. (2006). The value of reputation on eBay. *Experimental Economics*, 9(2), 79-101.
- Schelling, T. C. (1960). *The Strategy of Conflict*. Harvard University Press.
- Witkowski, J., & Parkes, D. C. (2012). Peer prediction without a common prior. *Proceedings of the 13th ACM Conference on Electronic Commerce*, 964-981.
