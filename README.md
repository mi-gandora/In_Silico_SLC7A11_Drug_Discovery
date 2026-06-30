# In_Silico_SLC7A11_Drug_Discovery
Computer-Aided Drug Design (CADD) pipeline for the identification of ferroptosis-inducing compounds targeting the SLC7A11 protein.

## 📌 Project Overview
Therapy resistance remains a critical bottleneck in oncology, often driven by the upregulation of antioxidant defense mechanisms. The cystine/glutamate antiporter **SLC7A11** (xCT) plays a pivotal role in maintaining intracellular glutathione (GSH) levels, shielding cancer cells from oxidative stress. 

This project utilizes a Computer-Aided Drug Design (CADD) pipeline to identify novel small-molecule inhibitors of SLC7A11. By blocking cystine uptake, these compounds aim to deplete GSH, elevate lipid peroxides, and selectively trigger **ferroptosis**—an iron-dependent form of regulated cell death—in therapy-resistant cancer phenotypes.

## 🛠️ Computational Toolkit
* **Platform:** Schrödinger Maestro Suite (Desktop Version), ChemDraw, & Web-based Predictors
* **Environment:** Windows 11 
* **Key Modules & Tools:**
  * **Protein Preparation Wizard:** Receptor optimization
  * **LigPrep:** Ligand ionization and stereoisomer generation
  * **QikProp:** Physics-based ADMET descriptor prediction
  * **ADMETlab 2.0:** ADMET & profiling 
  * **Phase:** Pharmacophore hypothesis modeling
  * **Glide:** Molecular Docking (HTVS/SP/XP) 
  * **Prime MM-GBSA:** Binding free energy estimation
  * **ChemDraw Professional (v15.0):** Structural modification & rational lead optimization

    ---
