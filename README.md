# 🐘 Automated Wildlife Tracking & Re-Identification Pipeline

[![Paper](https://img.shields.io/badge/Paper-Drone%20Systems%20%26%20Applications-blue)](https://cdnsciencepub.com/doi/10.1139/dsa-2025-0032)

This repository contains the official code and implementation for the paper **"A framework for detecting and tracking elephants in drone videos"**, published in *Drone Systems and Applications* (2025).

It provides an end-to-end computer vision pipeline designed to detect, track, and re-identify animals in drone video footage. While developed specifically for tracking **Elephants**, the framework is adaptable to other species.

It solves the common problem of "ID Switching" (where a tracker loses an animal and re-assigns a new ID) by using a novel post-processing algorithm based on visual similarity (SIFT/SSIM) and physical feasibility.

🔗 **[Read the Full Paper](https://cdnsciencepub.com/doi/10.1139/dsa-2025-0032)**

## 🌟 Features

* **Detection & Tracking:** Uses **YOLOv11** combined with **BotSort** for robust frame-by-frame tracking.
* **Smart Re-Identification:** automatically stitches broken tracks together using:
    * **SIFT Features & Homography** (to align animal bodies across frames).
    * **SSIM (Structural Similarity)** (to compare visual identity).
    * **Velocity Checks** (to ensure merges are physically possible).
* **Analytics Dashboard:** Automatically generates:
    * Movement trajectories (X/Y plots).
    * Distance traveled per ID.
    * Herd density heatmaps.
    * Social interaction (overlap) statistics.
    * Cluster analysis (identifying sub-groups within the herd).
* **Visualization:** Outputs a fully annotated video with corrected IDs.

## 📂 Repository Structure

```text
├── notebooks/
│   ├── 1_Training_Pipeline.ipynb     # Train YOLOv11 on your custom dataset
│   ├── 2_Evaluation_Pipeline.ipynb   # Calc Precision, Recall, mAP & Confusion Matrix
│   └── 3_Inference_Pipeline.ipynb    # Main Pipeline: Tracking -> ReID -> Viz -> Stats
├── configs/
│   ├── botsortV4.yaml                # Tracker configuration
│   ├── TestData.yaml                 # Dataset configuration
├── scripts/
│   ├── Viz.py                        # Analytics generation script
│   └── process_pipeline.py           # Python script version of the pipeline
└── README.md
```

## 🧠 Model Weights

To run this pipeline, you need the trained YOLOv11 model weights used in our study.

**📥 [Download Trained Weights (best_xl.pt)](https://drive.google.com/file/d/1xcIQm5liQEj3ZSYKPyXt9xvLcb03tU0_/view?usp=sharing)**

*Place the downloaded `best_xl.pt` file in the root directory or your Google Drive working folder.*

## 🚀 Getting Started

### Prerequisites
The project is optimized to run on **Google Colab** (for GPU access) or a local machine with a dedicated GPU.

**Install Dependencies:**
```bash
pip install ultralytics opencv-python pandas numpy scikit-image seaborn tqdm matplotlib
```
### Running the Pipeline

1.  **Training (Optional):**
    If you want to train the model from scratch on your own dataset, open `notebooks/1_Training_Pipeline.ipynb`. Ensure your dataset is formatted according to YOLO standards.

2.  **Evaluation:**
    To test the model's accuracy (mAP, Precision, Recall), run `notebooks/2_Evaluation_Pipeline.ipynb`.

3.  **Inference (Tracking & Analysis):**
    This is the main tool. Open `notebooks/3_Inference_Pipeline.ipynb`.
    * Set `INPUT_VIDEO` to your video path.
    * Set `MODEL_PATH` to the downloaded `best_xl.pt`.
    * Run all cells to generate videos and statistics.

## 📊 Outputs

The pipeline automatically creates a `pipeline_results` folder containing:

* **`Final_Output_ReID.mp4`**: The video with corrected tracking IDs overlaid.
* **`tracking_processed.csv`**: Cleaned tracking data after Re-ID.
* **`stats/`**:
    * `id_statistics.txt`: Seconds/Frames visible per ID.
    * `overlap.csv`: Interaction percentages between animals.
    * `cluster_frame_ranges.csv`: Grouping dynamics over time.
    * `sum_distance_per_id.csv`: Total distance traveled.
* **`plots/`**:
    * `trajectories.png`: Path of movement for every animal.
    * `density_heatmap.png`: Where the herd spent the most time.
    * `avg_distance_plot.png`: Average movement speed of the herd.

## 📚 Citation

If you use this code or model in your research, please cite our paper:

> **Chaim Chai Elchik, Serge Wich, and André Burger. 2025. A framework for detecting and tracking elephants in drone videos. *Drone Systems and Applications*. 13: 1-16. https://doi.org/10.1139/dsa-2025-0032**

## 📜 License
This project is open-source and available under the MIT License.
