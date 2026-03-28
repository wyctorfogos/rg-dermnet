# RG-DermNet

Repositório focado no pipeline experimental para artigo científico:

- treino multimodal em `src/scripts/benchmark`
- agregação de métricas e testes estatísticos em `src/scripts/aggreation`

## 1. Ambiente

```bash
conda create -n rg-dermnet python=3.10 -y
conda activate rg-dermnet
pip install -r requirements.txt
```

## 2. Configuração (`conf/.env`)

O treino lê estas variáveis de `conf/.env`:

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

Estrutura esperada do dataset:

```text
<DATASET_FOLDER_PATH>/
|-- images/
`-- metadata.csv
```

## 3. Treino

Exemplos de execução:

```bash
python src/scripts/benchmark/train_pad_20.py
python src/scripts/benchmark/train_pad_25.py
python src/scripts/benchmark/train_isic_2019.py
python src/scripts/benchmark/train_isic_2020.py
```

As métricas por fold são salvas no caminho configurado em `RESULTS_FOLDER_PATH`.

## 4. Agregação e estatística

Depois dos treinos:

```bash
python src/scripts/aggreation/average_metric_values.py
python src/scripts/aggreation/statistical_test.py
```

Arquivos principais:

- `src/scripts/aggreation/average_metric_values.py`
- `src/scripts/aggreation/statistical_test.py`
- `src/scripts/aggreation/stats.py`

## 5. Escopo mínimo do projeto

Para reprodução do artigo, o núcleo é:

- `conf/`
- `requirements.txt`
- `src/scripts/benchmark/`
- `src/scripts/aggreation/`
- `data/` (local, não versionado)

## Citation (IJCNN 2026)

If you use this project, please cite:

Rocha, W. F., Bouzon, P. H. G., Ramos, L. A., Pacheco, A. G. C., and Souza Jr., L. A.  
**RG-DermNet: A Multimodal Attention-Based Model with Residual Block Usage for Skin Lesion Classification.**  
Accepted at the International Joint Conference on Neural Networks (IJCNN), 2026.

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
