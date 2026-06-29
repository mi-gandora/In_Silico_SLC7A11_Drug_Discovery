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

## 🧬 Molecular Workflow & Protocols
### 1. Protein Preparation
The cryo-EM structure of the human cystine-glutamate antiporter SLC7A11 was retrieved from the Protein Data Bank (**PDB ID: 7EPZ**). 

For the simulation environment, **Chain B** was isolated and processed using the **Schrödinger Protein Preparation Wizard** through the following protocol:
* **Bond Orders:** Assigned bond orders and utilized the Chemical Component Dictionary (CCD) database for accuracy.
* **Hydrogen Addition:** Hydrogens were added to the structure to satisfy valency requirements (without removing original hydrogens).
* **Structural Bonds:** Generated zero-order bonds to metals and created disulfide bonds to ensure proper structural integrity.
* **Epik Optimization:** Heteroatom ionization and tautomeric states were generated using Epik at a physiological pH range of $7.0 \pm 2.0$.

### 2. Ligand Preparation (LigPrep)
A library of **5,100 screening compounds** was obtained from **FutureGRIN NextGen**. Upon structural verification during workspace import, **5,089 compounds** were successfully integrated into Maestro, with 11 structurally invalid entries omitted.

The chemical library was then prepared using **Schrödinger LigPrep** to generate high-quality 3D structures ready for computational screening under the following criteria:
* **Force Field:** The **OPLS4** force field was applied for geometric energy minimization.
* **Ionization Treatment:** The structures were explicitly **neutralized** during preparation.
* **Salt Removal:** The `Desalt` option was enabled to remove counterions and isolate the parent therapeutic fragments. Tautomer generation was deactivated.
* **Stereochemistry:** Chiral centers were handled using the `Retain specified chiralities (vary other chiral centers)` protocol to preserve the core configurations defined by the source library.

### 3. Early Drug-Likeness Filtering & Database Screening

To systematically narrow down the screening library to the most viable starting candidates before molecular docking, a multi-stage filtering funnel was applied.

The **5,089** prepared ligands were evaluated using **Schrödinger QikProp** to compute physical and chemical properties. A strict drug-likeness filter was applied, discarding any molecule that exhibited a single violation of **Lipinski's Rule of Five (RO5)**:
* **Molecular Weight (MW):** $< 500\text{ Da}$
* **Hydrogen Bond Donors (HBD):** $\le 5$
* **Hydrogen Bond Acceptors (HBA):** $\le 10$
* **Octanol/Water Partition Coefficient (Log P):** $< 5$

* **Result:** **2,665 compounds** completely satisfied all Lipinski criteria with zero violations and were progressed to pharmacophore screening.

### 4. Structure-Based Pharmacophore Modeling (Schrödinger Phase)
To capture the essential spatial and chemical requirements for binding within the active pocket of SLC7A11 (Chain B), a structure-based pharmacophore model was developed using **Schrödinger Phase** from the receptor-ligand complex.

The resulting hypothesis identified a **3-feature pharmacophore model (AHN)** defined by the specific geometric coordinates and energetic contributions of the binding site:
* **Hydrophobic Region (H3):** Identifies critical hydrophobic interactions within the pocket (Score: -0.15 kcal/mol).
* **Hydrogen Bond Acceptor (A4):** Maps out essential hydrogen-accepting alignments near coordinating residues (Score: -1.00 kcal/mol).
* **Negative Ionic Feature (N15):** Captures favorable electrostatic interactions with positively charged side chains inside the transporter channel (Score: -0.30 kcal/mol).

* **Exclusion Volumes:** As visualized in `Pharmacophore picture.jpg`, a stringent shell of exclusion spheres (teal volumes) was incorporated based on the receptor's steric boundaries to ensure that screened hits match the spatial contours of the pocket without causing steric clashes.

### 5: Pharmacophore-Based Virtual Screening (Schrödinger Phase)
Using the structure-based **AHN hypothesis** (Hydrophobic `H3`, Hydrogen Bond Acceptor `A4`, and Negative Ionic `N15`) generated from the receptor-ligand complex, the 2,665 drug-like compounds were subjected to a database screen. 

