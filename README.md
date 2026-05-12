# CommunityTrust Protocol — CTP v0.1

> *"Je suis parce que nous sommes"* — Philosophie Ubuntu, Afrique australe
>
> *"I am because we are"*

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20031237.svg)](https://doi.org/10.5281/zenodo.20031237)
[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
[![ORCID](https://img.shields.io/badge/ORCID-0009--0006--0220--3006-green.svg)](https://orcid.org/0009-0006-0220-3006)
[![Status](https://img.shields.io/badge/Status-v0.1%20Founding%20Document-blue.svg)]()

**Auteur / Author:** Ange AWALA  
**Organisation:** OpenScience Community  
**Contact:** [ange.awala.bj@gmail.com](mailto:ange.awala.bj@gmail.com) · [opensciencec@gmail.com](mailto:opensciencec@gmail.com)

---

## What is CTP?

Existing trust systems measure **popularity**, **wealth**, or **institutional status**.

CTP measures something different: **behavioral reliability over time**, independently of any pre-existing financial or social capital.

Inspired by African tontines — decentralized community financing systems where collective trust replaces bank guarantees — CTP formalizes a **universal reputation engine** applicable to any community-based system:

- Social networks
- Decentralized autonomous organizations (DAOs)
- Knowledge cooperatives
- Micro-finance systems
- Scientific communities

---

## The Core Idea

In a traditional tontine, a group contributes periodically to a common pool. The system works without legal contracts or bank guarantees — **solely on reputation and social pressure**.

CTP formalizes this: every member has a **TrustScore** τ ∈ [0,1] that:

- **Rises slowly** through validated contributions (η = 0.05)
- **Falls fast** after a default (κ = 0.15 >> η)
- **Depreciates** through inactivity (δ = 0.02)
- **Cannot be bought, sold, or transferred**

> The TrustScore is built only through observed behavior in time.

---

## Architecture

```
Meta-Node (global rules, immutable)
    │
    ├── Node A (tontine, neighborhood)
    │       ├── Member 1 — τ = 0.87
    │       ├── Member 2 — τ = 0.54
    │       └── Member 3 — τ = 0.12
    │
    ├── Node B (scientific community)
    │       ├── Member 4 — τ = 0.93
    │       └── Member 5 — τ = 0.71
    │
    └── Node C (DAO governance)
            └── ...
```

**Five primitive concepts — immutable:**

| Concept | Definition |
|---|---|
| **Signal** | Any raw act freely produced by a member |
| **Contribution** | A Signal collectively validated by Witnesses |
| **Witness** | A member who stakes their reputation to validate |
| **TrustScore** | Behavioral reputation score τ ∈ [0,1] |
| **Node** | A community implementing CTP |

---

## Mathematical Model

The TrustScore evolves according to **Equation (1)**:

```
τ_{i,j}(t+1) = clip(τ_{i,j}(t) + G - L - D - W, 0, 1)
```

Where:

| Force | Equation | Role |
|---|---|---|
| **G** (Gain) | η · Σ q(s_k) | Reward for validated contributions |
| **L** (Loss) | κ · 1_default | Penalty for defaults |
| **D** (Depreciation) | δ · τ · 1_inactive | Inactivity erosion |
| **W** (Witness cost) | φ · Σ τ/\|W(s)\| | Cost for poor validation |

**Contribution quality** q(s) = τ̄_W · ρ(s) — weighted by Witness reputation.

**Adaptive threshold** k_j = max(3, floor(0.20 · |M_j|)) — scales with community size.

---

## Game Theory

Three strategic behaviors are analyzed and neutralized:

| Behavior | Mechanism | Condition |
|---|---|---|
| Free rider | Depreciation δ + value of rights | f(τ) real and increasing |
| Witness collusion | Accountability φ | φ ≥ 3η |
| Strategic deserter | Migration penalty μ | μ ∈ [0.6, 0.8] |

**Result:** honest behavior strictly dominates all strategic alternatives under recommended parameters.

---

## Monte Carlo Validation

1,000 simulations × 52 periods — 12 members per Node:

| Behavior | Mean score | Status |
|---|---|---|
| Honest | 0.9231 | ✓ Dominant |
| Colluder | 0.2878 | ✓ Neutralized by φ |
| Deserter | 0.2242 | ✓ Neutralized by μ |
| Free rider | 0.1135 | ✓ Neutralized by δ |

**All 3 theoretical propositions verified at 100.00%** across 1,000 independent simulations.

---

## Repository Structure

```
ctp/
├── README.md                          ← this file
├── README_FR.md                       ← French version
│
├── paper/
│   ├── CTP_v0.1_Ange_AWALA_EN.pdf    ← English article (22 pages)
│   ├── CTP_v0.1_Ange_AWALA_FR.pdf    ← French article (23 pages)
│   ├── CTP_v0.1_Ange_AWALA_EN.tex    ← LaTeX source (EN)
│   └── CTP_v0.1_Ange_AWALA_FR.tex    ← LaTeX source (FR)
│
├── simulation/
│   ├── CTP_MonteCarlo_v0.1.py        ← Full Monte Carlo simulation
│   └── results/
│       └── CTP_simulation_v0.1.png   ← Academic-style figures
│
├── contracts/
│   ├── CTPCore_v0.1.sol              ← Solidity smart contract
│   └── test/
│       └── CTPCore.test.js           ← Hardhat test suite
│
└── docs/
    ├── whitepaper.md                 ← Non-technical overview
    ├── philosophy.md                 ← Ubuntu, tontines, vision
    └── roadmap.md                    ← v0.1 → v0.2 → production
```

---

## Quick Start

### Run the simulation

```bash
# Clone the repository
git clone https://github.com/angeawalabj/ctp.git
cd ctp

# Install dependencies
pip install numpy matplotlib scipy

# Run Monte Carlo validation (1000 simulations × 52 periods)
python simulation/CTP_MonteCarlo_v0.1.py
```

### Test the smart contract

```bash
# Install Node.js dependencies
cd contracts
npm install --save-dev hardhat @nomicfoundation/hardhat-toolbox

# Compile
npx hardhat compile

# Run tests
npx hardhat test

# Deploy on testnet (Polygon Mumbai)
PRIVATE_KEY=your_key npx hardhat run scripts/deploy.js --network mumbai
```

### Test on Remix (no installation)

1. Go to [remix.ethereum.org](https://remix.ethereum.org)
2. Create `CTPCore.sol` and paste the contract
3. Compile with Solidity 0.8.24
4. Deploy on JavaScript VM (free testnet)
5. Call functions directly in the interface

---

## Recommended Parameters

| Parameter | Role | Value | Constraint |
|---|---|---|---|
| η (eta) | Contribution gain rate | 0.05 | η ≪ κ |
| κ (kappa) | Default penalty | 0.15 | κ ≫ η |
| δ (delta) | Inactivity depreciation | 0.02 | δ > 0 |
| φ (phi) | Witness accountability | 0.15 | φ ≥ 3η |
| λ (lambda) | Base portability | 0.20 | λ ∈ [0,1] |
| μ (mu) | Migration penalty | 0.70 | μ ∈ [0.6, 0.8] |
| k_min | Minimum Witnesses | 3 | k_min ≥ 3 |
| α_k | Adaptive fraction | 0.20 | α_k ∈ (0,1) |

---

## Related Work

CTP builds on and complements:

- **EigenTrust** (Kamvar et al., 2003) — foundational P2P reputation algorithm
- **PeerTrust** (Xiong & Liu, 2004) — credibility-weighted trust
- **Trust as a Computational Concept** (Marsh, 1994) — founding thesis
- **Daoudi, Lefrançois & Zimmermann (2026)** — formal contract negotiation in data spaces. CTP provides the actor reliability mechanism their framework lacks; their ODRL ontologies provide the formal basis for CTP's sim(j,j') function.
- **Ardener (1964)** — rotating savings and credit associations
- **Ostrom (1990)** — governing the commons

---

## Roadmap

### v0.1 — Current ✓
- Complete conceptual specification
- Rigorous mathematical model
- Game theory analysis
- Monte Carlo validation (1,000 simulations)
- Solidity MVP smart contract
- Article published on Zenodo (FR + EN)

### v0.2 — Planned
- Capital of Forgiveness — adaptive gain rate η_i(t)
- Formal sim(j,j') — directional similarity from member flows
- Algorithmic complexity analysis — gas cost table
- Marsh (1994) formally positioned
- Explicit limitations section

### v0.3 — Future
- CTP-Finance module (micro-finance allocations)
- Combination with Daoudi et al. (data spaces)
- Calibration on real tontine data (West Africa)
- Multi-Node global score on-chain

---

## Ethical Principles

These are not recommendations — they are **constitutive of the protocol**.  
Any implementation that violates them is not CTP-compliant.

1. **Community autonomy** — strengthens self-governance without institutional dependency
2. **Reciprocity** — long-term parasitism is mechanically impossible
3. **Repairable short memory** — a past default does not condemn for life
4. **Rule transparency, personal confidentiality** — rules are public, scores are not (unless the Node decides otherwise)
5. **Non-commodification of score** — TrustScore is not a token, not an asset, not tradeable
6. **Data decentralization** — each Node manages its own data

---

## Citing This Work

```bibtex
@misc{awala2026ctp,
  author       = {Awala, Ange},
  title        = {{CommunityTrust Protocol (CTP v0.1): A Universal
                   Community Trust Protocol Inspired by African Tontines}},
  year         = {2026},
  doi          = {10.5281/zenodo.20031237},
  url          = {https://doi.org/10.5281/zenodo.20031237},
  publisher    = {Zenodo},
  note         = {OpenScience Community. License: CC BY-SA 4.0}
}
```

---

## Contributing

CTP is an open protocol. Contributions are welcome:

- **Test** the protocol on concrete use cases
- **Propose** formal modifications via the GovSignal mechanism
- **Contribute** to the open source implementation
- **Share** calibration data from real community contexts

Open an issue or send a pull request. For significant changes, open a discussion first.

---

## License

[Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/)

Copyright © 2026 Ange AWALA — OpenScience Community

---

<div align="center">

**OpenScience Community**

[ange.awala.bj@gmail.com](mailto:ange.awala.bj@gmail.com) · [opensciencec@gmail.com](mailto:opensciencec@gmail.com) · [ORCID](https://orcid.org/0009-0006-0220-3006) · [Zenodo DOI](https://doi.org/10.5281/zenodo.20031237)

*"I am because we are"*

</div>
