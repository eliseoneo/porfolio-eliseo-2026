# LIME XGBoost Approach

## Overview

This directory contains a comprehensive **LIME-focused explainability analysis** specifically designed for **XGBoost tree-based classification models**. Unlike the multi-method comparison in the main examples, this approach provides deep, instance-level explanations using LIME's local interpretability framework.

## Key Features

### 🎯 Focus Areas

1. **Instance-Level Explanations**: Detailed analysis of individual predictions
2. **Local Feature Importance**: How features influence specific predictions
3. **Explanation Stability**: Consistency analysis across multiple instances
4. **Actionable Insights**: Domain-specific interpretation for oil & gas experts
5. **Comprehensive Visualizations**: High-resolution plots for each instance

### 🔬 What Makes This Approach Unique

| Feature | This Approach | Standard LIME |
|---------|---------------|---------------|
| **Instances Analyzed** | 20 (configurable) | 3-5 typical |
| **Perturbations** | 5000 per instance | 1000-2000 typical |
| **Stability Analysis** | ✓ Included | ✗ Not standard |
| **Aggregate Importance** | ✓ Computed | ✗ Not standard |
| **Tree-Based Focus** | ✓ Optimized for XGBoost | General purpose |
| **Visualizations** | Individual + Aggregate | Basic plots |

## Installation

### Prerequisites

```bash
# Ensure virtual environment is activated
source .venv/bin/activate

# Install required packages (if not already installed)
pip install xgboost scikit-learn pandas numpy matplotlib seaborn lime
```

### macOS-Specific (XGBoost OpenMP)

```bash
# Install OpenMP runtime
brew install libomp

# Set environment variables
export LDFLAGS="-L/opt/homebrew/opt/libomp/lib"
export CPPFLAGS="-I/opt/homebrew/opt/libomp/include"
```

## Usage

### Basic Execution

```bash
# From project root directory
.venv/bin/python lime_xgboost_approach/xgb_mod.py
```

### With Environment Variables (macOS)

```bash
export LDFLAGS="-L/opt/homebrew/opt/libomp/lib"
export CPPFLAGS="-I/opt/homebrew/opt/libomp/include"
.venv/bin/python lime_xgboost_approach/xgb_mod.py
```

### Expected Execution Time

- **Total**: 5-8 minutes
- **Data Loading**: < 1 second
- **Model Training**: 0.2 seconds
- **LIME Explanations**: 4-6 minutes (20 instances × 5000 perturbations each)
- **Visualization**: 10-20 seconds

## Output Files

The script generates **9 comprehensive output files** in the `results/` directory:

### CSV Files (3)

| File | Content | Size |
|------|---------|------|
| `lime_xgb_aggregate_importance.csv` | Aggregate feature importance across all instances | ~1 KB |
| `lime_xgb_stability_metrics.csv` | Stability statistics (mean, std, CV) for each feature | ~2 KB |
| `lime_xgb_detailed_report.txt` | Comprehensive text report with insights | ~3 KB |

### Visualization Files (6)

| File | Content | Resolution |
|------|---------|------------|
| `lime_instance_0_explanation.png` | Instance 0 detailed explanation | 300 DPI |
| `lime_instance_1_explanation.png` | Instance 1 detailed explanation | 300 DPI |
| `lime_instance_2_explanation.png` | Instance 2 detailed explanation | 300 DPI |
| `lime_instance_3_explanation.png` | Instance 3 detailed explanation | 300 DPI |
| `lime_instance_4_explanation.png` | Instance 4 detailed explanation | 300 DPI |
| `lime_xgb_aggregate_importance.png` | Top 20 features aggregate bar chart | 300 DPI |

## Methodology

### 1. Data Preparation

```
Oil & Gas Dataset (600 samples, 30 features)
    ↓
80/20 Train-Test Split (stratified)
    ↓
StandardScaler Normalization
    ↓
Ready for XGBoost Training
```

### 2. Model Training

```python
XGBClassifier(
    n_estimators=100,      # 100 trees
    max_depth=6,           # Tree depth
    learning_rate=0.1,     # Step size
    subsample=0.8,         # 80% data per tree
    colsample_bytree=0.8   # 80% features per tree
)
```

### 3. LIME Explanation Pipeline

```
For each of 20 test instances:
    ↓
Generate 5000 perturbed samples
    ↓
Train local linear model
    ↓
Extract feature importance
    ↓
Visualize + Save
```

### 4. Stability Analysis

```
Aggregate across all 20 instances:
    ↓
Calculate mean importance
    ↓
Calculate standard deviation
    ↓
Compute coefficient of variation (CV)
    ↓
Identify stable vs variable features
```

## Interpreting Results

### Aggregate Importance CSV

**Format**:
```csv
feature,lime_importance
depth_feet,0.068042
gamma_ray_api,0.032156
formation_age_ma,0.021978
...
```

