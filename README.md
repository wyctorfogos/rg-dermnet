# RG-DermNet

This repository is focused on the experimental pipeline used in the scientific paper:

- multimodal training scripts in `src/scripts/benchmark`
- metric aggregation and statistical analysis in `src/scripts/aggreation`

## 1. Environment setup

```bash
conda create -n rg-dermnet python=3.10 -y
conda activate rg-dermnet
pip install -r requirements.txt
```

## 2. Configuration (`conf/.env`)

Training scripts read the following variables from `conf/.env`:

- `NUM_EPOCHS`
- `BATCH_SIZE`
- `K_FOLDS`
- `LIST_NUM_HEADS`
- `COMMON_DIM`
- `DATASET_FOLDER_NAME`
- `DATASET_FOLDER_PATH`
- `RESULTS_FOLDER_PATH`
- `NUMBER_OF_WORKERS`
- `UNFREEZE_WEIGHTS`
- `LLM_MODEL_NAME_SEQUENCE_GENERATOR`
- `save_to_disk`

Expected dataset structure:

```text
<DATASET_FOLDER_PATH>/
|-- images/
`-- metadata.csv
```

## 3. Training

Example runs:

```bash
python src/scripts/benchmark/train_pad_20.py
python src/scripts/benchmark/train_pad_25.py
python src/scripts/benchmark/train_isic_2019.py
python src/scripts/benchmark/train_isic_2020.py
```

Metrics per fold are saved under `RESULTS_FOLDER_PATH`.

## 4. Aggregation and statistics

After training:

```bash
python src/scripts/aggreation/average_metric_values.py
python src/scripts/aggreation/statistical_test.py
```

Main files:

- `src/scripts/aggreation/average_metric_values.py`
- `src/scripts/aggreation/statistical_test.py`
- `src/scripts/aggreation/stats.py`

## 5. Minimum project scope

For paper reproducibility, the core is:

- `conf/`
- `requirements.txt`
- `src/scripts/benchmark/`
- `src/scripts/aggreation/`
- `data/` (local, not versioned)


## Implementation note on the fusion configuration

The configuration used in the experiments is:

`att-intramodal+residual+cross-attention-metadados`

In this implementation, image features and clinical metadata are first projected into a shared latent space. Then, each modality is independently refined through an intramodal residual-gated attention block. After this refinement, multimodal interaction is performed through bidirectional cross-attention: the visual branch attends to the refined metadata representation, while the metadata branch attends to the refined visual representation. The resulting representations are concatenated and passed to the final classification head.

Therefore, in the released code, the residual-gated attention stage acts as an intramodal refinement step before fusion, while the cross-modal interaction is performed by the subsequent bidirectional cross-attention layers.

## Citation (IJCNN 2026)

If you use this project, please cite:

Rocha, W. F., Bouzon, P. H. G., Ramos, L. A., Pacheco, A. G. C., and Souza Jr., L. A.  
**RG-DermNet: A Multimodal Attention-Based Model with Residual Block Usage for Skin Lesion Classification.**  
Submitted for publication at the International Joint Conference on Neural Networks (IJCNN), 2026.

### Authors and affiliations

- Wyctor F. da Rocha, Pedro H. G. Bouzon, Andre G. C. Pacheco, Luis A. Souza Jr.  
Graduate Program of Informatics, Federal University of Espirito Santo, Vitoria, Brazil
- Lucas A. Ramos  
Computer Vision and Data Science, NHL Stenden University of Applied Sciences, Leeuwarden, The Netherlands

### BibTeX

```bibtex
@inproceedings{rocha2026rgdermnet,
  title     = {RG-DermNet: A Multimodal Attention-Based Model with Residual Block Usage for Skin Lesion Classification},
  author    = {Rocha, Wyctor F. and Bouzon, Pedro H. G. and Ramos, Lucas A. and Pacheco, Andre G. C. and Souza Jr., Luis A.},
  booktitle = {International Joint Conference on Neural Networks (IJCNN)},
  year      = {2026},
  note      = {Accepted}
}
```

## Questions and support

For further questions about the lab and related projects, please visit:

https://life.inf.ufes.br/
