# Interaction Log

- Session start timestamp (captured): 2026-03-08 12:20:37 +00:00
- Project: `c:\Users\Hoang Long\Desktop\MSIN0097 Predictive Analytics Individual CW`
- Main notebook edited: `Individual-Predictive-coursework-WVQX7/deepfashion_subset_builder.ipynb`

## Timeline Of Work

### 1) Step 2 EDA insertion (between old cell 12 and old cell 13)
Added a markdown heading `## Step 2: Exploratory Data Analysis` and 2 code cells:
- Missingness & Data Quality
- Class Imbalance Deep Dive

### 2) Continued Step 2 additions
Added 2 more code cells after the first two Step 2 EDA cells:
- Sample Image Grid (3 categories x 4 samples, PIL resize to 128x128)
- Image Property Outliers (1,000 sample analysis: width, height, aspect ratio, file size, with 2x2 plots)

### 3) Colour correlation analysis
Added:
- One code cell for colour-attribute correlation heatmap
- One markdown interpretation cell after that plot

### 4) Modelling narrative markdown (Step 4.1 to 4.7)
Inserted markdown cells before each modelling code block without modifying code cells:
- Before Step 4.1: introduced `## Step 4: Model Exploration & Selection`
- Before Step 4.15, 4.2, 4.3, 4.4, 4.5, and 4.7: added short narrative context paragraphs

### 5) Step 4.5 overfitting correction (Optuna champion training)
Patched champion training loop with patience-based early stopping:
- Added `patience = 3`
- Tracked `best_val_loss` and `best_epoch`
- Used `copy.deepcopy(champion.state_dict())` to keep best checkpoint
- Stopped training after 3 non-improving epochs
- Restored best weights after loop
- Printed: `Early stopping triggered at epoch X. Best val_loss: Y at epoch Z`
- `champion_history` naturally only records epochs up to stop point

### 6) Step 5 narrative markdown insertion
Inserted markdown before Step 5.2:
- `## Step 5: Fine-Tuning & Evaluation`
Inserted markdown after Step 5.2 and before Step 6:
- Interpretation paragraph covering confusion pairs, calibration, and t-SNE

### 7) Required coursework reflection cell
Inserted markdown before Step 6:
- Heading: `### Agent Mistake Identified & Corrected`
- One-paragraph narrative describing a real mistake from workflow: Step 4.5 champion retraining overfit issue and correction via early stopping + best-weight restore

### 8) Runtime error fixes after user execution
Addressed two follow-up errors from Step 2 EDA:
- SyntaxError in missingness plot cell due to unclosed parenthesis in `plt.figure(...)`
  - Fixed to: `plt.figure(figsize=(14, max(4, len(subset_df) / 500)))`
- TypeError `unsupported operand type(s) for -: 'str' and 'int'` when mapping category labels
  - Fixed category mapping cells to derive `category_label_0idx` from row index (stable mapping) instead of subtracting `1` from a parsed text column from `list_category_cloth.txt`

## Files Modified

- `Individual-Predictive-coursework-WVQX7/deepfashion_subset_builder.ipynb`
- `interaction_log_2026-03-08.md` (this file)

## Notes

- Some markdown text previews in terminal were shown without visible line breaks due to notebook JSON rendering, but markdown content was inserted as separate lines.
- Error fixes were applied directly in the notebook cells, no external scripts were introduced.
