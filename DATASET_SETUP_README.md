# Math Error Detection Datasets - Setup Guide

## Installation

### Prerequisites

```bash
pip install datasets requests pandas numpy matplotlib
```

### Setup

1. Download `dataset_download_guide.ipynb`
2. Run the notebook in Jupyter
3. Follow step-by-step instructions for each dataset

## Datasets

### Math Misconceptions (MaE)
- **Size**: 220 examples, 55 algebra misconceptions
- **Language**: English  
- **Content**: Common student mistakes in middle school algebra
- **Source**: [GitHub](https://github.com/nancyotero-projects/math-misconceptions)

### FERMAT
- **Size**: 2,244 handwritten math problems + images
- **Language**: English
- **Content**: Handwritten math solutions with error annotations
- **Source**: [HuggingFace](https://huggingface.co/datasets/ai4bharat/fermat)

### EGE Math
- **Size**: 122 examples + 448 images (3 types)
- **Language**: Russian
- **Content**: Russian EGE exam solutions with quality scores
- **Source**: [HuggingFace](https://huggingface.co/datasets/Karifannaa/EGE_Math_Solutions_Assessment_Benchmark)

## Output Structure

```
math_datasets/
└── processed/
    ├── math_misconceptions/
    ├── fermat/
    └── ege_math/
```