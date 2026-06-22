# Feature-Based Object Tracking — Harris + Lucas–Kanade Optical Flow

> CS4065 Computer Vision · Spring 2026 · FAST-NUCES Lahore  

A classical, training-free object tracking pipeline built from first principles. The user draws a bounding box on the first frame; the system detects Harris corners inside it and propagates them across all subsequent frames using pyramidal Lucas–Kanade optical flow. Loss detection and automatic re-detection handle partial occlusion and temporary disappearance.

---

## Pipeline

```
First frame → User BBox → Harris corner detection
     ↓
 [Per-frame loop]
     ↓
 Pyramidal LK optical flow → filter by status + max drift
     ↓
 Loss check (point count / bbox drift / out-of-frame)
     ↓
 [If lost] → expand search region → re-detect every N frames
     ↓
 Update bbox + centroid → annotate frame → write to output video
     ↓
 [After all frames] → generate 6 diagnostic plots → ZIP archive
```

---

## Outputs

| File | Description |
|---|---|
| `tracked_output.mp4` | Annotated video with bbox (green=tracking, red=lost), centroid crosshair, point overlays |
| `harris_initial.png` | Full frame with corners, ROI crop, Harris response heatmap |
| `sample_frames.png` | 2×3 grid of evenly sampled annotated frames |
| `tracking_analysis.png` | Active point count, centroid X/Y over time, 2D trajectory |
| `bbox_size.png` | Bounding-box width, height, and area over time |
| `heatmap.png` | Cumulative spatial density of bbox presence |
| `loss_analysis.png` | Tracked vs lost frame split + loss-event durations |

---

## Key Parameters

| Parameter | Default | Effect |
|---|---|---|
| `HARRIS_THRESH_RATIO` | 0.10 | Lower → more corners detected |
| `LK_WIN_SIZE` | (41, 41) | Larger → handles faster motion |
| `LK_MAX_LEVEL` | 6 | More pyramid levels → larger capture range |
| `MAX_POINT_DRIFT` | 60 px | Per-frame displacement limit |
| `MAX_BBOX_DRIFT` | 350 px | Max bbox centre jump before declaring loss |
| `LOST_REDETECT_EVERY` | 10 fr | Re-detection attempt interval while lost |

---

## Setup

```bash
pip install opencv-python numpy matplotlib ipywidgets
```

Run in Jupyter or Google Colab. Open `CV-Project.ipynb` and run cells top to bottom.

1. Set `VIDEO_PATH` in the config cell to your input MP4
2. Use the interactive widget (Cell 7) to select start frame and draw a bounding box
3. Run the tracking loop — annotated video and plots are saved automatically

---

## Dependencies

| Library | Version |
|---|---|
| Python | 3.x |
| OpenCV | ≥ 4.5 |
| NumPy | ≥ 1.23 |
| Matplotlib | ≥ 3.6 |
| ipywidgets | any |

---

## Report

Full methodology, results, and discussion are in `CV_Project_Report.docx` / `CV_ObjectTracking_Report.docx`.
