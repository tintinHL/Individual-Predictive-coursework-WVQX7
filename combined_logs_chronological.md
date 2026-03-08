# Combined Logs (Chronological)

Generated: 2026-03-08 12:26:50 +00:00

---

## Source: .\Individual-Predictive-coursework-WVQX7\interaction_log.md
Sort timestamp: 2026-03-01 00:00:00

# Interaction Log

Date: 2026-03-01  
Project: DeepFashion subset preparation

## Conversation Timeline

1. User requested a Python script to build a 10,000-image training subset from DeepFashion:
   - Read `list_eval_partition.txt` and select first 10,000 `train` images.
   - Read `list_category_img.txt` for category labels.
   - Read `list_attr_img.txt` and `list_attr_cloth.txt` for color-related binary attributes.
   - Merge into one DataFrame and save `final_subset.csv`.

2. Assistant created:
   - `deepfashion_subset_builder.py`
   - Script includes parsing, selection, color-attribute filtering, merge, and CSV export.
   - Syntax check passed (`py_compile`).

3. User requested notebook format instead.

4. Assistant created:
   - `deepfashion_subset_builder.ipynb`
   - Notebook implemented all required steps and export to `final_subset.csv`.

5. User asked to add a sanity-check plot.



7. User reported `FileNotFoundError` for `list_eval_partition.txt`.

8. Assistant investigated workspace layout and found files in:
   - `Eval/list_eval_partition.txt`
   - `Anno_fine/list_attr_img.txt`
   - `Anno_fine/list_attr_cloth.txt`
   - Initially no `list_category_img.txt` found.

9. Assistant updated notebook:
   - Added recursive file auto-discovery.
   - Added clearer missing-file messaging.
   - Added temporary optional fallback for missing `list_category_img.txt`.

10. User clarified they had downloaded wrong annotation files and asked to:
    - Remove optional handling of `list_category_img.txt`.
    - Adjust paths to current project layout.

11. Assistant re-scanned files and found:
    - `Eval/list_eval_partition.txt`
    - `Anno_coarse/list_category_img.txt`
    - `Anno_coarse/list_attr_img.txt`
    - `Anno_coarse/list_attr_cloth.txt`

12. Assistant updated notebook again:
    - Made `list_category_img.txt` mandatory.
    - Set explicit project-layout paths to `Eval/` and `Anno_coarse/`.
    - Removed fallback category inference logic.
    - Added required-file existence check with explicit error list.

## Files Created/Updated During This Interaction

- `deepfashion_subset_builder.py` (created)
- `deepfashion_subset_builder.ipynb` (created and updated multiple times)
- `interaction_log.md` (this file)

## Current Notebook Path Configuration

- `Eval/list_eval_partition.txt`
- `Anno_coarse/list_category_img.txt`
- `Anno_coarse/list_attr_img.txt`
- `Anno_coarse/list_attr_cloth.txt`

## Interaction Entry - 2026-03-08

### User Request
Create a markdown log of interactions with an agent based on this project.

### Assistant Action
- Located the existing project log file: `interaction_log.md`.
- Added this dated entry to keep a continuous interaction history in one markdown file.

### Result
Project interaction log now includes this request and response as a new entry.

## Interaction Entry - 2026-03-08 (Follow-up)

### User Request
Keep track of everything we interact on for this project.

### Assistant Action
- Confirmed continuous logging to `interaction_log.md` for this project.
- Committed to appending a new entry for each subsequent interaction in this session/workflow.

### Result
Interaction tracking is now active as an ongoing process for this project log.

---

## Source: .\Individual-Predictive-coursework-WVQX7\session_log_2026-03-02_00-56-17.md
Sort timestamp: 2026-03-02 00:56:17

# Session Log (Full Interaction)

Created: 2026-03-02 00:56:17 +00:00  
Last Updated: 2026-03-08 (Europe/London)

## Chronological Record

1. Built initial DeepFashion subset script (`deepfashion_subset_builder.py`) for:
- selecting first 10,000 train images
- merging category labels
- extracting color attributes
- exporting `final_subset.csv`

2. Converted workflow into notebook form (`deepfashion_subset_builder.ipynb`).

3. Added color-frequency sanity plot.

4. Fixed file-path issues by aligning to project structure:
- `Eval/list_eval_partition.txt`
- `Anno_coarse/list_category_img.txt`
- `Anno_coarse/list_attr_img.txt`
- `Anno_coarse/list_attr_cloth.txt`

5. Fixed parser errors:
- handled multi-word attribute names in `list_attr_cloth.txt`
- handled packed attribute vectors in `list_attr_img.txt`

6. Strengthened color extraction:
- strict keyword list
- strict word-boundary matching to avoid leaks like `embroidered`/`shirred`

7. Set `category_label` to 0-index (`label - 1`) for model compatibility.

8. Added diagnostics:
- print final columns
- print selected color columns
- verify plotting inputs

9. Added notebook image-copy pipeline:
- copy listed images while preserving folders
- report missing files
- support `img.zip` fallback when `img/` is missing

10. Corrected sampling bias workflow:
- replaced `.head(10000)` with `.sample(n=10000, random_state=42)`
- saved `final_subset_representative.csv`
- preserved old extracted data by renaming:
- `subset_images` -> `subset_images_unbalanced_alphabetical`
- copied new representative images to:
- `subset_images_representative`

11. Added category distribution analysis for report:
- 0-index to category-name mapping
- horizontal frequency chart
- top-3 / bottom-3 reporting and top-3 coverage %

12. Added representative color comparison:
- representative-only color-frequency chart
- comparison vs previous alphabetical subset
- exact per-color count table (initial vs representative)

13. Added Step 3 preparation workflow in notebook:
- stratified split, transforms, DataLoaders, verification grid

14. Revised split policy for report quality:
- removed categories with fewer than 10 samples
- printed removed category names + dropped image count
- enforced strict stratified split on filtered data
- ensured downstream loaders/grid use filtered data only

15. Refactored Step 3 into 3 timed cells:
- Cell 1: data cleaning + split + GC
- Cell 2: DataLoader initialization
- Cell 3: first-batch diagnostic + 3x3 verification

16. Applied memory-safe loader settings:
- `num_workers=0`
- `pin_memory=False`

17. Added CUDA diagnostics and environment checks in Step 3 Cell 2:
- `CUDA_VISIBLE_DEVICES`
- `sys.executable`
- `torch.__version__`
- `torch.version.cuda`
- `torch.cuda.is_available()`
- available RAM via `psutil`
- GPU name/VRAM (when available)
- kernel reset guidance and critical warning message
- `torch.cuda.empty_cache()` when CUDA is available

18. Added/updated logging files:
- `interaction_log.md`
- `session_log_2026-03-02_00-56-17.md` (this file)

## Files Touched During Interaction

- `deepfashion_subset_builder.ipynb` (primary evolving notebook)
- `deepfashion_subset_builder.py` (initial standalone script)
- `copy_deepfashion_subset_images.py`
- `step3_data_preparation.py`
- `final_subset.csv`
- `final_subset_representative.csv`
- `interaction_log.md`
- `session_log_2026-03-02_00-56-17.md`

---

## Source: .\interaction_log_2026-03-08.md
Sort timestamp: 2026-03-08 00:00:00

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

