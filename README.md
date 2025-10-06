# Stanford-RNA-3D-folding
# 🧬 Stanford RNA 3D Folding
**Solve RNA structure prediction, one of biology’s remaining grand challenges**  

---

## 🧠 About this Competition
In this competition you will predict five 3D structures for each RNA sequence.

---

## 🏁 Competition Phases and Updates
This is a code competition that will proceed in three phases.

1. **Initial model training phase.**  
   At launch, there were approximately 25 sequences in the hidden test set. Some of those sequences were used for a private leaderboard to allow the host to track progress on wholly unseen data. During this phase the public test set sequences included—but was not limited to—targets from the [2024 CASP16 competition](https://predictioncenter.org/casp16/index.cgi) whose structures have not yet been publicly released in the [PDB database](https://www.rcsb.org/).

2. **Model training phase 2.**  
   On April 23, 2025, we updated the hidden test set and reset the leaderboard. Sequences in the current public test set were added to the train data, sequences currently in the private set were rolled into the new public set, and new sequences were added to the public test set.

3. **Future data phase.**  
   Your selected submissions will be run against a completely new private test set generated after the end of the model training phases. There will be up to 40 sequences in the test set, all of them used for the private leaderboard.

---

## 📁 Files

### [`train/validation/test]_sequences.csv`
The target sequences of the RNA molecules.

| Field | Type | Description |
|-------|------|-------------|
| `target_id` | string | An arbitrary identifier. In `train_sequences.csv`, this is formatted as `pdb_id_chain_id`, where `pdb_id` is the id of the entry in the [Protein Data Bank](https://www.rcsb.org/) and `chain_id` is the chain id of the monomer in the pdb file. |
| `sequence` | string | The RNA sequence. For `test_sequences.csv`, guaranteed to be a string of A, C, G, and U. For some `train_sequences.csv`, other characters may appear. |
| `temporal_cutoff` | string | The date in `yyyy-mm-dd` format that the sequence was published. |
| `description` | string | Details of the origins of the sequence. For a few targets, additional information on small molecule ligands bound to the RNA is included. |
| `all_sequences` | string | [FASTA-formatted](https://en.wikipedia.org/wiki/FASTA_format) sequences of all molecular chains present in the experimentally solved structure. |

---

### [`train/validation]_labels.csv`
Experimental structures.

| Field | Type | Description |
|-------|------|-------------|
| `ID` | string | Identifies the `target_id` and residue number, separated by `_`. (Residue numbers use one-based indexing.) |
| `resname` | character | The RNA nucleotide (`A`, `C`, `G`, or `U`) for the residue. |
| `resid` | integer | Residue number. |
| `x_1, y_1, z_1, x_2, y_2, z_2, …` | float | Coordinates (in Ångstroms) of the C1′ atom for each experimental RNA structure. There is typically one structure per RNA sequence. |

Note: `train_labels.csv` curates one structure for each training sequence, while `validation_labels.csv` has examples of targets with multiple reference structures (`x_2, y_2, z_2`, etc.).

---

### `train_sequences/labels_v2.csv`
Extracted from the protein data bank with full-text search for keyword RNA, relaxed filter for unstructured RNAs based on pairwise C1′ distances where 20% of residues have to be close to some other residue that is over 4 bases apart.

---

### `sample_submission.csv`
Same format as `train_labels.csv` but with **five** sets of coordinates for each of your predicted structures (`x_1, y_1, z_1, … x_5, y_5, z_5`).  
➡️ You **must** submit five sets of coordinates.

---

### `MSA/` and `MSA_v2/`
Contain multiple sequence alignments in FASTA format for each target in `train_sequences.csv` and `train_sequences.v2.csv`.  
Files are named `{target_id}.MSA.fasta`.

---

### `PDB_RNA/`
Contains 3D structural information available in the Protein DataBank.

| File | Description |
|-------|-------------|
| `{PDB_id}.cif` | Files for each RNA-containing entry. |
| `pdb_seqres_NA.fasta` | Sequences of all nucleic acid chains in the PDB in FASTA format. |
| `pdb_release_dates_NA.csv` | Entry ID and release dates of the RNA-containing PDB entries in CSV format. |

---

## 🧩 Additional Notes

- The `validation_sequences.csv` and `test_sequences.csv` publicly provided here comprise 12 targets from the **2022 CASP15 competition**, which have been a widely used test set in the RNA modeling field.  
- When using these for validation, ensure your `train_sequences.csv` have `temporal_cutoff` dates **before** `2022-05-27`.  
- You can use `train_sequences.csv` with `temporal_cutoff` **after** this date as an additional validation set.  
- Once you begin hill climbing on the competition’s **Public Leaderboard**, you may use all data in `train_sequences.csv` and structural info from the [PDB database](https://www.rcsb.org/).  
- RNA chains from the same or different PDB entries that share sequences are given as different entries in `train_sequences.csv`. You may consider deduplicating and merging.  
- If using **RibonanzaNet** (as in the competition’s starter notebook), it does not use information from the PDB before CASP15 and is valid for all test sets.  
- If using large language models, ensure any information used predates the `temporal_cutoff` for each target.  
  - Example: CASP16 competition ended **2024-09-18**, so any information released after this date should be excluded.

---

## 📦 Additional Files

- The developers of **RFdiffusion** have made available a synthetic dataset of over **400,000 RNA structures** [here](https://www.rfdiffusion.org/).

---

