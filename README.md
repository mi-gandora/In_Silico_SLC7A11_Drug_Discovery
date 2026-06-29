## 🗺️ Table of Contents
* [🔬 1. Structure-Based Pharmacophore Screen](#1-structure-based-pharmacophore-screen)
* [🎛️ 2. Receptor Grid Generation](#2-receptor-grid-generation)
* [🌊 3. Hierarchical Virtual Screening Cascade](#3-hierarchical-virtual-screening-cascade)
* [🔋 4. Binding Free Energy Estimation (MM-GBSA)](#4-binding-free-energy-estimation-mm-gbsa)
* [🧬 5. Physiochemical & Drug-Likeness Profiling](#5-physiochemical--drug-likeness-profiling)
* [💊 6. In Silico ADMET Functional Screening](#6-in-silico-admet-functional-screening)
* [🚀 Project Roadmap & Progress](#-project-roadmap--current-progress)

---

## 🔬 1. Structure-Based Pharmacophore Screen
Using a structure-based **AHN hypothesis** (Hydrophobic `H3`, Hydrogen Bond Acceptor `A4`, and Negative Ionic `N15`) generated from the receptor-ligand complex, a database of 2,665 drug-like compounds was subjected to a spatial feature screen.

* **Screening Criteria:** Compounds were required to match $\ge 66.7\%$ of the core features (minimum 2 out of 3) while avoiding the receptor's steric exclusion volumes.
* **Result:** **2,419 compounds** successfully cleared the filter and were isolated for molecular docking.

![Structure-Based AHN Pharmacophore Hypothesis](Images/Pharmacophore%20picture.jpg)

---

## 🎛️ 2. Receptor Grid Generation
The structural boundaries of the SLC7A11 binding cavity were explicitly mapped to constrain targeted ligand placement during docking simulation runs.

* **Active Site Coordinate Mapping:** Centered on the native workspace ligand pocket to lock the exact binding cavity coordinates:
  $$\mathbf{X: 127.06, \quad Y: 125.07, \quad Z: 122.21}$$
* **Box Dimensions:** Configured to dynamically accommodate screening ligands matching or similar in size to the native reference ligand.

<details>
<summary>⚙️ View Grid Constraints & Scaling Parameters</summary>

* **Active Site Definition:** Automated workspace ligand centroid exclusion.
* **Van der Waals Radius Scaling:** Nonpolar receptor atoms were scaled with a factor of `1.0` and a partial charge cutoff of `0.25` to maintain standard structural boundaries.
</details>

---

## 🌊 3. Hierarchical Virtual Screening Cascade
To evaluate binding behaviors within the mapped pocket, the 2,419 pharmacophore hits were subjected to a multi-tiered enrichment cascade in **Schrödinger Glide**. Five standard drugs (Erastin, Sorafenib, Imidazole Ketone Erastin, HG-106, Sulfasalazine) and the co-crystallized ligand (**PX-8**) were prepped via LigPrep and run simultaneously as benchmark controls.

* **Phase A: High-Throughput Virtual Screening (HTVS):** Executed on the complete library pool (2,425 total entries) to quickly clear non-binders.
* **Phase B: Standard Precision (SP) & Extra Precision (XP):** The top-performing **100 compounds** from the HTVS run (plus all 6 reference controls) were advanced to back-to-back SP and XP refinement phases to model stringent electrostatic interactions and active-site water elimination.

<details>
<summary>⚙️ View Advanced Glide Docking Engine Parameters</summary>

* **Ligand Sampling Mode:** `Flexible` conformation exploration.
* **Conformational Adjustments:** Checked `Sample nitrogen inversions` and `Sample ring conformations`.
* **Torsional Sampling:** Biased sampling activated for `All predefined functional groups`.
* **Scoring Constraints:** Checked `Add Epik state penalties to docking score` to account for ionization costs.
* **Van der Waals Scaling:** Scaled at a factor of `0.80` with a partial charge cutoff of `0.15`.
</details>

---

## 🔋 4. Binding Free Energy Estimation (MM-GBSA)
To calculate exact thermodynamic binding affinities and eliminate false positives from rigid docking models, the top **30 scoring compounds** from the XP run—along with the 6 reference standards—were rescored using **Schrödinger Prime MM-GBSA**.

* **Solvation Model:** **VSGB** (Variable Surface Generalized Born) model.
* **Force Field:** **OPLS4**.

<details>
<summary>⚙️ View Local Protein Flexibility Parameters</summary>

* **Protein Flexibility Options:** Fixed distance from ligand set to `0.0 Å`.
* **Sampling Method:** Local energy minimization (`Minimize`) performed exclusively on the bound ligand within the pocket matrices.
</details>

### 📊 Dataset Reference
> 📂 **Data Availability:** The complete matrix containing competitive HTVS, SP, XP scores, and final MM-GBSA free energies is stored in the project database.
> 
> 👉 **View the complete data sheet here:** [**`SLC7A11_Virtual_Screening_Data.xlsx`**](./data/SLC7A11_Virtual_Screening_Data.xlsx)

---

## 🧬 5. Physiochemical & Drug-Likeness Profiling
To confirm molecular viability, the top 10 candidate compounds and the 6 controls were evaluated for adherence to strict Lipinski's Rule of Five (Ro5) criteria.

* **Zero Lipinski Violations:** While prominent clinical references (Erastin, Imidazole Ketone Erastin, and PX-8) exhibit 1 or more structural rule violations, all 10 novel candidate compounds adhere strictly to zero violations.
* **Balanced Properties:** Selected hits showcase highly uniform profiles (Molecular Weight: 300–493 Da; TPSA: 71–165 Å²), indicating an optimal balance for potential cellular uptake.

> 👉 **View the descriptive matrix document:** [**`Descriptors and drug likeness values of Top 10 + Standards.docx`**](./data/Descriptors%20and%20drug%20likeness%20values%20of%20Top%2010%20+%20Standards.docx)

---

## 💊 6. In Silico ADMET Functional Screening
As a final downstream safety filter, the top 10 leads and 6 benchmark controls were profiled across essential pharmacokinetic and physiological endpoints using predictive QSAR models in **ADMETlab 2.0**.

* **Absorption/Distribution:** Characterized via Human Intestinal Absorption (HIA), Caco-2 permeability, Plasma Protein Binding (PPB), and Blood-Brain Barrier (BBB) indexes.
* **Metabolic Clearance & Toxicity:** Tracked against critical Cytochrome P450 isoforms (CYP3A4, CYP2D6, CYP2C9), half-life profiles ($t_{1/2}$), Drug-Induced Liver Injury (DILI) probability, Ames mutagenicity, and hERG channel inhibition.

> 👉 **View the complete predictive profile document:** [**`ADMET ANALYSIS of Ten Compounds and Standards.docx`**](./data/ADMET%20ANALYSIS%20of%20Ten%20Compounds%20and%20Standards.docx)

***

## 🚀 Project Roadmap & Current Progress
- [x] Target Isolation (7EPZ - Chain B) & Protein Preparation
- [x] Source Library Import & LigPrep (5,089 Compounds Restructured)
- [x] Drug-Likeness Filtering via QikProp Lipinski RO5 (2,665 Leads Remaining)
- [x] Structure-Based AHN Pharmacophore Modeling & Database Screen (2,419 Hits Isolated)
- [x] Receptor Grid Generation & Pocket Coordinate Mapping (X:127.06, Y:125.07, Z:122.21)
- [x] Hierarchical Virtual Screening Cascade (Glide HTVS $\rightarrow$ SP $\rightarrow$ XP Docking)
- [x] Binding Free Energy Estimation via Prime MM-GBSA (Top 30 + 6 Standards)
- [x] Advanced Toxicity & Pharmacokinetic Profiling via ADMETlab 2.0 (Top 10 + 6 Standards)
- [ ] Structural Refinement & Rational Lead Optimization via ChemDraw Professional
