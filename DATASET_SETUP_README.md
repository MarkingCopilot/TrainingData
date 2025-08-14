# Math Error Detection Datasets - Setup Guide

This repository provides step-by-step instructions for downloading and setting up three key datasets for math error detection research.

## Quick Overview

| Dataset | Size | Language | Content | Use Case |
|---------|------|----------|---------|----------|
| **Math Misconceptions** | 220 | English | Algebra misconceptions | Error detection testing |
| **FERMAT** | 2,244 | English | Handwritten math + images | OCR + error detection |
| **EGE Math** | 122 | Russian | EGE exam solutions + 3 image types | Cross-language detection |

## Getting Started

### Prerequisites

```bash
pip install datasets requests pandas numpy matplotlib
```

### Quick Setup

1. **Download the notebook**: `dataset_download_guide.ipynb`
2. **Run the notebook**: Opens in Jupyter Lab/Notebook
3. **Follow the step-by-step instructions** for each dataset

## Dataset Details

### 1. Math Misconceptions Dataset

- **Source**: [MarkingCopilot Research Benchmarks](https://github.com/MarkingCopilot/markingResearch/blob/main/docs/benchmarks.md)
- **Size**: 220 examples
- **Format**: JSON with structured Q&A pairs
- **Content**: Common algebra misconceptions with incorrect/correct answer pairs
- **Use Cases**: 
  - Testing error detection algorithms
  - Understanding common student mistakes
  - Training misconception identification models

**Example Structure**:
```json
{
  "Question": "Solve for x: 2x + 3 = 7",
  "Incorrect Answer": "x = 5",
  "Correct Answer": "x = 2", 
  "Misconception": "Adding instead of subtracting when isolating variable",
  "Topic": "Linear equations"
}
```

### 2. FERMAT Dataset

- **Source**: [HuggingFace - ai4bharat/fermat](https://huggingface.co/datasets/ai4bharat/fermat)
- **Paper**: "Can Vision-Language Models Evaluate Handwritten Math?" (arXiv:2501.07244)
- **Size**: 2,244 handwritten math problems
- **Format**: JSON + PNG images
- **Content**: Handwritten math solutions with error annotations
- **Use Cases**:
  - Testing OCR accuracy on handwritten math
  - Evaluating VLM error detection capabilities
  - Training handwritten math recognition models

**Example Structure**:
```json
{
  "problem": "Solve the equation: x² - 5x + 6 = 0",
  "correct_answer": "x = 2, x = 3",
  "student_answer": "x = 2, x = 4", 
  "has_error": true,
  "error_reasoning": "Incorrect factorization",
  "domain": "Algebra",
  "grade": "c09",
  "image_filename": "fermat_0001.png"
}
```

### 3. EGE Math Solutions Assessment Benchmark

- **Source**: [HuggingFace - Karifannaa/EGE_Math_Solutions_Assessment_Benchmark](https://huggingface.co/datasets/Karifannaa/EGE_Math_Solutions_Assessment_Benchmark)
- **Paper**: "EGE Math Solutions Assessment Benchmark" (arXiv:2507.22958)
- **Size**: 122 examples
- **Language**: Russian
- **Format**: JSON + 3 types of PNG images
- **Content**: Russian EGE exam solutions with quality assessments
- **Use Cases**:
  - Russian language math OCR testing
  - Cross-language error detection
  - Comparing student vs correct solutions

**Three Image Types**:
1. **`images_with_answer`**: Student handwritten solutions
2. **`images_without_answer`**: Problem statements only
3. **`images_with_true_solution`**: Correct reference solutions

**Example Structure**:
```json
{
  "solution_id": "13.1.1",
  "task_type": "Trigonometric equations",
  "score": 2,
  "image_counts": {
    "images_with_answer": 1,
    "images_without_answer": 1, 
    "images_with_true_solution": 1
  }
}
```

## Output Directory Structure

After running the setup notebook, you'll have:

```
math_datasets/
├── processed/                    # Clean, ready-to-use datasets
│   ├── math_misconceptions/
│   │   ├── math_misconceptions.json
│   │   └── dataset_info.json
│   ├── fermat/
│   │   ├── fermat_processed.json
│   │   ├── dataset_info.json
│   │   └── images/              # Handwritten math images
│   │       ├── fermat_0001.png
│   │       └── ...
│   └── ege_math/
│       ├── ege_processed.json
│       ├── complete_image_mapping.json
│       ├── dataset_info.json
│       └── images/
│           ├── student_answers/   # Student solutions
│           ├── problems/          # Problem statements  
│           └── correct_solutions/ # Reference solutions
├── raw/                         # Original downloaded data
└── scripts/                     # Processing utilities
```

## Customization Options

### FERMAT Dataset Size

The FERMAT dataset is large (2,244 examples). You can customize the download:

```python
# Small sample for testing
fermat_data = download_fermat_dataset(sample_size=100, save_images=True)

# Larger sample
fermat_data = download_fermat_dataset(sample_size=500, save_images=True)

# Full dataset (takes time and space)
fermat_data = download_fermat_dataset(sample_size=None, save_images=True)
```

### Image Saving

You can choose whether to save images:

```python
# Save images (recommended for OCR testing)
fermat_data = download_fermat_dataset(save_images=True)

# Skip images (faster, smaller disk usage)
fermat_data = download_fermat_dataset(save_images=False)
```

## Usage Examples

### Loading Datasets

```python
import json

# Load math misconceptions
with open('math_datasets/processed/math_misconceptions/math_misconceptions.json', 'r') as f:
    misconceptions = json.load(f)

# Load FERMAT data
with open('math_datasets/processed/fermat/fermat_processed.json', 'r') as f:
    fermat_data = json.load(f)

# Load EGE data
with open('math_datasets/processed/ege_math/ege_processed.json', 'r') as f:
    ege_data = json.load(f)
```

### Using with Error Detection Tools

```python
# Example: Test error detection on misconceptions
for example in misconceptions[:5]:
    problem = example['Question']
    student_answer = example['Incorrect Answer']
    correct_answer = example['Correct Answer']
    
    # Your error detection code here
    detected_errors = your_error_detector.analyze(problem, student_answer)
    print(f"Known misconception: {example['Misconception']}")
    print(f"Detected errors: {detected_errors}")
```

### OCR Testing with Images

```python
# Example: Test OCR on FERMAT images
import os
from PIL import Image

fermat_images_dir = "math_datasets/processed/fermat/images"
for image_file in os.listdir(fermat_images_dir):
    if image_file.endswith('.png'):
        image_path = os.path.join(fermat_images_dir, image_file)
        
        # Your OCR code here
        extracted_text = your_ocr_system.extract(image_path)
        print(f"Image: {image_file}")
        print(f"Extracted: {extracted_text}")
```

## Contributing

### To MarkingCopilot/trainingData Repository

1. Fork the repository
2. Add the `dataset_download_guide.ipynb` notebook
3. Add this README as `README.md`
4. Submit a pull request

### Improvements Welcome

- Additional dataset formats
- Better processing scripts
- Error detection benchmarks
- Documentation improvements

## Dataset Statistics

| Metric | Math Misconceptions | FERMAT | EGE Math |
|--------|-------------------|---------|----------|
| **Total Examples** | 220 | 2,244 | 122 |
| **Language** | English | English | Russian |
| **Has Images** | No | Yes (2,244) | Yes (448) |
| **Error Annotations** | Yes | Yes | Yes (scores) |
| **Topics** | Algebra | Mixed | EGE exam topics |
| **Grade Levels** | Middle/High | 7th-12th | High school |

## Quality Checks

Each dataset includes:
- **Data validation**: JSON structure verified
- **Image verification**: All images saved and accessible
- **Metadata consistency**: Complete dataset_info.json files
- **Format standardization**: Consistent field naming

## Research Applications

These datasets enable research in:
- **Automated math grading**
- **Error detection algorithms**
- **OCR for handwritten mathematics**
- **Cross-language error analysis**
- **Educational technology development**

## Troubleshooting

### Common Issues

1. **HuggingFace download errors**: Check internet connection and try again
2. **Memory issues with large datasets**: Use smaller sample sizes first
3. **Image saving failures**: Ensure sufficient disk space
4. **JSON encoding issues**: All files use UTF-8 encoding

### Getting Help

- Open an issue in the [MarkingCopilot/trainingData](https://github.com/MarkingCopilot/trainingData) repository
- Check the individual dataset sources for specific issues
- Review the notebook cell outputs for detailed error messages

## License Information

- **Math Misconceptions**: Check source repository for license
- **FERMAT**: CC-BY-4.0
- **EGE Math**: Check HuggingFace dataset page for license

## Useful Links

- [MarkingCopilot Research](https://github.com/MarkingCopilot/markingResearch)
- [FERMAT Paper](https://arxiv.org/abs/2501.07244)
- [EGE Math Paper](https://arxiv.org/abs/2507.22958)
- [HuggingFace Datasets Documentation](https://huggingface.co/docs/datasets/)

---

*This guide was created to facilitate math error detection research. For questions or contributions, please open an issue in the repository.*