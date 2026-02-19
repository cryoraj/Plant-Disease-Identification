# <span style="color: #51e2f5;"> Plant Disease Classification - Capstone Project </span>

**Author:** Rangarajan Ramachandran  
**Course:** Professional Certificate in Machine Learning and Artificial Intelligence  
**Date:** February 2026  

## <span style="color: #9df9ef;">Project Overview </span>

This project investigates the application of convolutional neural networks (CNNs) to classify plant diseases from leaf images. The goal is to develop a baseline machine learning model that can accurately identify common plant diseases and provide a foundation for future improvements in Module 24.

### <span style="color: #edf756;">Research Question

*"Can a machine learning model using convolutional neural networks accurately classify plant diseases from leaf images with sufficient reliability to support decision-making for home gardeners and small-scale growers?"*

## <span style="color: #9df9ef;">Dataset </span>

**Source:** PlantVillage Dataset (Available on Kaggle)

### <span style="color: #edf756;"> **Dataset Characteristics:** </span>
- **Total Images:** 54,305
- **Number of Classes:** 38 disease categories
- **Plant Species:** 14 species (Tomato, Potato, Pepper, Corn, Apple, Grape, Cherry, Peach, Strawberry, Blueberry, Orange, Raspberry, Soybean, Squash)
- **Image Format:** JPEG, RGB color
- **Image Dimensions:** 256×256 pixels (uniform across dataset)
- **Split:** 70% Training (38,014 images), 15% Validation (8,145 images), 15% Test (8,146 images)

### <span style="color: #edf756;"> **Class Distribution:** Highly imbalanced (CV=88.99%) </span>
- Range: 152 - 5,507 images per class
- Ratio: 36.2:1 (max to min)
- Standard deviation: 1,271.74 images
- This significant imbalance necessitates stratified sampling and class-weighted loss functions during training

### <span style="color: #edf756;"> **Healthy vs. Diseased:** </span>
- Healthy leaves: 15,084 images (27.78%)
- Diseased leaves: 39,221 images (72.22%)
- Disease prevalence reflects realistic agricultural scenarios where disease identification is the primary use case

### <span style="color: #edf756;"> **Plant Representation:** </span>
- **Most represented:** Tomato (18,160 images, 33.4% of dataset)
- **Moderately represented:** Orange (5,507), Soybean (5,090), Grape (4,062), Corn (3,852)
- **Under-represented:** Raspberry (371 images, 0.7% of dataset)

### <span style="color: #edf756;"> **Classes Include:** </span>
- Healthy plant leaves for each species
- Various fungal diseases (early blight, late blight, leaf spots, powdery mildew, rust)
- Bacterial diseases (bacterial spot, canker)
- Viral diseases (mosaic viruses, leaf curl)
- Pest damage (spider mites)

## <span style="color: #9df9ef;">Methodology </span>

### <span style="color: #edf756;">1. Exploratory Data Analysis (EDA) </span>

#### <span style="color: #ffa8B6;">Comprehensive analysis of the dataset including:
- **Class distribution analysis:** Identified severe imbalance (36.2x range) requiring special handling
- **Image property analysis:** Verified uniform dimensions (256×256) and format (JPEG, RGB)
- **Healthy vs diseased ratio:** 72.22% diseased reflects realistic use case
- **Per-plant disease distribution:** Tomato dominates with 10 disease categories
- **Sample visualization:** Displayed representative images across all 38 classes
- **Data quality checks:** No corrupted images, missing values, or duplicates found

#### <span style="color: #ffa8B6;">**Key EDA Findings:**
1. Severe class imbalance (CV=88.99%) with 36x difference between largest and smallest classes
2. Tomato diseases comprise 33.4% of entire dataset, creating potential bias
3. All images standardized at 256×256 pixels in JPEG format with RGB color
4. Dataset captured under controlled laboratory conditions with uniform backgrounds
5. Plant species representation varies from 18,160 images (Tomato) to 371 images (Raspberry)

### <span style="color: #edf756;">2. Data Preprocessing </span>

#### <span style="color: #ffa8B6;">**Image Preparation:**
- **Resizing:** All images resized to 224×224 pixels (MobileNetV2 requirement)
- **Normalization:** Pixel values scaled to [0, 1] range using rescaling (1./255)
- **Train/Val/Test Split:** Stratified 70/15/15 split maintaining class distribution
  - Training: 38,014 images
  - Validation: 8,145 images  
  - Test: 8,146 images

