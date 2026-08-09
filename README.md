# Mind2vec

**Semantic alignment of imagined-speech EEG with language embeddings.**

Mind2vec contains Kaggle-ready notebooks for decoding semantic intent from imagined-speech EEG in the **Chinese Imagined Speech Corpus (CHISCo)**. The main framework maps EEG trials into a frozen language-embedding space, allowing conventional category decoding while also supporting similarity-based retrieval, reliability analysis, and semantic probing.

The repository also includes a task-optimized closed-set baseline, a text-encoder selection benchmark, and code for reproducing the main result figures.

## Repository structure

```text
Mind2vec/
├── contrastive_eeg_text_alignment.ipynb
├── closed_set_baseline.ipynb
├── text_encoder_benchmark.ipynb
├── results_visualization.ipynb
└── README.md
```

### `contrastive_eeg_text_alignment.ipynb`

Main Mind2vec model and evaluation pipeline.

It includes:
- CHISCo imagined-speech EEG loading and preprocessing
- sentence-grouped five-fold cross-validation
- a frozen `BAAI/bge-small-zh-v1.5` text encoder
- an EEGNet + Transformer EEG encoder with attention pooling and projection into a 512-dimensional language space
- semantically reweighted contrastive alignment combined with text-prototype-based class discrimination
- Top-k accuracy, Macro-F1, and balanced 2v2 evaluation
- MaxCosine reliability analysis
- Semantic Gallery Probe (SGP)
- evaluation-time generalization across semantic axes

### `closed_set_baseline.ipynb`

Conventional 39-class EEG decoding baseline used for comparison with Mind2vec.

The model uses an EEGNet-inspired convolutional backbone, positional encoding, a Transformer block, and a 39-way softmax classifier. It uses the same sentence-grouped cross-validation strategy and reports Top-k accuracy, Macro-F1, and balanced 2v2 accuracy.

### `text_encoder_benchmark.ipynb`

Benchmarks frozen Chinese and multilingual sentence-embedding models before EEG-language alignment.

The notebook evaluates:
- Logistic Regression
- MLP
- XGBoost
- zero-shot category-prototype matching

across six candidate text encoders. Only the CHISCo `textdataset/` files are needed for this notebook; EEG recordings are not loaded.

### `results_visualization.ipynb`

Generates the final subject-wise comparison figures from pre-computed results, including:
- Top-1 and Top-5 accuracy
- Macro-F1 and 2v2 accuracy
- semantic-axis generalization

The result values are stored directly in the notebook and can be replaced if new experiments are run.

---

## Dataset

The code is designed to run with the five subject-wise CHISCo datasets below:

| Subject | Kaggle dataset |
|---|---|
| Subject 01 | [CHISCo IS Sub01](https://www.kaggle.com/datasets/shahryarnamdari/chisco-is-sub01) |
| Subject 02 | [CHISCo IS Sub02](https://www.kaggle.com/datasets/shahryarnamdari/chisco-is-sub02) |
| Subject 03 | [CHISCo IS Sub03](https://www.kaggle.com/datasets/shahryarnamdari/chisco-is-sub03) |
| Subject 04 | [CHISCo IS Sub04](https://www.kaggle.com/datasets/shahryarnamdari/chisco-is-sub04) |
| Subject 05 | [CHISCo IS Sub05](https://www.kaggle.com/datasets/shahryarnamdari/chisco-is-sub05) |

The EEG notebooks expect a dataset containing approximately the following structure:

```text
subject_root/
├── textdataset/
│   └── *.xlsx
└── derivatives/
    └── preprocessed_pkl/
        └── *imagine*.pkl
```

Each usable imagined-speech sample is expected to contain the stimulus text and its EEG input features.

---

## Running on Kaggle

The notebooks were designed for **Kaggle Notebooks**.

1. Open or upload the notebook you want to run on Kaggle.
2. Add the corresponding subject dataset using **Add Input**.
3. For the EEG training notebooks, enable a **GPU accelerator**.
4. Enable Internet access when needed so pretrained Hugging Face text encoders can be downloaded.
5. In the configuration cell, set:

```python
SUBJECT_ID = "sub-01"  # sub-01, sub-02, sub-03, sub-04, or sub-05
```

6. Run the notebook from top to bottom.

The EEG notebooks already contain default Kaggle paths for all five subject datasets. If your mounted dataset path is different, change `DATA_ROOT` or set the corresponding environment variable in the configuration section.

### Text-encoder benchmark

`text_encoder_benchmark.ipynb` only requires a CHISCo dataset containing `textdataset/`. The default configuration uses Subject 01.

---

## Dependencies

Most scientific Python packages are already available in the Kaggle environment. Depending on the notebook, the additional packages include:

```text
sentence-transformers
xgboost
openpyxl
adjustText
deep-translator
tqdm
```

A convenient Kaggle installation cell is:

```python
!pip install -q sentence-transformers xgboost openpyxl adjustText deep-translator tqdm
```

The code also uses TensorFlow, NumPy, Pandas, SciPy, scikit-learn, Matplotlib, and PyTorch where required.

---

## Experimental setup

The EEG experiments are performed **within subject** using sentence-grouped five-fold cross-validation. Sentences are kept within a single split to prevent the same sentence from appearing in both training and held-out data. Within each outer fold, 15% of the training sentence groups are reserved for validation.

EEG trials are formatted to **122 channels × 1651 time samples** after removal of the final three non-EEG channels, trial-wise normalization, and temporal cropping/padding when necessary.

For the contrastive framework, EEG representations are aligned to frozen 512-dimensional embeddings from `BAAI/bge-small-zh-v1.5`.

---

## Main outputs

The contrastive notebook saves fold-level and summary CSV files together with generated figures under the Kaggle working directory, including decoding metrics, balanced 2v2 results, MaxCosine analyses, Semantic Gallery Probe outputs, and semantic-axis generalization results.

The closed-set notebook saves model checkpoints during training and PDF figures for its evaluation results. The text-encoder benchmark exports its fold-level and cross-validation summaries as CSV files and generates the language-model comparison figure.

`results_visualization.ipynb` writes the final figures to a local `figures/` directory.

---

## Associated paper

This repository contains the code accompanying:

**Semantic Alignment of EEG and Text: A Contrastive Learning Framework for Decoding Imagined Speech**

Citation information and a public paper link can be added here once available.

---

## Notes

- The five EEG subjects are trained and evaluated separately; the notebooks do not train a cross-subject model.
- The text encoder remains frozen during EEG training.
- The Semantic Gallery Probe is a qualitative analysis rather than a quantitative decoding metric.
- `results_visualization.ipynb` uses pre-computed values and does not retrain the models.
