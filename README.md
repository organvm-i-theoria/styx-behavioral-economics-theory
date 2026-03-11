# styx-behavioral-economics-theory

Theoretical foundation for the Styx behavioral accountability platform. This is the first ORGANVM repo to traverse the full organ chain: **ORGAN-I (Theoria) -> ORGAN-II (Poiesis) -> ORGAN-III (Ergon)**.

## What This Is

This repo extracts, formalizes, and maintains the behavioral economics theory that grounds the [Styx](https://github.com/labores-profani-crux/peer-audited--behavioral-blockchain) peer-audited behavioral blockchain. Where the ORGAN-III product implements code, this repo holds the pure theory: the models, proofs, and formal specifications that justify why the system works.

## Key Theoretical Frameworks

**Behavioral Economics Foundations** -- Loss aversion (Kahneman & Tversky, 1979), prospect theory, mental accounting (Thaler, 1999), and present-biased preferences (Laibson, 1997). The calibration constant lambda = 1.955 is the single most important number in the system: it quantifies how much more painful a lost stake feels compared to an equivalent reward.

**Game-Theoretic Accountability** -- Mechanism design for the Fury peer-audit network. The central result (Theorem T4) proves that honest auditing is the dominant strategy under a 3x false-accusation penalty weight. Draws on Myerson's revelation principle, Prelec's Bayesian Truth Serum, and Schelling focal point theory to formalize why peer consensus converges on truth.

**Spoof Resistance Models** -- Formal treatment of the adversarial model: how to ensure behavioral proofs cannot be fabricated. Covers hardware immutability constraints (wearable sensor metadata filtering), multi-sensor correlation requirements, and the cybernetic feedback model (HVCS) that treats the platform as a negative feedback control surface.

## Organ Chain

```
ORGAN-I (this repo)          ORGAN-II                    ORGAN-III
behavioral-economics    -->  styx-behavioral-art    -->  peer-audited--behavioral-blockchain
theory & proofs              artistic visualization       product implementation
```

- **I -> II**: Theory informs how behavioral dynamics are visualized -- loss aversion curves, audit network topology, feedback loop animations.
- **I -> III**: Theory directly grounds product decisions -- staking tiers, penalty weights, demotion thresholds, Aegis safety caps.

## Docs

| Document | Scope |
|----------|-------|
| [behavioral-economics-foundations.md](docs/behavioral-economics-foundations.md) | Loss aversion, prospect theory, commitment devices, present bias |
| [game-theoretic-accountability.md](docs/game-theoretic-accountability.md) | Fury audit mechanism design, dominant strategy proofs, reputation systems |
| [spoof-resistance-models.md](docs/spoof-resistance-models.md) | Adversarial models, hardware immutability, multi-sensor correlation, HVCS |

## References

Primary sources synthesized here include Kahneman & Tversky (1979, 1992), Thaler (1999), Laibson (1997), Benartzi & Thaler (2004), Bryan, Karlan & Nelson (2010), Myerson (2007), Prelec (2004), Witkowski & Parkes (2012), Gneezy & Rustichini (2000), and Resnick et al. (2000, 2006). Full citations are in each document.
