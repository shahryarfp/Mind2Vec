# Mind2vec

**Mind2vec** contains Kaggle-ready code for semantic decoding of imagined-speech EEG from the **CHISCo (Chinese Imagined Speech Corpus)** dataset.

The main approach aligns EEG representations with a frozen language-embedding space using contrastive learning. The repository also includes a conventional closed-set baseline, text-encoder benchmarking, and result-visualization code.

## Repository

```text
Mind2vec/
├── contrastive_eeg_text_alignment.ipynb
├── closed_set_baseline.ipynb
├── text_encoder_benchmark.ipynb
├── results_visualization.ipynb
└── README.md
```

- **`contrastive_eeg_text_alignment.ipynb`** — main EEG–text alignment model and evaluation.
- **`closed_set_baseline.ipynb`** — conventional 39-class EEG decoding baseline.
- **`text_encoder_benchmark.ipynb`** — comparison of candidate language embedding models.
- **`results_visualization.ipynb`** — generation of the main result figures.

## Dataset

The code uses the imagined-speech EEG data from the **CHISCo dataset**. The five subject-wise Kaggle datasets below were downloaded directly from the official CHISCo release on OpenNeuro:

**Original dataset:**  
https://openneuro.org/datasets/ds005170/versions/1.1.2

**Kaggle datasets:**

- [Subject 01](https://www.kaggle.com/datasets/shahryarnamdari/chisco-is-sub01)
- [Subject 02](https://www.kaggle.com/datasets/shahryarnamdari/chisco-is-sub02)
- [Subject 03](https://www.kaggle.com/datasets/shahryarnamdari/chisco-is-sub03)
- [Subject 04](https://www.kaggle.com/datasets/shahryarnamdari/chisco-is-sub04)
- [Subject 05](https://www.kaggle.com/datasets/shahryarnamdari/chisco-is-sub05)

## Running the code

The notebooks are designed to run on **Kaggle**.

1. Open the desired notebook on Kaggle.
2. Add the corresponding subject dataset as an input.
3. Enable a GPU for the EEG training notebooks.
4. Select the desired subject in the notebook configuration.
5. Run the notebook from top to bottom.

Some notebooks download pretrained language models, so Internet access may be required.

## Paper

This repository contains the code accompanying:

**Semantic Alignment of EEG and Text: A Contrastive Learning Framework for Decoding Imagined Speech**

Citation and publication information will be added when available.