## 🗺️ Table of Contents
* [🧬 1. Protein Target Preparation](#1-protein-target-preparation)
* [🔌 2. Ligand Preparation & Energetic Minimization](#2-ligand-preparation--energetics-minimization)
* [⏳ 3. Physicochemical Filtering & QikProp Drug-Likeness Screen](#3-physicochemical-filtering--qikprop-drug-likeness-screen)
* [🔬 4. Structure-Based Pharmacophore Screen](#4-structure-based-pharmacophore-screen)
* [🔎 5. Pharmacophore-Based Virtual Screening](#5-pharmacophore-based-virtual-screening)
* [🎛️ 6. Receptor Grid Generation](#6-receptor-grid-generation)
* [🌊 7. Hierarchical Virtual Screening Cascade](#7-hierarchical-virtual-screening-cascade)
* [🔋 8. Binding Free Energy Estimation (MM-GBSA)](#8-binding-free-energy-estimation-mm-gbsa)
* [🧬 9. Physiochemical & Drug-Likeness Profiling](#9-physiochemical--drug-likeness-profiling)
* [💊 10. In Silico ADMET Functional Screening](#10-in-silico-admet-functional-screening)
* [🛠️ 11. Rational Lead Optimization & R-Group Substitution](#11-Rational-lead-optimization-&-r-group-substitution)
* [🚀 Project Roadmap & Progress](#-project-roadmap--current-progress)

---

## 🧬 1. Protein Target Preparation
The 3D atomic structure of the human cystine-glutamate antiporter **SLC7A11** was retrieved from the Protein Data Bank (**PDB ID: 7EPZ**) to establish a clean structural foundation for docking experiments.

* **Target Isolate:** Chain B was explicitly isolated from the cryo-EM complex to serve as the simulation target framework.
* **Optimization Workflow:** Structure refinement and preprocessing were executed using the **Schrödinger Protein Preparation Wizard** to resolve architectural anomalies.

<details>
<summary>⚙️ View Protein Preparation Protocol & Processing Criteria</summary>

The isolated protein chain was systematically prepared and minimized under the following standard parameters:
* **Bond Orders:** Formal bond orders were assigned across all residues, referencing the Chemical Component Dictionary (CCD) database to guarantee proper atomic valence.
* **Hydrogen Addition:** Hydrogens were added to the structural coordinate file to completely satisfy electronic valency requirements without removing any original structural hydrogens.
* **Structural Bonds:** Zero-order bonds were generated to coordinate metals, and cross-linking disulfide bonds were mapped out to maintain proper structural integrity.
* **Epik Optimization:** Heteroatom ionization and tautomeric states were dynamically generated using **Epik** at a biologically target-relevant physiological pH range of $7.0 \pm 2.0$.
</details>

---

## 🔌 2. Ligand Preparation (LigPrep)
A library of **5,100 screening compounds** was obtained from **FutureGRIN NextGen**. Upon structural verification during workspace import, **5,089 compounds** were successfully integrated into Maestro, with 11 structurally invalid entries omitted.

<details>
<summary>⚙️ View LigPrep Processing Parameters & Criteria</summary>

The chemical library was prepared using **Schrödinger LigPrep** to generate high-quality 3D structures ready for computational screening under the following criteria:
* **Force Field:** The **OPLS4** force field was applied for geometric energy minimization.
* **Ionization Treatment:** The structures were explicitly **neutralized** during preparation.
* **Salt Removal:** The `Desalt` option was enabled to remove counterions and isolate the parent therapeutic fragments. Tautomer generation was deactivated.
* **Stereochemistry:** Chiral centers were handled using the `Retain specified chiralities (vary other chiral centers)` protocol to preserve the core configurations defined by the source library.
</details>

---

## ⏳ 3. Physicochemical Filtering & QikProp Drug-Likeness Screen
To systematically narrow down the screening library to the most viable starting candidates before molecular docking, a rigorous, multi-stage filtering funnel was applied.

* **Screening Standard:** The **5,089** structurally verified ligands were evaluated using **Schrödinger QikProp** to compute physical and chemical descriptors.
* **Yield:** A total of **2,665 compounds** completely satisfied all criteria with zero violations and were progressed to the pharmacophore screening phase.

<details>
<summary>⚙️ View QikProp Drug-Likeness Criteria & Filters</summary>

A strict drug-likeness filter was applied, discarding any molecule that exhibited a single violation of **Lipinski's Rule of Five (RO5)** under the following threshold parameters:
* **Molecular Weight (MW):** $< 500\text{ Da}$
* **Hydrogen Bond Donors (HBD):** $\le 5$
* **Hydrogen Bond Acceptors (HBA):** $\le 10$
* **Octanol/Water Partition Coefficient (Log P):** $< 5$
</details>

---

## 🔬 4. Structure-Based Pharmacophore Modeling
To capture the essential spatial and chemical requirements for binding within the active pocket of SLC7A11 (Chain B), a structure-based pharmacophore model was developed using **Schrödinger Phase** directly from the native receptor-ligand complex.

* **Result:** The analysis yielded a finalized **3-feature pharmacophore model (AHN)** driven by the specific geometric coordinates and energetic profiles of the binding site pocket.

![Structure-Based AHN Pharmacophore Hypothesis](Images/Pharmacophore_picture.jpg)

<details>
<summary>⚙️ View Pharmacophore Hypothesis Spatial Weights & Energetics</summary>

The 3-feature framework defines the critical chemical checkpoints necessary for targeted pocket complementarity:
* **Hydrophobic Region (H3):** Identifies critical hydrophobic interactions within the pocket (Score: -0.15 kcal/mol).
* **Hydrogen Bond Acceptor (A4):** Maps out essential hydrogen-accepting alignments near coordinating residues (Score: -1.00 kcal/mol).
* **Negative Ionic Feature (N15):** Captures favorable electrostatic interactions with positively charged side chains inside the transporter channel (Score: -0.30 kcal/mol).
</details>
---

## 🔎 5. Pharmacophore-Based Virtual Screening
Using the finalized structure-based **AHN hypothesis**, the structural database was subjected to an alignment-based virtual screening run to filter out geometrically incompatible configurations.

* **Screening Library Pool:** The **2,665** drug-like compounds isolated from the QikProp screen served as the entry dataset.
* **Yield:** A total of **2,419 compounds** successfully cleared the feature parameters and were isolated as the finalized candidate library for advanced molecular docking simulations.

<details>
<summary>⚙️ View Database Alignment & Selection Criteria</summary>

Molecules were screened and scored based on structural fitness within the dynamic binding envelope:
* **Feature Match Tolerance:** Compounds were strictly required to match at least **2 out of the 3** core pharmacophoric features ($\ge 66.7\%$ total feature match) to be considered active.
* **Exclusion Volumes:** As visualized in `Pharmacophore picture.jpg`, a stringent shell of exclusion spheres (teal volumes) was incorporated based on the receptor's steric boundaries to ensure that screened hits match the spatial contours of the pocket without causing steric clashes.
</details>
---

## 🎛️ 6. Receptor Grid Generation
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

## 🌊7. Hierarchical Virtual Screening Cascade
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

## 🔋 8. Binding Free Energy Estimation (MM-GBSA)
To calculate exact thermodynamic binding affinities and eliminate false positives from rigid docking models, the top **30 scoring compounds** from the XP run—along with the 6 reference standards—were rescored using **Schrödinger Prime MM-GBSA**.

* **Solvation Model:** **VSGB** (Variable Surface Generalized Born) model.
* **Force Field:** **OPLS4**.

<details>
<summary>⚙️ View Local Protein Flexibility Parameters</summary>

* **Protein Flexibility Options:** Fixed distance from ligand set to `0.0 Å`.
* **Sampling Method:** Local energy minimization (`Minimize`) performed exclusively on the bound ligand within the pocket matrices.
</details>

### 📊 Dataset Reference
> 📂 **Data Availability Note:** The complete dataset—including the competitive HTVS, SP, and XP docking scores along with the finalized Prime MM-GBSA binding free energies for all top 30 candidate compounds and the 6 control reference standards—is fully documented and available for download.
> 
> 👉 **View the complete data sheet here:** [**`SLC7A11_Virtual_Screening_Data.xlsx`**](./Data/SLC7A11_Virtual_Screening_Data.xlsx)

---

## 🧬 9. Physiochemical & Drug-Likeness Profiling
To confirm molecular viability, the top 10 candidate compounds and the 6 controls were evaluated for adherence to strict Lipinski's Rule of Five (Ro5) criteria.

* **Zero Lipinski Violations:** While prominent clinical references (Erastin, Imidazole Ketone Erastin, and PX-8) exhibit 1 or more structural rule violations, all 10 novel candidate compounds adhere strictly to zero violations.
* **Balanced Properties:** Selected hits showcase highly uniform profiles (Molecular Weight: 300–493 Da; TPSA: 71–165 Å²), indicating an optimal balance for potential cellular uptake.

> 📂 **Data Availability Note:** The complete predictive physiochemical matrix—containing precise calculations for molecular weight (MW), hydrogen bond acceptors (nHA), hydrogen bond donors (nHD), topological polar surface area (TPSA), predicted partition coefficient (LogP), and predicted water solubility (LogS)—has been compiled as a standalone reference.
> 
> 👉 **View the descriptive matrix document:** [**`Drug-Likeness Prediction of Ten Compounds and Standards.docx`**](./Data/Drug-Likeness%20Prediction%20of%20Ten%20Compounds%20and%20Standards.docx)

### 📈 Comparative Overview
Initial screening highlights show that several novel small-molecule candidates from the library achieved significantly stronger Extra Precision (XP) docking scores ($\le -9.0\text{ kcal/mol}$) compared to the clinical reference standards (such as Erastin and Sorafenib). This indicates highly promising competitive binding within the active pocket of the SLC7A11 transporter channel, setting a strong foundation for subsequent lead optimization.

---

## 💊 10. In Silico ADMET Functional Screening
As a final downstream safety filter, the top 10 leads and 6 benchmark controls were profiled across essential pharmacokinetic and physiological endpoints using predictive QSAR models in **ADMETlab 2.0**.

* **Absorption/Distribution:** Characterized via Human Intestinal Absorption (HIA), Caco-2 permeability, Plasma Protein Binding (PPB), and Blood-Brain Barrier (BBB) indexes.
* **Metabolic Clearance & Toxicity:** Tracked against critical Cytochrome P450 isoforms (CYP3A4, CYP2D6, CYP2C9), half-life profiles ($t_{1/2}$), Drug-Induced Liver Injury (DILI) probability, Ames mutagenicity, and hERG channel inhibition.

> 📂 **Data Availability Note:** The full predictive matrix containing all 16 evaluation models for the top 10 hits and control compounds has been compiled as a reference document.
> 
> 👉 **View the complete predictive profile document:** [**`ADMET ANALYSIS of Ten Compounds and Standards.docx`**](./Data/ADMET%20ANALYSIS%20of%20Ten%20Compounds%20and%20Standards.docx)

### 🎯 Key ADMET Highlights
The predictive QSAR layer successfully highlighted critical safety differentiators among the top structural clusters. For instance, **Compound 29629** not only demonstrated a robust docking profile but also showed a highly favorable safety margin with a significantly low predicted risk for Drug-Induced Liver Injury (DILI: 0.084) compared to clinical standards like Sorafenib (DILI: 0.998) and Erastin (DILI: 0.826), marking it as an exceptional candidate for downstream scaffolding optimization.

---

## 🛠️ 11. Rational Lead Optimization & R-Group Substitution
To maximize therapeutic efficacy and structural interactions within the active binding site of SLC7A11, the top two small-molecule candidates (**Compound 29629** and **Compound 25904**) were advanced to a rational lead optimization phase. Visual inspection of their bound conformations inside the receptor cavity revealed exposed, non-interacting hydroxyl groups ($\text{-OH}$). 

To exploit these positions, a library of **20 structurally diverse functional group substituents** was systematically introduced at the hydroxyl site using **ChemDraw Professional**, generating the **L1** (derived from Compound 29629) and **L2** (derived from Compound 25904) analogue series.

* **Evaluation Pipeline:** All 40 engineered analogues were re-subjected to Extra Precision (XP) molecular docking, Prime MM-GBSA thermodynamic rescoring, and downstream ADMET lab 2.0 safety profiling.
* **Final Selection:** Based on a rigorous multi-parameter optimization trade-off, three finalized derivatives—**L1_OCH2CH2OH**, **L1_OCH3**, and **L2_OCH2CH2OH**—were isolated as the ultimate optimized leads for this study.

<details>
<summary>⚙️ View Analogue Selection Parameters & Screening Data</summary>

The 40 specialized analogues were benchmarked using a stringent matrix balance prioritizing high thermodynamic stability (MM-GBSA) without compromising critical pharmacological safety baselines:

1. **L1_OCH2CH2OH (Lead 1 2-hydroxyethoxy derivative):**
   * **Thermodynamic Affinity:** Demonstrated a highly robust binding profile with an XP Docking score of `-8.471 kcal/mol` and an exceptionally favorable Prime MM-GBSA free energy value of `-59.83 kcal/mol`.
   * **Safety Matrix:** Exhibited a remarkably safe profile with zero blood-brain barrier penetration risk (BBB: 0), minimal cardiotoxicity (hERG inhibition: 0.424), and an extremely low hepatotoxicity probability (DILI score: 0.110).

2. **L1_OCH3 / L1_OMe (Lead 1 methoxy derivative):**
   * **Thermodynamic Affinity:** Achieved well-balanced energetics including an XP Docking score of `-8.359 kcal/mol` and a stable MM-GBSA value of `-51.24 kcal/mol`.
   * **Safety Matrix:** Remained clear of central nervous system side effects (BBB: 0) while maintaining a safe liver profile (DILI score: 0.121).

3. **L2_OCH2CH2OH (Lead 2 2-hydroxyethoxy derivative):**
   * **Thermodynamic Affinity:** Unlocked significantly enhanced binding stability over its parent compound, achieving an XP Docking score of `-8.170 kcal/mol` paired with a phenomenal MM-GBSA binding free energy of `-61.53 kcal/mol`.
   * **Safety Matrix:** Showcased a highly balanced clearance rate ($t_{1/2}$: 1.330) and an exceptional cardiac safety margin with an extremely low predicted hERG channel inhibition risk (0.210).
</details>

### 📊 Optimized Analogue Dataset Reference
> 📂 **Data Availability Note:** The complete tracking sheets containing XP docking scores, Prime MM-GBSA binding values, and the complete 16-parameter ADMETlab 2.0 evaluation profiles across all 40 generated L1 and L2 functional group variations are organized as separate data sheets.
> 
> 👉 **View the optimization data here:**
> * [**`XP DOCKING AND MMGBSA OF COMPD 25904 ANALOGS.docx`**](./Data/XP%20DOCKING%20AND%20MMGBSA%20OF%20COMPD%2025904%20ANALOGS)
> * [**`XP DOCKING AND MMGBSA OF COMPD 29629 ANALOGS.docx`**](./Data/XP%20DOCKING%20AND%20MMGBSA%20OF%20COMPD%2029629%20ANALOGS)
> * [**`ADMET ANALYSIS OF COMPOUND 25904 ANALOGS.docx`**](./Data/ADMET%20ANALYSIS%20OF%20COMPOUND%2025904%20ANALOGS)
> * [**`ADMET ANALYSIS OF COMPOUND 29629 ANALOGS.docx`**](./Data/ADMET%20ANALYSIS%20OF%20COMPOUND%2029629%20ANALOGS)


---
---

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
- [x] Structural Refinement & Rational Lead Optimization via ChemDraw Professional
- [x] Bioisosteric R-Group Substitution & Selection of Ultimate Optimized Leads
