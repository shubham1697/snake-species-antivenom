# SNAKEID - AI Powered Snake Species Identification for Antivenom Selection

## 🎯 Objective
SNAKEID is a deep learning-based snake species classification system designed to identify snake species from photographs and recommend the correct antivenom. The system addresses the critical need for rapid species identification during the "golden hour" after snakebite — where correct antivenom administration saves lives.

---

## 🧠 Core Concept
India records ~58,000 snakebite deaths per year (WHO). Misidentification of snake species leads to incorrect antivenom administration, which can be fatal. While herpetology expertise is scarce in rural areas, smartphone cameras are ubiquitous. SNAKEID bridges this gap using CNN-based image classification to provide species identification and antivenom recommendations — even without expert human intervention.

---

## 🚀 Features
- Multi-model comparison: Baseline ML → CNN from scratch → Transfer Learning
- Real-time species identification from a single photograph
- Antivenom recommendation engine with urgency levels
- Grad-CAM visualization for model interpretability
- Confidence-aware predictions with low-confidence warnings
- Modular pipeline suitable for mobile deployment

---

## 🏗️ Architecture

SNAKEID evaluates and compares the following approaches:
- **Random Forest + GridSearchCV (Baseline)**: Traditional ML on flattened pixel features
- **CNN from Scratch**: 3 convolutional blocks trained end-to-end
- **MobileNetV2 Transfer Learning**: Frozen ImageNet backbone + custom classification head

### 🔍 Transfer Learning Approach
```
Input (224×224×3) → Data Augmentation → MobileNetV2 [FROZEN] → GlobalAvgPool
→ Dense(128, relu) → Dropout(0.3) → Dense(10, softmax)
```

MobileNetV2 (pre-trained on 1.4M ImageNet images) serves as a frozen feature extractor. Only the classification head is trained on snake images — proving that pre-trained visual embeddings transfer effectively to fine-grained species classification.

### 🔍 CNN from Scratch
```
Input (128×128×3) → Augmentation → Conv2D(32) → MaxPool → Conv2D(64) → MaxPool
→ Conv2D(128) → MaxPool → Flatten → Dense(256) → Dropout(0.5) → Dense(10, softmax)
```

---

## 🧪 Methodology
- **Dataset**:
  - Kaggle Snake Species Dataset (135 species, ~23,800 images)
  - Top 10 species selected by image count (~200 images/class)
  - Metadata: species binomial, family, venomous status, GPS coordinates
  - Train/Val split: 80/20 (stratified)
- **Augmentation**:
  - Random horizontal flip, rotation (±10°), zoom (±10%), contrast adjustment
- **Training**:
  - Optimizer: Adam | Loss: Sparse Categorical Crossentropy
  - EarlyStopping (patience=5) + ReduceLROnPlateau (factor=0.5, patience=3)
  - Batch size: 32 | Max epochs: 50

---

## 📊 Performance Highlights
| Model | Accuracy | vs Baseline | Key Insight |
|-------|----------|-------------|-------------|
| Random Forest + GridSearchCV | 26.9% | — | Traditional ML struggles with raw pixels |
| CNN from Scratch (3 blocks) | 44.0% | +17.2% | Learns spatial features but limited by data |
| Transfer Learning (MobileNetV2) | **71.4%** | **+44.5%** | Pre-trained embeddings dramatically improve performance |

---

## 🐍 Antivenom Recommendation System
The end-to-end pipeline provides:
- **Top-3 species predictions** with confidence percentages
- **Antivenom mapping** (e.g., Polyvalent ASV, CroFab, Antivipmyn-Tri)
- **Urgency classification**: CRITICAL / HIGH / LOW
- **Safety warning** for low-confidence predictions (< 50%)

```
============================================================
SNAKE SPECIES IDENTIFICATION & ANTIVENOM RECOMMENDATION
============================================================
#1 Prediction: Agkistrodon piscivorus (Confidence: 73.2%)
   Antivenom: CroFab (Crotalidae)
   Urgency:   HIGH
============================================================
```

---

## 🔥 Novelty: Grad-CAM Visualization
Gradient-weighted Class Activation Mapping (Grad-CAM) provides interpretability by highlighting which image regions the model focuses on for its decision. This is critical for:
- **Clinical trust** — doctors need to understand why the AI made a recommendation
- **Validation** — confirms model focuses on snake body features (not background)
- **Failure analysis** — identifies when model is confused by habitat/lighting

---

## 📦 Output Examples
- Species classification with confidence scores
- Antivenom recommendations with urgency levels
- Confusion matrix showing per-class performance
- Training/validation loss and accuracy curves
- Grad-CAM heatmaps overlaid on snake images
- Per-class recall analysis (critical for venomous species)

---

## 📈 Metrics & Evaluation
- **Primary metric**: Accuracy (overall model comparison)
- **Critical metric**: Per-class Recall (especially for venomous species — false negatives are dangerous)
- **Visualization**: Confusion Matrix, Training Curves, Grad-CAM
- **Comparison**: Side-by-side training curves for CNN vs Transfer Learning

---

## 🔮 Future Scope
- Fine-tune top layers of MobileNetV2 for further accuracy gains
- Expand to all 135 species with additional training data
- Mobile deployment via TensorFlow Lite (field use in rural areas)
- GPS-based species narrowing using regional priors
- Integration with hospital antivenom inventory systems
- Multi-image input (different angles) for higher confidence
- Active learning: flag uncertain predictions for expert review

---

## 🛠️ Tech Stack
- Python 3.12
- TensorFlow / Keras 3.x
- scikit-learn (Baseline + GridSearchCV)
- NumPy, Pandas, Matplotlib, Seaborn
- Grad-CAM (custom implementation)

---

## 📁 Project Structure
```
snake-species-antivenom/
├── snake_classifier.ipynb          # Main notebook (full pipeline)
├── snake_classifier_v2.ipynb       # Experimental (merged dataset)
├── snake_classifier_transfer_learning.ipynb  # Standalone TL notebook
├── Snake_Species_Capstone_Presentation.pptx  # Presentation
├── PROJECT_PLAN.md                 # Project planning document
├── README.md                       # This file
└── saved_model/
    ├── snake_cnn_model.keras       # Trained CNN model
    └── classes.npy                 # Label encoder classes
```

---

## 🙏 Acknowledgements
- Kaggle for the Snake Species Dataset
- Google for MobileNetV2 pre-trained weights
- TensorFlow/Keras team for the deep learning framework
- IIT Roorkee for the AI/ML certification program

---

**Every second counts after a snakebite — SNAKEID makes identification intelligent.**