#### <span style="color: #ffa8B6;">**Data Augmentation** (Training set only):
- Rotation: ±20 degrees
- Width/height shift: 20%
- Shear transformation: 20%
- Zoom: 20%
- Horizontal flip: Yes
- Fill mode: Nearest neighbor

#### <span style="color: #ffa8B6;">**Rationale:** 
- Augmentation increases robustness to real-world image variations (lighting, angle, orientation) while preventing overfitting on the relatively small per-class samples.

### <span style="color: #edf756;">3. Baseline Model Architecture </span>

#### <span style="color: #ffa8B6;">**Model:** 
- Transfer Learning with MobileNetV2

#### <span style="color: #ffa8B6;">**Architecture Details:**
- **Base Model:** MobileNetV2 pre-trained on ImageNet
- **Input Shape:** 224×224×3 (RGB images)
- **Feature Extraction:** Pre-trained convolutional layers
- **Custom Classification Head:**
  - Global Average Pooling layer
  - Dropout (0.2) for regularization
  - Dense layer (38 units, softmax activation)
- **Total Parameters:** ~2.3M (only classification head trainable)

#### <span style="color: #ffa8B6;">**Training Configuration:**
- **Optimizer:** Adam (learning_rate=0.001)
- **Loss Function:** Categorical Crossentropy
- **Metrics:** Accuracy
- **Batch Size:** 32
- **Epochs:** 10 with early stopping
- **Callbacks:**
  - Early Stopping (patience=3, monitor='val_loss')
  - Learning Rate Reduction (patience=2, factor=0.2)

#### <span style="color: #ffa8B6;">**Rationale for MobileNetV2:**
- Lightweight architecture suitable for potential mobile deployment
- Proven effectiveness for image classification tasks
- Fast inference time (~45ms per image)
- Good accuracy-efficiency trade-off for baseline model
- Pre-trained weights provide strong feature extraction

## <span style="color: #9df9ef;">Results </span>

### <span style="color: #edf756;">Model Performance </span>

| Metric | Score |
|--------|-------|
| **Test Accuracy** | **94.46%** |
| **Test Loss** | 0.1784 |
| **Macro Avg Precision** | 0.9407 |
| **Macro Avg Recall** | 0.9205 |
| **Macro Avg F1-Score** | 0.9258 |
| **Weighted Avg Precision** | 0.9464 |
| **Weighted Avg Recall** | 0.9446 |
| **Weighted Avg F1-Score** | 0.9440 |

**Training Performance:**
- Final Training Accuracy: 92.69%
- Final Validation Accuracy: 93.95%
- Training Time: ~4-5 hours on Google Colab T4 GPU
- Model converged with minimal overfitting (train/val gap <2%)

### <span style="color: #edf756;">Performance Analysis </span>

The baseline MobileNetV2 model achieved **strong performance** (94.46% test accuracy), demonstrating effective transfer learning for plant disease classification. This exceeds typical human non-expert performance and approaches expert-level accuracy for many disease categories.

#### <span style="color: #ffa8B6;">**Best Performing Classes (F1-Score > 0.99):**
1. **Grape___Leaf_blight** (F1: 0.9969) - Distinctive lesion patterns
2. **Cherry___Powdery_mildew** (F1: 0.9968) - Clear white coating symptoms
3. **Squash___Powdery_mildew** (F1: 0.9964) - Similar distinctive white symptoms
4. **Blueberry___healthy** (F1: 0.9956) - Clear healthy leaf appearance
5. **Orange___Haunglongbing** (F1: 0.9952) - Characteristic chlorosis pattern

#### <span style="color: #ffa8B6;">**Common characteristics of best performers:**
- Distinctive visual symptoms (powdery coating, chlorosis)
- Clear differentiation from healthy leaves
- Adequate training samples (>1,000 images)
- Consistent symptom presentation across images

#### <span style="color: #ffa8B6;">**Worst Performing Classes (F1-Score < 0.85):**
1. **Potato___healthy** (F1: 0.5625) - Poorest performing class
2. **Tomato___Early_blight** (F1: 0.6719) - Confused with other tomato diseases
3. **Tomato___Target_Spot** (F1: 0.7786) - Similar appearance to spider mites
4. **Corn___Gray_leaf_spot** (F1: 0.8025) - Confused with Northern Leaf Blight
5. **Tomato___Spider_mites** (F1: 0.8280) - Subtle pest damage, small visual cues

#### <span style="color: #ffa8B6;">**Common characteristics of worst performers:**
- Subtle or early-stage symptoms
- Visual similarity to other diseases on same plant
- Smaller class sizes (<500 images)
- High intra-class variation