**Interpretation**:
- Higher values = Greater influence on predictions
- Focus on top 5-10 features for domain validation
- Compare with SHAP results for cross-validation

### Stability Metrics CSV

**Format**:
```csv
feature,mean_importance,std_importance,min_importance,max_importance,cv
depth_feet,0.068,0.012,0.042,0.089,0.176
...
```

**Key Metric: Coefficient of Variation (CV)**
- **Low CV (<0.3)**: Stable, reliable feature across instances
- **Medium CV (0.3-0.6)**: Moderate variability, context-dependent
- **High CV (>0.6)**: Highly variable, less reliable for global insights

### Instance Explanations

Each instance visualization shows:

**Left Panel**: Feature contributions (green = positive, red = negative)
**Right Panel**: Prediction probability distribution

**Use Cases**:
- Debugging individual predictions
- Explaining model decisions to stakeholders
- Identifying edge cases or outliers
- Validating domain knowledge

## Customization

### Analyze More Instances

```python
# In main() function, change:
num_instances = 50  # Analyze 50 instances instead of 20
```

### Increase Perturbations (Higher Stability)

```python
# In analyze_lime_explanations(), change:
num_samples=10000  # 10,000 perturbations (slower but more stable)
```

### Change Number of Top Features

```python
# In analyze_lime_explanations(), change:
num_features=20  # Show top 20 features per instance
```

### Modify XGBoost Hyperparameters

```python
# In train_xgboost_model(), modify:
xgb_model = XGBClassifier(
    n_estimators=200,      # More trees
    max_depth=8,           # Deeper trees
    learning_rate=0.05,    # Slower learning
    ...
)
```

## Expected Results

### Model Performance

```
Accuracy:  88-92%
Precision: 88-91%
Recall:    95-99%
F1-Score:  92-95%
ROC-AUC:   0.70-0.75
```

### Top Features (Typical)

```
1. depth_feet              (0.068)
2. gamma_ray_api           (0.032)
3. formation_age_ma        (0.022)
4. recovery_factor_percent (0.016)
5. velocity_ft_s           (0.013)
```

### Domain Validation

| Feature | LIME Rank | Oil & Gas Intuition | Match? |
|---------|-----------|---------------------|--------|
| **depth_feet** | #1 | Deeper = better formations | ✓ YES |
| **gamma_ray_api** | #2 | Shale/source rock indicator | ✓ YES |
| **formation_age** | #3 | Geological maturity matters | ✓ YES |
| **porosity** | Top 10 | Storage capacity critical | ✓ YES |

## Comparison: LIME XGBoost vs Standard LIME

### Advantages of This Approach

1. **Tree-Based Optimization**: Designed specifically for XGBoost interpretability
2. **High Perturbation Count**: 5000 samples = more stable local models
3. **Stability Metrics**: Quantifies explanation reliability
4. **Aggregate Analysis**: Identifies globally important features from local explanations
5. **Comprehensive Visuals**: Production-ready plots for stakeholders

### When to Use This Approach

✅ **Use LIME XGBoost** when:
- Need instance-specific explanations ("Why this prediction?")
- Explaining decisions to non-technical stakeholders
- Debugging edge cases or misclassifications
- Comparing local vs global feature importance
- Tree-based model (XGBoost, RandomForest, etc.)

❌ **Use SHAP instead** when:
- Need global feature importance (all predictions)
- Want theoretically guaranteed explanations
- Computational speed is critical
- Comparing across different model types

## Troubleshooting

### Issue: "ImportError: libomp.dylib not loaded"

**Solution** (macOS):
```bash
brew install libomp
export LDFLAGS="-L/opt/homebrew/opt/libomp/lib"
export CPPFLAGS="-I/opt/homebrew/opt/libomp/include"
```

### Issue: "Architecture mismatch (x86_64 vs arm64)"

**Solution**: Use virtual environment Python:
```bash
.venv/bin/python lime_xgboost_approach/xgb_mod.py
```

### Issue: LIME explanations take too long

**Solution**: Reduce perturbations or instances:
```python
num_instances = 10      # Fewer instances
num_samples=2000        # Fewer perturbations
```

### Issue: Low explanation stability (high CV)

**Causes**:
- Feature has low actual importance
- Feature has non-linear effects (interactions)
- Insufficient perturbation samples

**Solution**: Increase perturbations to 10,000 per instance

## References

- **LIME Paper**: "Why Should I Trust You?" (Ribeiro et al., 2016)
- **XGBoost Paper**: "XGBoost: A Scalable Tree Boosting System" (Chen & Guestrin, 2016)
- **Dataset**: Synthetic Oil & Gas Well Productivity (30 geological/engineering features)

## License

This code is part of the SHAP/LIME explainability project. See main repository README for license details.

## Contact

For questions or issues, please refer to the main project repository or documentation.
