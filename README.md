TNBC_Modeling
Supplementary material for the article "Precision Therapeutics in triple-negative breast cancer: Boolean Network Control and Pharmacological Mapping of Individualized Models"
This repository contains the computational framework, patient-specific datasets, supplementary resources, and analysis workflows developed for the study:
"Precision Therapeutics in Triple-Negative Breast Cancer: Boolean Network Control and Pharmacological Mapping of Individualized Models."
The repository provides all materials required to reproduce the computational analyses presented in the manuscript, including the generation of individualized Boolean network models, attractor-based phenotypic characterization, control target identification, pharmacological mapping, and the production of the associated results and figures.
The resources are organized to ensure transparency, reproducibility, and reusability of the study, enabling reviewers and readers to replicate the analyses and explore the modeling framework applied to patient-specific triple-negative breast cancer (TNBC) datasets.
In addition to the materials provided in this GitHub repository, a complementary dataset required for the reproduction of the analyses is publicly available through Zenodo at:
https://doi.org/10.5281/zenodo.21205999.

 Reproducibility Workflow

The analyses reported in this study can be reproduced by following the computational workflow implemented in the repository. Starting from any patient identifier listed in the file `TNBC_Patient_ID` (located in the `supplementary_data` directory), users can execute the Python notebooks contained in the `scripts` directory in the prescribed order. Throughout the workflow, the scripts make use of the supporting files provided in the `supplementary_data` directory to generate patient-specific models, intermediate results, and final outputs.
For each selected patient, the complete set of generated results can be compared with the corresponding reference files available in the appropriate patient folder within the `patient_records` directory. This organization allows readers and reviewers to reproduce every step of the analysis and verify the consistency of the results presented in the manuscript.

