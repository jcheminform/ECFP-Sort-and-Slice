# Sort & Slice: A Simple and Superior Alternative to Hash-Based Folding for Extended-Connectivity Fingerprints

Code repository for the paper Sort & Slice: A Simple and Superior Alternative to Hash-Based Folding for Extended-Connectivity Fingerprints: 

https://arxiv.org/abs/2403.17954

This repository contains:

* A simple, self-contained and fast [function](sort_and_slice_ecfp_featuriser.py) to transform RDKit mol objects into vectorial extended-connectivity fingerprints (ECFPs) pooled via Sort & Slice.
* The code base and data sets to fully reproduce the computational results from the paper.
* Original numerical results from the experiments conducted in the paper.




  
## Easily Generating Vectorial ECFPs via Sort & Slice from RDKit mol Objects

The function in [sort_and_slice_ecfp_featuriser.py](sort_and_slice_ecfp_featuriser.py) represents a computationally efficient, easy-to-use and self-contained method to create a featuriser that can transform RDKit mol objects into vectorial ECFPs pooled via Sort & Slice (rather than via classical hash-based folding). It only relies on RDKit and NumPy and can be readily employed for molecular feature extraction and other ECFP-based applications. This function should be all you need in case you want to use vectorial Sort & Slice ECFPs in your own project. Our numerical observations suggest that ECFPs vectorised via Sort & Slice regularly (and sometimes substantially) outperform ECFPs vectorised via classical hash-based folding across a wide variety of molecular property prediction scenarios.

EXAMPLE:
    
First construct a molecular featurisation function with desired settings, for instance via
    
    ecfp_featuriser = construct_sort_and_slice_ecfp_featuriser(mols_train = [mol_1, mol_2, ...], 
                                                               max_radius = 2, 
                                                               pharm_atom_invs = False, 
                                                               bond_invs = True, 
                                                               chirality = False, 
                                                               sub_counts = True, 
                                                               vec_dimension = 1024, 
                                                               break_ties_with = lambda sub_id: sub_id, 
                                                               print_train_set_info = True)
                                                               
    Note that the ECFP settings (max_radius, pharm_atom_invs, bond_invs, chirality, sub_counts, vec_dimension, break_ties_with) as well as chemical information from mols_train are all by construction implicitly transferred to "ecfp_featuriser".
    Now let mol be an RDKit mol object. Then ecfp_featuriser(mol) is a 1-dimensional numpy array of length vec_dimension representing the vectorial Sort & Slice ECFP fingerprint for mol.
    The function ecfp_featuriser(mol) works by (i) first generating the (multi)set of integer ECFP-substructure identifiers for mol and then (ii) vectorising it via a Sort & Slice operator trained on mols_train (rather than via classical hash-based folding).








## Data Sets

The data-folder contains three clean chemical data sets of small-molecule inhibitors of dopamine receptor D2, factor Xa, or SARS-CoV-2 main protease respectively. Each data set is represented by two files: molecule_data_clean.csv and MMP_data_clean.csv. The first file contains SMILES strings with associated activity values and the second file contains all matched molecular pairs (MMPs) identified within the first file.

## Reproducing the Experiments

The experiments in the paper can be reproduced by running the code in the Jupyter notebook QSAR_activity_cliff_experiments.ipynb. First, the QSAR-, AC-, and PD-prediction tasks for the chosen data set are formally constructed in a data-preparation section. Then, an appropriate data split is conducted, both at the level of individual molecules and MMPs. Finally, a molecular representation (PDV, ECFP, or GIN) and a regression technique (RF, kNN, MLP) are chosen and the resulting model is trained and evaluated for QSAR-prediction, AC-classification and PD-classification. The computational environment in which the original results were conducted can be found in environment.yml.

![Substructure Pooling Overview](/figures/sub_pool_methods_overview.png)

## Visually Investigating the Results:

The experimental results can be visually explored using the visualise_results-function at the end of QSAR_activity_cliff_experiments.ipynb. This function produces scatterplots such as the one in the graphical abstract above. The original numerical results from the paper are saved in the resuls-folder; thus the original plots from the paper (and more) can be generated with visualise_results.
