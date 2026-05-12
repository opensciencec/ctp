# CommunityTrust Protocol — CTP v0.1

> *"Je suis parce que nous sommes"* — Philosophie Ubuntu, Afrique australe

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20031237.svg)](https://doi.org/10.5281/zenodo.20031237)
[![License: CC BY-SA 4.0](https://img.shields.io/badge/Licence-CC%20BY--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
[![ORCID](https://img.shields.io/badge/ORCID-0009--0006--0220--3006-green.svg)](https://orcid.org/0009-0006-0220-3006)

**Auteur :** Ange AWALA  
**Organisation :** OpenScience Community  
**Contact :** [ange.awala.bj@gmail.com](mailto:ange.awala.bj@gmail.com) · [opensciencec@gmail.com](mailto:opensciencec@gmail.com)

---

## Qu'est-ce que CTP ?

Les systèmes de confiance existants mesurent la **popularité**, la **richesse**, ou le **statut institutionnel**.

CTP mesure quelque chose de différent : la **fiabilité comportementale dans le temps**, indépendamment de tout capital financier ou social préexistant.

Inspiré des tontines africaines — systèmes de financement communautaire décentralisé où la confiance collective remplace la garantie bancaire — CTP formalise un **moteur de réputation universel** applicable à tout système communautaire :

- Réseaux sociaux
- Organisations autonomes décentralisées (DAOs)
- Coopératives de connaissance
- Systèmes de micro-finance
- Communautés scientifiques

---

## L'idée centrale

Dans une tontine traditionnelle, un groupe contribue périodiquement à un pot commun. Le système fonctionne sans contrat légal ni garantie bancaire — **uniquement sur la réputation et la pression sociale**.

CTP formalise ceci : chaque membre a un **TrustScore** τ ∈ [0,1] qui :

- **Monte lentement** grâce aux contributions validées (η = 0.05)
- **Chute vite** après un défaut (κ = 0.15 >> η)
- **Se déprécie** par inactivité (δ = 0.02)
- **Ne peut pas être acheté, vendu ou transféré**

> Le TrustScore se construit uniquement par le comportement observé dans le temps.

---

## Architecture

```
Meta-Node (règles globales, immuables)
    │
    ├── Node A (tontine de quartier)
    │       ├── Membre 1 — τ = 0.87
    │       ├── Membre 2 — τ = 0.54
    │       └── Membre 3 — τ = 0.12
    │
    ├── Node B (communauté scientifique)
    │       ├── Membre 4 — τ = 0.93
    │       └── Membre 5 — τ = 0.71
    │
    └── Node C (gouvernance DAO)
            └── ...
```

**Cinq concepts primitifs — immuables :**

| Concept | Définition |
|---|---|
| **Signal** | Tout acte brut produit librement par un membre |
| **Contribution** | Un Signal collectivement validé par des Witnesses |
| **Witness** | Un membre qui engage sa réputation pour valider |
| **TrustScore** | Score de réputation comportemental τ ∈ [0,1] |
| **Node** | Une communauté qui implémente CTP |

---

## Modèle Mathématique

Le TrustScore évolue selon l'**Équation (1)** :

```
τ_{i,j}(t+1) = clip(τ_{i,j}(t) + G - L - D - W, 0, 1)
```

Où :

| Force | Équation | Rôle |
|---|---|---|
| **G** (Gain) | η · Σ q(s_k) | Récompense des contributions validées |
| **L** (Perte) | κ · 1_défaut | Pénalité de défaut |
| **D** (Dépréciation) | δ · τ · 1_inactif | Érosion par inactivité |
| **W** (Coût Witness) | φ · Σ τ/\|W(s)\| | Coût de mauvaise validation |

**Qualité de Contribution** q(s) = τ̄_W · ρ(s) — pondérée par la réputation des Witnesses.

**Seuil adaptatif** k_j = max(3, floor(0.20 · |M_j|)) — s'adapte à la taille de la communauté.

---

## Théorie des Jeux

Trois comportements stratégiques analysés et neutralisés :

| Comportement | Mécanisme | Condition |
|---|---|---|
| Passager clandestin | Dépréciation δ + valeur des droits | f(τ) réelle et croissante |
| Collusion Witnesses | Responsabilité φ | φ ≥ 3η |
| Déserteur stratégique | Pénalité de migration μ | μ ∈ [0.6, 0.8] |

**Résultat :** le comportement honnête domine strictement toutes les alternatives stratégiques sous les paramètres recommandés.

---

## Validation Monte Carlo

1 000 simulations × 52 périodes — 12 membres par Node :

| Comportement | Score moyen | Statut |
|---|---|---|
| Honnête | 0.9231 | ✓ Dominant |
| Colludeur | 0.2878 | ✓ Neutralisé par φ |
| Déserteur | 0.2242 | ✓ Neutralisé par μ |
| Passager clandestin | 0.1135 | ✓ Neutralisé par δ |

**Les 3 propositions théoriques vérifiées à 100.00%** sur 1 000 simulations indépendantes.

---

## Structure du dépôt

```
ctp/
├── README.md                          ← version anglaise
├── README_FR.md                       ← ce fichier
│
├── paper/
│   ├── CTP_v0.1_Ange_AWALA_EN.pdf    ← article anglais (22 pages)
│   ├── CTP_v0.1_Ange_AWALA_FR.pdf    ← article français (23 pages)
│   ├── CTP_v0.1_Ange_AWALA_EN.tex    ← source LaTeX (EN)
│   └── CTP_v0.1_Ange_AWALA_FR.tex    ← source LaTeX (FR)
│
├── simulation/
│   ├── CTP_MonteCarlo_v0.1.py        ← simulation Monte Carlo complète
│   └── results/
│       └── CTP_simulation_v0.1.png   ← graphiques style académique
│
├── contracts/
│   ├── CTPCore_v0.1.sol              ← smart contract Solidity
│   └── test/
│       └── CTPCore.test.js           ← suite de tests Hardhat
│
└── docs/
    ├── whitepaper.md                 ← présentation non-technique
    ├── philosophy.md                 ← Ubuntu, tontines, vision
    └── roadmap.md                    ← v0.1 → v0.2 → production
```

---

## Démarrage rapide

### Lancer la simulation

```bash
# Cloner le dépôt
git clone https://github.com/angeawalabj/ctp.git
cd ctp

# Installer les dépendances
pip install numpy matplotlib scipy

# Lancer la validation Monte Carlo (1000 simulations × 52 périodes)
python simulation/CTP_MonteCarlo_v0.1.py
```

### Tester le smart contract

```bash
# Installer les dépendances Node.js
cd contracts
npm install --save-dev hardhat @nomicfoundation/hardhat-toolbox

# Compiler
npx hardhat compile

# Lancer les tests
npx hardhat test

# Déployer sur testnet (Polygon Mumbai)
PRIVATE_KEY=ta_clé npx hardhat run scripts/deploy.js --network mumbai
```

### Tester sur Remix (sans installation)

1. Aller sur [remix.ethereum.org](https://remix.ethereum.org)
2. Créer `CTPCore.sol` et coller le contrat
3. Compiler avec Solidity 0.8.24
4. Déployer sur JavaScript VM (testnet gratuit)
5. Appeler les fonctions directement dans l'interface

---

## Paramètres recommandés

| Paramètre | Rôle | Valeur | Contrainte |
|---|---|---|---|
| η (eta) | Taux de gain | 0.05 | η ≪ κ |
| κ (kappa) | Pénalité de défaut | 0.15 | κ ≫ η |
| δ (delta) | Dépréciation | 0.02 | δ > 0 |
| φ (phi) | Responsabilité Witness | 0.15 | φ ≥ 3η |
| λ (lambda) | Portabilité de base | 0.20 | λ ∈ [0,1] |
| μ (mu) | Pénalité de migration | 0.70 | μ ∈ [0.6, 0.8] |
| k_min | Witnesses minimum | 3 | k_min ≥ 3 |
| α_k | Fraction adaptative | 0.20 | α_k ∈ (0,1) |

---

## Citation

```bibtex
@misc{awala2026ctp,
  author       = {Awala, Ange},
  title        = {{CommunityTrust Protocol (CTP v0.1) : Un protocole universel
                   de confiance communautaire inspiré des tontines africaines}},
  year         = {2026},
  doi          = {10.5281/zenodo.20031237},
  url          = {https://doi.org/10.5281/zenodo.20031237},
  publisher    = {Zenodo},
  note         = {OpenScience Community. Licence : CC BY-SA 4.0}
}
```

---

## Contribuer

CTP est un protocole ouvert. Les contributions sont les bienvenues :

- **Tester** le protocole sur des cas d'usage concrets
- **Proposer** des modifications formalisées via le mécanisme de GovSignal
- **Contribuer** à l'implémentation open source
- **Partager** des données de calibration de vraies communautés

Ouvre une issue ou envoie une pull request.

---

## Licence

[Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/)

Copyright © 2026 Ange AWALA — OpenScience Community

---

<div align="center">

**OpenScience Community**

[ange.awala.bj@gmail.com](mailto:ange.awala.bj@gmail.com) · [opensciencec@gmail.com](mailto:opensciencec@gmail.com) · [ORCID](https://orcid.org/0009-0006-0220-3006) · [DOI Zenodo](https://doi.org/10.5281/zenodo.20031237)

*"Je suis parce que nous sommes"*

</div>
