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
Upon execution, the notebook generates an output file named TNBC_patient_GSM158xxxx_vs_normals.csv, stored in the Patient record directories. This file reports the expression values of the selected TNBC patient (first column) alongside the expression values of non‑cancer samples for all network genes included in the analysis.

The notebook py_2.ipynb takes as input the file
TNBC_patient_GSM158xxxx_vs_normals.csv, generated in the previous step, and computes the log Fold Change (logFC) for all network genes of the selected patient. The script produces an output file named logFC_patient_x.csv, which reports the logFC values quantifying the differential expression of each network gene in the patient relative to the corresponding non‑cancer samples.

The notebook py_3.ipynb takes as input the file GRN.csv, which specifies all regulatory interactions among the genes in the network together with their interaction type—activation (+1) or inhibition (–1)—and the file logFC_patient_x.csv, containing the patient‑specific log Fold Change values computed in the previous step.
Based on these inputs, the notebook performs a pruning procedure on the regulatory network and produces two output files:
pruned_net.csv, which contains all network edges that were retained after the pruning process;
deleted_links.csv, which lists all edges removed from the network structure during pruning.

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

The notebook py_9.ipynb, located in the script directory, takes as input the file Drugs_1.xlsx, stored in the supplementary_data directory. This file is a curated subset of output_drugs.xlsx, in which all entries lacking a clearly defined activating or inhibitory drug action on the corresponding therapeutic targets have been removed.
Using this refined input, the notebook produces an output file named selected_drugs_result_x.csv, which lists the drugs identified for the specific patient together with the regulatory action they exert on their associated targets.

All patient‑specific output files generated by the scripts are stored and can be inspected within the dedicated subdirectories located in the Patient Records folder of the repository.