* **Screening Criteria:** Compounds were required to match at least **2 out of the 3** core pharmacophoric features ($\ge 66.7\%$ feature match) while entirely avoiding the receptor's steric exclusion volumes.
* **Result:** **2,419 compounds** successfully cleared the pharmacophore screen and were isolated as the finalized candidate library for advanced molecular docking simulations.

### 6. Receptor Grid Generation (Schrödinger Glide)
Before initiating molecular docking simulations, the structural boundaries of the SLC7A11 binding cavity were explicitly mapped out using **Schrödinger Glide Receptor Grid Generation** to ensure targeted ligand placement.

The grid generation parameters were defined according to the native pocket environment:
* **Active Site Definition:** The reference bound ligand within the cryo-EM structure was selected to automatically center the binding coordinates and exclude it from the final grid volume.
* **Grid Center Coordinates:** The centroid of the binding pocket was locked at the following exact Cartesian coordinates: 
  $$\mathbf{X: 127.06, \quad Y: 125.07, \quad Z: 122.21}$$
* **Box Dimensions:** The enclosing box size was dynamically configured to accommodate incoming screening ligands matching or similar in size to the reference native workspace ligand.
* **Van der Waals Scaling:** Receptor nonpolar atoms were scaled with a factor of `1.0` and a partial charge cutoff of `0.25` to permit standard rigid-receptor structural boundaries during the docking process.

### 7. High-Throughput Virtual Screening Cascade (Schrödinger Glide)
To identify potential lead compounds with high binding affinity for the SLC7A11 pocket, a hierarchical virtual screening cascade was performed using **Schrödinger Glide**. 

As a rigorous scientific control framework, **5 standard reference drugs** (Erastin, Sorafenib, Imidazole Ketone Erastin, HG-106, and Sulfasalazine) along with the co-crystallized control ligand (**PX-8**) were prepared via LigPrep and screened simultaneously alongside the main compound library.

#### Phase A: High-Throughput Virtual Screening (HTVS)
* **Input Library:** 2,419 pharmacophore-matched hits + 6 control reference standards (Total: 2,425 entries selected in the Project Table).
* **Precision Mode:** HTVS (High-Throughput Virtual Screening).
* **Settings Configuration:**
  * **Ligand Sampling:** `Flexible` mode enabled to explore conformational space.
  * **Inversions/Conformations:** Checked `Sample nitrogen inversions` and `Sample ring conformations`.
  * **Torsional Sampling:** Biased sampling set for `All predefined functional groups`.
  * **Scoring Penalty:** Checked `Add Epik state penalties to docking score` to incorporate ionization energy costs.
  * **Van der Waals Scaling:** Scaled at a factor of `0.80` with a partial charge cutoff of `0.15` to accommodate ligand fit.

#### Phase B: Standard Precision (SP) & Extra Precision (XP) Refining
Following HTVS, the screening pipeline was systematically compressed to maximize accuracy:
1. **Glide SP Docking:** Executed on the top-performing **100 compounds** from the HTVS run, keeping all 6 reference standards embedded in the cohort.
2. **Glide XP Docking:** Run back-to-back on the same top-scoring cohort to capture highly precise structural interactions, water elimination configurations, and stringent electrostatic filtering within the active site.

### 8. Binding Free Energy Estimation (Schrödinger Prime MM-GBSA)
To calculate thermodynamic binding affinities and eliminate false positives from the rigid-receptor docking grid, the top **30 scoring compounds** from the XP run—alongside the 6 reference control standards—were subjected to MM-GBSA rescoring using **Schrödinger Prime**.

* **Input Setup:** Complexes were derived from separated ligand and protein structures, pulling ligands directly from selected entries in the Project Table and the receptor from the active Workspace entry.
* **Solvation Model:** **VSGB** (Variable Surface Generalized Born) model.
* **Force Field:** **OPLS4**.
* **Protein Flexibility:** Fixed distance from ligand set to `0.0 Å` with a `Minimize` sampling method to perform local energy minimization on the bound ligand within the rigid binding site.
  
