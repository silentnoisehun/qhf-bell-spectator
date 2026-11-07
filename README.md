# QHF Bell-Spectator: Quantum-Hydrodynamic Entrainment on Heron QPU

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![IBM Quantum](https://img.shields.io/badge/IBM%20Quantum-Heron%20r2%20(156q)-blue.svg)](https://quantum.ibm.com/)
[![ORCID](https://img.shields.io/badge/ORCID-0009--0003--3986--6039-green.svg)](https://orcid.org/0009-0003-3986-6039)

**Preregistered experimental campaign testing macroscopic quantum-hydrodynamic spectator entrainment and Bell-anchor correlation stability on a 156-qubit superconducting quantum processor (`ibm_marrakesh`, Heron r2).**

**Author:** Máté Róbert (Rongyász)  
**Affiliation:** Silent Coder Labs  
**ORCID:** [0009-0003-3986-6039](https://orcid.org/0009-0003-3986-6039)  
**Contact:** materobi86@gmail.com  

---

## 🔬 Experimental Lineage & Structure

This repository archives the complete, step-by-step preregistered experimental campaign to test whether a classical-quantum closed feedback loop (coupling a 2D Gross–Pitaevskii superfluid simulation to transmon qubits) alters the physical correlation $c = P(00) + P(11)$ of an entangled Bell anchor state.

### 1. Phase 1: Sequential Interleaved Baseline Campaign (`docs/PREREGISTRATION-c_live.md`)
* **Design:** 60 blocks (120 IBM Quantum jobs), 1024 shots per job.
* **Layout:** Qubits `[0, 1, 2]` (main 3-qubit RY circuit) and `[0, 1]` (Bell anchor). Circuits executed sequentially in each job.
* **Result (Campaign `20261107T012230Z`):**
  - Mean $c_F = 0.9305$, Mean $c_N = 0.9279$
  - Difference $d = +0.00254$, standard deviation $s = 0.0200$, $t = 0.982$, $p = 0.165$.
  - Result: $H_1$ was not supported under the preregistered threshold ($\Delta \ge 0.003, p < 0.01$). Textbook isolated QM prediction ($d = 0$) held.

### 2. Phase 2: Simultaneous 5-Qubit Spectator Campaign (`docs/PREREGISTRATION-c_live_simultaneous.md`)
* **Amendment 1 (Pre-Execution Audit):**
  1. **Drive Equivalence Across Arms:** Both F and N arms execute the circuit with identical microwave rotations extracted from the fluid. In arm F, measurements excite the fluid; in arm N, the fluid is decoupled. This isolates the feedback loop from trivial drive-induced Stark/crosstalk shifts.
  2. **Optimal Transmon Chain:** Mapped to the highest-fidelity internal line on the Heron lattice: `[51, 52, 53, 54, 55]`. Bell pair `[54, 55]` features $<0.49\%$ readout error and $0.13\%$ two-qubit gate error (baseline $c \approx 0.99$).
  3. **Simultaneous Single-Circuit Architecture:** A single 5-qubit circuit runs both the main GPE register `[51, 52, 53]` and the spectator Bell pair `[54, 55]` simultaneously, enabling real physical Hamiltonian spectator dynamics.
  4. **Sample Size:** 60 blocks (120 jobs), 2048 shots per iteration.

---

## 📁 Repository Layout

```
├── docs/
│   ├── PREREGISTRATION-c_live.md                 # Original preregistration protocol (Phase 1)
│   └── PREREGISTRATION-c_live_simultaneous.md    # Preregistration protocol & Amendment 1 (Phase 2)
├── scripts/
│   ├── c_live_interleaved.py                     # Phase 1 orchestrator (sequential circuits)
│   ├── c_live_analyze.py                         # Phase 1 preregistered statistical analyzer
│   ├── c_live_simultaneous.py                    # Phase 2 orchestrator (simultaneous 5-qubit circuit)
│   ├── c_live_simultaneous_analyze.py            # Phase 2 preregistered statistical analyzer
│   └── fluid_sim.py                              # High-performance Gross-Pitaevskii 2D simulation
└── measurement_raw/
    ├── c_live/
    │   ├── 20261107T012123Z_dry/                 # Local StatevectorSampler control dry run
    │   └── 20261107T012230Z/                     # Phase 1: Full 120-job live Heron QPU dataset & analysis.json
    └── c_live_simultaneous/
        └── 20261107T015936Z_dry/                 # Phase 2: Amendment 1 StatevectorSampler control dry run
```

---

## 🛠️ Replication & Verification

To verify the Phase 1 completed campaign analysis directly from raw quantum counts:
```bash
python scripts/c_live_analyze.py measurement_raw/c_live/20261107T012230Z
```

To run a zero-quota dry run of the simultaneous 5-qubit spectator pipeline:
```bash
python scripts/c_live_simultaneous.py --dry-run
python scripts/c_live_simultaneous_analyze.py measurement_raw/c_live_simultaneous/<dry_run_folder>
```