The notebook py_1.ipynb, located in the script directory, requires three inputs:
S1.csv (available at https://doi.org/10.5281/zenodo.21205999), which contains the gene‑expression matrix of the GSE65194 dataset.
network_genes.csv, located in the supplementary_data directory, listing all genes included in the regulatory network analyzed in this report.
A manual user‑provided input at line 132 of the notebook, where the user specifies the TNBC patient identifier. Valid identifiers are listed in the file TNBC_Patient_ID.csv.
Upon execution, the notebook generates an output file named TNBC_patient_GSM158xxxx_vs_normals.csv, stored in the patient subfolders of the Patient_records directory. This file reports the expression values of the selected TNBC patient (first column) alongside the expression values of non‑cancer samples for all network genes included in the analysis.

The notebook py_2.ipynb takes as input the file
TNBC_patient_GSM158xxxx_vs_normals.csv, generated in the previous step, and computes the log Fold Change (logFC) for all network genes of the selected patient. The script produces an output file named logFC_patient_x.csv, which reports the logFC values quantifying the differential expression of each network gene in the patient relative to the corresponding non‑cancer samples.

The notebook py_3.ipynb takes as input the file GRN.csv, which specifies all regulatory interactions among the genes in the network together with their interaction type—activation (+1) or inhibition (–1)—and the file logFC_patient_x.csv, containing the patient‑specific log Fold Change values computed in the previous step.
Based on these inputs, the notebook performs a pruning procedure on the regulatory network and produces two output files:
pruned_net.csv, which contains all network edges that were retained after the pruning process;
deleted_links.csv, which lists all edges removed from the network structure during pruning.The pruning is source-driven: an edge is evaluated only when its source gene is differentially expressed (|logFC| ≥ 1.0), and it is removed when the logFC of the target gene contradicts the sign of the interaction by more than the tolerance τ = 0.3; edges whose source gene is not differentially expressed are retained. The file GRN.csv provided here is the corrected version of the network (the sign of 16 of the 568 interactions was corrected).

The notebook py_4.ipynb, located in the script directory, takes as input the file S1.csv (available at https://doi.org/10.5281/zenodo.21205999), which contains the gene‑expression matrix of the GSE65194 dataset, together with the file network_genes.csv stored in the supplementary_data directory. Based on these inputs, the notebook performs a Booleanization procedure on the expression values of all network genes and produces an output file named TNBC_GSM158xxxx_BOOLEAN_p95.csv. In this file, the expression level of each network node is converted into a Boolean state (0 or 1) according to the statistical thresholding strategy implemented in the notebook.

The notebook py_5.ipynb, located in the script directory, takes as input the file network_rules.txt, stored in the supplementary_data directory, which contains the Boolean gene regulatory network constructed for this study. It also requires the file deleted_links.csv, which lists all regulatory interactions removed during the pruning procedure for the specific patient under analysis. 
Using these inputs, the notebook generates an output file named rules_pruned.txt, which contains the patient‑specific Boolean gene regulatory network. This pruned rule set reflects the regulatory structure tailored to the selected patient, identified by their unique TNBC ID.

The notebook py_6.ipynb, located in the script directory, takes as input the file rules_pruned.txt (referenced at line 260 of the notebook) and the file TNBC_GSM158xxxx_BOOLEAN_p95.csv (referenced at line 288).
Using these inputs, the notebook generates an output file named UNFILTERED_Report_TNBC_GSM158xxxx_BOOLEAN_p95.txt. This report lists all target nodes of the regulatory network together with their corresponding activating or inhibitory actions, thereby identifying the set of regulatory interventions required to drive the system toward the apoptotic configuration of the 26 target genes.

The notebook py_7.ipynb, located in the script directory, takes as input the files rules_pruned.txt and TNBC_GSM158xxxx_BOOLEAN_p95.csv, together with a manual user‑provided specification at line 15 of the notebook. In this manual input field, the user inserts the patient‑specific target genes and their recommended regulatory actions (1 for activation, 0 for inhibition), as listed in the file UNFILTERED_Report_TNBC_GSM158xxxx_BOOLEAN_p95.txt.
Using these inputs, the notebook produces two output files:
network_trajectory_highlighted_x.xlsx, which displays the full network trajectory from the initial Boolean configuration to the final attractor state;
Heatmap_Target_Attractor_x.png, which provides a heatmap visualization of this trajectory, highlighting the dynamic evolution of the target genes toward the attractor.

The notebook py_8.ipynb, located in the script directory, takes as input the file S1_D.xlsx, stored in the supplementary_data directory. This file contains the curated list of drug–target associations used in this study, specifying for each compound the corresponding therapeutic targets.
Using this input, the notebook produces an output file named output_drugs.xlsx, which reports the type of regulatory action—activation or inhibition—exerted by each drug on its associated therapeutic targets.

The notebook py_9.ipynb, located in the script directory, takes as input the file Drugs_1.xlsx, stored in the supplementary_data directory. This file is a curated subset of output_drugs.xlsx, in which all entries lacking a clearly defined activating or inhibitory drug action on the corresponding therapeutic targets have been removed, and further filtered by removing compounds identified as contraindicated or pro-tumorigenic in the TNBC context (diethylstilbestrol and dexamethasone) before drug selection.

All patient‑specific output files generated by the scripts are stored and can be inspected within the dedicated subdirectories located in the Patient_records folder of the repository (see the description below).

Structure of the Patient_records folder. The folder contains one subfolder per patient (41 subfolders, named PA_<n>...<xx>, where <n> is the progressive patient number). Each subfolder contains the ten files generated by the workflow: TNBC_patient_GSM158xxxx_vs_normals.csv, logFC_patient_<n>.csv, pruned_net_<n>.csv, deleted_links_<n>.csv, rules_pruned_<n>.txt, TNBC_GSM158xxxx_BOOLEAN_p95.csv, UNFILTERED_Report_TNBC_GSM158xxxx_BOOLEAN_p95.txt, network_trajectory_highlighted_<n>.xlsx, Heatmap_Target_Attractor_<n>.png and selected_drugs_result_<n>.csv. Two notes: (i) the report files of patients 12–41 end with _p95test.txt instead of _p95.txt (same content and format); (ii) selected_drugs_result_<n>.csv can be empty (e.g., patient 3) when no drug is assigned to the patient.

**Additional results and supplementary files.**

- `results_tables/`: Tables 2–6 of the article (files `TABLE_2` to `TABLE_6`, with the same numbering as in the manuscript). Table 3 reports the sensitivity analysis to the update scheme (asynchronous updates, with node-state inversion noise of 0% for 13 patients and 2% for patient 8); the corresponding output folders are `asincrony_NEW`. Table 4 lists the drug–target annotations retrieved from ChEMBL for the network genes; Table 5 is the patient-level results table (target-match scores before and after intervention, intervention size, driver nodes and imposed states, mapped drugs, unmatched driver nodes and Non_Driver_Target_Count). Definitions of the columns that need explanation:
  - `Non_Driver_Target_Count`: number of molecular targets modulated by the selected drug combination that are distinct from the driver nodes it was selected for. It is an in-network, topology-based proxy of off-target engagement and has not been validated against experimental or clinical adverse-effect data.
  - `Unmatched_Driver_Nodes`: driver nodes for which no drug was assigned; an empty cell means that all driver nodes were matched to a drug.
  - `Gene_Symbol_Match` (Table 4): "Exact" when the ChEMBL target corresponds to the network gene; "Fallback (verify manually)" when the drug was mapped through a related ChEMBL target.
  - `Directness_of_Interaction` (Table 4): "Direct binding" when the drug binds the target protein of the network gene; "Direct binding to ChEMBL target (not the network gene)" for fallback mappings.
- `supplementary_data/Exhaustive_k1_k2_NEW.xlsx`: exhaustive enumeration of all intervention sets of size k = 1 and k = 2, used to benchmark the genetic algorithm.
- `supplementary_data/Sensitivity_P4_P20_NEW.xlsx`: sensitivity analysis of the pruning tolerance (τ = 0.4) for patients 4 and 20.

- ### Strict drug–gene mapping (ChEMBL release 37)

The pharmacological mapping uses only direct, single-protein, directional drug–gene associations derived from the mechanism-of-action records of ChEMBL (release 37). An association is retained only if (i) the ChEMBL target is a single protein corresponding to the network gene, (ii) the interaction is annotated as direct, and (iii) the action type is clearly inhibitory (Boolean state 0) or activating (state 1). Associations mediated by nucleic acids, protein families, protein complexes, chimeric proteins or non-directional binding agents are excluded. The ASP optimization and the directional-consistency constraint are unchanged; only the search space differs from the one defined by `Drugs_1.xlsx`.

Files (folder `supplementary_data`):
- `build_drugs_direct.ipynb`: notebook that queries the ChEMBL web services for the compounds listed in `Drugs_1.xlsx`, applies the rule above and writes the two files below.
- `ChEMBL_mechanisms_raw.csv`: all 298 mechanism-of-action records retrieved, with the decision taken for each (`0`/`1` = retained with the Boolean state; otherwise the reason for exclusion).
- `Drugs_direct.xlsx`: the resulting 178 drug–gene associations (110 compounds, 83 genes), including the ChEMBL mechanism and source references. This is the search space used by the ASP drug-mapping step.
- `Drugs_1.xlsx`: curated drug list of the previous analysis (3,716 associations); it is the input of `build_drugs_direct.ipynb` and is kept for reproducibility.
- `Supplementary_File_S2_Pharmacology.xlsx`: filtering cascade, removed compounds, final association, the 16 associations of the previous analysis with the reason for their removal, a family-level variant (hypotheses only, not used in the conclusions) and a per-patient comparison.

Results (folder `results_tables`): `TABLE_4_NEW.xlsx` (final drug–target association with ChEMBL annotation) and `TABLE_5_NEW.xlsx` (driver nodes, mapped drugs and non-driver target counts per patient).

Note: a drug is assigned in 1 of the 41 patients (Patient 41: vorinostat, HDAC1 inhibition). The drug assignments are computational predictions derived from database annotations; they are not clinically validated treatments.
- `results_tables/Wilcoxon_paired_scores_NEW.png`: paired comparison of baseline and final target-match scores (Wilcoxon signed-rank test, descriptive).
- `results_tables/Figure5_interactive.html`: interactive version of Figure 5 (driver nodes, drugs and Non_Driver_Target_Count for each patient), viewable in any web browser.
supplementary_data/Supplementary_File_S2_Pharmacology.xlsx`: Supplementary File S2 of the article (pharmacological mapping: filtering cascade, removed compounds and final drug–target associations).