### <span style="color: #edf756;">Patterns in Misclassifications </span>

Confusion matrix analysis reveals several systematic error patterns:

#### <span style="color: #ffa8B6;"> **1. Same-plant disease confusion (65% of top errors):**
- **Tomato diseases dominate errors:** 8 of top 10 confusions involve tomato
- **Most problematic pairs:**
  - Target Spot ↔ Spider mites (bidirectional, 32 total errors)
    - Target Spot → Spider mites: 18 errors (8.5%)
    - Spider mites → Target Spot: 14 errors (5.6%)
  - Corn Gray leaf spot ↔ Northern Leaf Blight (27 total errors)
    - Gray leaf spot → Northern Leaf Blight: 14 errors (18.2% - highest single error rate)
    - Northern Leaf Blight → Gray leaf spot: 13 errors (8.8%)
- **Bacterial spot acts as "catch-all":** 57 false positives from Target Spot (16), Yellow Leaf Curl Virus (16), Septoria leaf spot (12), and Early blight (13)

##### <span style="color: #a28089;"> **Pattern indicates:** </span> 
Model learns plant species identification well but struggles with disease-specific features on the same plant, particularly for tomato which dominates the training data.

#### <span style="color: #ffa8B6;"> **2. Tomato bacterial spot over-prediction (57 false positives):**
- Incorrectly predicted for Target Spot, Yellow Leaf Curl Virus, Septoria leaf spot, and Early blight
- Likely cause: Small spot-like patterns common to multiple tomato diseases
- Suggests bacterial spot learned as "default" classification for ambiguous tomato leaf damage
- **Implication:** Requires finer-grained feature learning for spot size, distribution, and texture

#### <span style="color: #ffa8B6;"> **3. High-impact single confusions:**
- **Corn Gray leaf spot → Northern Leaf Blight: 18.2% error rate** (worst single confusion)
- Both diseases cause elongated lesions on corn leaves
- Known challenge even for human agricultural experts
- May benefit from plant-specific expert models in Module 24

#### <span style="color: #ffa8B6;"> **4. Bidirectional confusion patterns indicate genuine uncertainty:**
- Target Spot ↔ Spider mites (32 errors: 18+14)
- Corn diseases ↔ each other (27 errors: 14+13)
- **This is positive:** Shows model isn't systematically biased, just genuinely uncertain
- **Action for Module 24:** Candidates for ensemble methods or confidence thresholding

#### <span style="color: #ffa8B6;"> **5. Cross-plant confusions minimal:**
- **Zero cross-species errors in top 10** misclassifications
- Only same-plant confusions appear in top error cases
- **Demonstrates:** Excellent plant species identification
- **Main weakness:** Disease differentiation within same plant species

#### <span style="color: #ffa8B6;"> **6. Disease type clustering:**
- **Spot diseases** confused with each other (5 cases)
- **Blight diseases** confused with each other (2 cases)
- Model struggles with diseases sharing symptom terminology and appearance

### <span style="color: #edf756;">Key Insights from Error Analysis </span>

#### <span style="color: #ffa8B6;"> **Strengths:**
- ✅ **Strong overall accuracy (94.46%)** demonstrates effective baseline
- ✅ **Excellent plant species identification** (zero cross-species errors in top 10)
- ✅ **Bidirectional confusions** indicate genuine model uncertainty, not systematic bias
- ✅ **Near-perfect performance** on distinctive diseases (powdery mildew, chlorosis)
- ✅ **Botanically reasonable errors** (confused diseases have similar visual symptoms)

#### <span style="color: #ffa8B6;"> **Weaknesses:**
- ⚠️ **Tomato disease differentiation** (80% of top errors involve tomato)
- ⚠️ **Bacterial spot over-prediction** (57 false positives across 4 disease types)
- ⚠️ **Subtle pest damage detection** (spider mites F1: 0.828, poorest among pests)
- ⚠️ **Class imbalance impact** (Potato healthy F1: 0.563 with only 152 training images)
- ⚠️ **Early-stage symptom detection** (early blight performs poorly: F1: 0.672)

#### <span style="color: #ffa8B6;"> **Implications for Real-World Deployment:**
- ✅ **Suitable for well-defined diseases:** Late blight, powdery mildew, healthy leaf identification
- ⚠️ **Requires expert verification for:** Spider mites, bacterial spots, early-stage infections
- ⚠️ **Class-specific confidence thresholds needed:** Especially for bacterial spot (over-predicted)
- ⚠️ **Under-represented plants need caution:** Raspberry, Potato have limited training data