## 📊 Virtual Screening Results & Benchmarking

> 📂 **Data Availability Note:** The complete dataset—including the competitive HTVS, SP, and XP docking scores along with the finalized Prime MM-GBSA binding free energies for all top 30 candidate compounds and the 6 control reference standards—is fully documented and available for download.
> 
> 👉 **View the complete dataset here:** [**`SLC7A11_Virtual_Screening_Data.xlsx`**](./Data/SLC7A11_Virtual_Screening_Data.xlsx)

### 📈 Comparative Overview
Initial screening highlights show that several novel small-molecule candidates from the library achieved significantly stronger Extra Precision (XP) docking scores ($\le -9.0\text{ kcal/mol}$) compared to the clinical reference standards (such as Erastin and Sorafenib). This indicates highly promising competitive binding within the active pocket of the SLC7A11 transporter channel, setting a strong foundation for subsequent lead optimization.

### 9.  Post-Simulation ADMET Profiling (ADMETlab 2.0)

To complement the physics-based docking scores, the top 10 lead compounds along with the 6 reference standards were subjected to high-throughput *in silico* property evaluation via **ADMETlab 2.0**. SMILES strings were extracted from Maestro and profiled across key ADMET endpoints using optimized predictive QSAR models:

* **Absorption & Distribution:** Human Intestinal Absorption (HIA), Caco-2 permeability, Plasma Protein Binding (PPB), and Blood-Brain Barrier (BBB) permeability to ensure optimal oral bioavailability and systemic distribution profiles.
* **Metabolism:** Substrate and inhibition profiles against key Cytochrome P450 isoforms (CYP3A4, CYP2D6, CYP2C9) to identify potential metabolic clearance pathways or drug-drug interactions.
* **Excretion & Toxicity:** *In silico* evaluation of plasma half-life ($t_{1/2}$), along with high-priority safety endpoints including Drug-Induced Liver Injury (DILI), hERG potassium channel inhibition, Ames mutagenicity, and acute rat oral toxicity ($LD_{50}$).

> 📂 **Data Availability Note:** The full predictive matrix containing all 16 evaluation models for the top 10 hits and control compounds has been compiled as a reference document.
> 
> 👉 **View the complete predictive profile here:** [**`ADMET ANALYSIS of Ten Compounds and Standards.docx`**](./Data/ADMET%20ANALYSIS%20of%20Ten%20Compounds%20and%20Standards.docx)

### 🎯 Key ADMET Highlights
The predictive QSAR layer successfully highlighted critical safety dffferentiators among the top structural clusters. For instance, **Compound 29629** not only demonstrated a robust docking profile but also showed a highly favorable safety margin with a significantly low predicted risk for Drug-Induced Liver Injury (DILI: 0.084) compared to clinical standards like Sorafenib (DILI: 0.998) and Erastin (DILI: 0.826), marking it as an exceptional candidate for downstream scaffolding optimization.
 ---

## 🚀 Project Roadmap & Current Progress
- [x] Target Isolation (7EPZ - Chain B) & Protein Preparation
- [x] Source Library Import & LigPrep (5,089 Compounds Restructured)
- [x] Drug-Likeness Filtering via QikProp Lipinski RO5 (2,665 Leads Remaining)
- [x] Structure-Based AHN Pharmacophore Modeling & Database Screen (2,419 Hits Isolated)
- [x] Receptor Grid Generation & Pocket Coordinate Mapping (X:127.06, Y:125.07, Z:122.21)
- [x] Hierarchical Virtual Screening Cascade (Glide HTVS $\rightarrow$ SP $\rightarrow$ XP Docking)
- [x] Binding Free Energy Estimation via Prime MM-GBSA (Top 30 + 6 Standards)
- [x] Advanced Toxicity & Pharmacokinetic Profiling via ADMETlab 2.0 (BBB, DILI, etc.)
- [ ] Structural Refinement & Rational Lead Optimization via ChemDraw Professional
