🟢 ACTIVE

### Dataset description

The original sequence from the aav study is UniProt [P03135](https://www.uniprot.org/uniprot/P03135). A copy of the wildtype sequence can be found in this folder as P03135.fasta
On the reference sequence, mutations where introduced starting from region `[561, 588]`, which reflects the AA sequence: `DEEEIRTTNPVATEQYGSVSTNLQRGNR`.

All tasks come in two modalities: `regression` and `binary`. The latter is obtained by applying a cutoff on the regression task.


### Full dataset

The full dataset can be found in the zipped CSV `full_data.csv.zip`. All "tasks" are derivatives of this CSV. The columns in the CSV are:

- `mutation_mask`: the mutation mask applied to the reference region. small chars mean insertions, `*` means delition, capital letters mean mutations, `_` means no change
- `mutated_region`: the AA sequence of the mutated region. Can contain special characters, like `*`, reflecting the `mutation_mask`
- `category`: the category of the sequence, e.g. designed or not
- `number_of_mutations`: the number of mutations according to the original dataset
- `levenshtein_distance`: the Levenshtein distance between the `mutated_region` and the reference sequence
- `score`: the `GAS1_virus_S` score from the original dataset, here selected as the target score. Note that the `GAS1_virus_S` has been averaged over all synonymous variants.
- `full_aa_sequence`: the sequence from [P03135](https://www.uniprot.org/uniprot/P03135) with the region `[561, 588]` replaced by the `mutated_region`
- `binary_score`: a binarization of the `score`, according to a Gaussian mixture model, as done in the original publication.
- `reference_region`: the reference region, as above.
- Additional columns to create tasks (see following)

### Tasks

- `design_task`: `train` if in `category` in `'previous_chip_viable', 'previous_chip_nonviable', 'stop', 'singles', 'wild_type', 'random_doubles'`, `test` otherwise
  ```
  Set design_task has 28102 train and 186075 test sequences. 55% of test is positive.
  ```
- `design_task_reversed`: `train` and `test` from `design_task` are reversed
  ```
  Set design_task_reversed has 186075 train and 28102 test sequences. 12% of test is positive.
  ```
- `natural_task_1`: `train` for 80% of data in `category` in `'previous_chip_viable', 'previous_chip_nonviable', 'stop', 'singles', 'wild_type', 'random_doubles'`, `test` for 20% of data, `unknown` otherwise
  ```
  Set natural_task_1 has 22350 train and 5752 test sequences. 12% of test is positive.
  ```
- `natural_task_2`: `train` for data in `category` in `'previous_chip_viable', 'previous_chip_nonviable', 'stop', 'singles', 'wild_type', 'random_doubles'` with `number_of_mutations <= 7` , `test` for `number_of_mutations > 7`, `unknown` otherwise
  ```
  Set natural_task_2 has 27661 train and 441 test sequences. 13% of test is positive.
  ```

All tasks are contained in the `tasks.zip` file. These are CSV with colums:

- `sequence`: the AA sequence. May contain special characters!
- `target`: the prediction target, which is float value (the task is a regression).
- `set`: either `train` or `test`, if the sequence should be used for training or testing your model!

### Cite
From the publisher:
> Bryant, D.H., Bashir, A., Sinai, S. et al. Deep diversification of an AAV capsid protein by machine learning. Nat Biotechnol 39, 691–696 (2021). https://doi.org/10.1038/s41587-020-00793-4

Bibtex:
```
@article{bryant2021deep,
  title={Deep diversification of an AAV capsid protein by machine learning},
  author={Bryant, Drew H and Bashir, Ali and Sinai, Sam and Jain, Nina K and Ogden, Pierce J and Riley, Patrick F and Church, George M and Colwell, Lucy J and Kelsic, Eric D},
  journal={Nature Biotechnology},
  volume={39},
  number={6},
  pages={691--696},
  year={2021},
  publisher={Nature Publishing Group}
}
```

### Data licensing

The RWA data was downloaded from [GitHub](https://github.com/churchlab/Deep_diversification_AAV/tree/main/Data) which doesn't feature a license.
Modified data available in this repository and in the `tasks` falls under [AFL-3](https://opensource.org/licenses/AFL-3.0).