## <span style="color: #9df9ef;">Technologies Used </span>

- **Python 3.8+** - Programming language
- **TensorFlow 2.15** / **Keras** - Deep learning framework
- **NumPy 1.24** - Numerical computing
- **Pandas 2.0** - Data manipulation and analysis
- **Matplotlib 3.7** - Visualization
- **Seaborn 0.12** - Statistical visualization
- **Scikit-learn 1.3** - Data splitting, metrics, preprocessing
- **PIL (Pillow) 10.0** - Image processing

## <span style="color: #9df9ef;">Project Structure </span>


[Plant-Disease-Identification](https://github.com/cryoraj/Plant-Disease-Identification/tree/main)\
├── [README.md](https://github.com/cryoraj/Plant-Disease-Identification/blob/main/README.md) # This file - project documentation\
├── [models]()\
│   └── [best_model_Baseline.h5](https://github.com/cryoraj/Plant-Disease-Identification/blob/main/notebooks/best_model_Baseline.h5)      # Trained baseline model\
├── [notebooks](https://github.com/cryoraj/Plant-Disease-Identification/tree/main/notebooks)\
│   └── [plant_disease_eda_baseline.ipynb](https://github.com/cryoraj/Plant-Disease-Identification/blob/main/notebooks/plant_disease_eda_baseline.ipynb)  # Main analysis notebook with EDA + baseline model\
├── [PlantVillageData](https://github.com/cryoraj/Plant-Disease-Identification/tree/main/PlantVillageData)\
│   └── [color](https://github.com/cryoraj/Plant-Disease-Identification/tree/main/PlantVillageData/color)\                 
│       ├── [Apple___Apple_scab](https://github.com/cryoraj/Plant-Disease-Identification/tree/main/PlantVillageData/color/Apple___Apple_scab)\
│       ├── [Apple___Black_rot](https://github.com/cryoraj/Plant-Disease-Identification/tree/main/PlantVillageData/color/Apple___Black_rot)\
│       └── ... (38 total class folders)\
├── [UNDERSTANDING_THE_DATA.md](https://github.com/cryoraj/Plant-Disease-Identification/blob/main/Understanding%20The%20Data.md)         # Comprehensive dataset documentation



## <span style="color: #9df9ef;">Key Findings Summary </span>

### <span style="color: #edf756;">Dataset Characteristics: </span>
- **54,305 images** across **38 disease categories** and **14 plant species**
- **Highly imbalanced:** 36.2:1 ratio between largest (5,507 images) and smallest class (152 images)
- **Tomato-dominant:** 33.4% of entire dataset, creating potential model bias
- **Disease prevalence:** 72.22% diseased vs 27.78% healthy (realistic agricultural scenario)
- **Uniform format:** All images 256×256 JPEG RGB, captured in controlled conditions

### <span style="color: #edf756;">Baseline Model Results: </span>
- **94.46% test accuracy** - Strong performance demonstrating effective transfer learning
- **Training stability:** Minimal overfitting (92.69% train, 93.95% val, 94.46% test)
- **Macro F1: 0.9258** - Good balanced performance across classes
- **Weighted F1: 0.9440** - Excellent overall classification considering class sizes

### <span style="color: #edf756;">Performance Patterns: </span>
1. **Excellent on distinctive diseases:** Powdery mildew, chlorosis, leaf blight (F1 > 0.99)
2. **Struggles with subtle symptoms:** Spider mites (F1: 0.828), early blight (F1: 0.672)
3. **Tomato diseases most confused:** 80% of top errors involve tomato
4. **Zero cross-species errors** in top 10 - excellent plant identification
5. **Class size matters:** Under-represented classes (Potato healthy: 152 images) perform poorly (F1: 0.563)

### <span style="color: #edf756;">Error Analysis: </span>
- **65% same-plant errors:** Model identifies plant well but struggles with disease differentiation
- **Bacterial spot over-predicted:** 57 false positives suggest it's learned as "default" for ambiguous damage
- **Highest single error:** Corn Gray leaf spot → Northern Leaf Blight (18.2%)
- **Bidirectional confusions:** Target Spot ↔ Spider mites (32 errors) shows genuine model uncertainty

## <span style="color: #9df9ef;"> Acknowledgments </span>

- **PlantVillage Project** for providing the open-access dataset
- **TensorFlow/Keras team** for the deep learning framework
- **UC Berkeley Professional Certificate in ML & AI Program** for course structure and guidance
