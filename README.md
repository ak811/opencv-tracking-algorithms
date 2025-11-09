# Object Tracking with Lucas–Kanade, Farnebäck, Mean/CAMShift, and OpenCV Tracker APIs

A lightweight playground for classic object-tracking algorithms using OpenCV. Compare sparse & dense optical flow (Lucas–Kanade / Farnebäck), color-based trackers (MeanShift / CAMShift), and several OpenCV tracker APIs (MIL/KCF/TLD/Boosting/MedianFlow).

---

## Algorithms

* **Lucas–Kanade Optical Flow** (sparse) — tracks good corner features through time.
* **Gunnar–Farnebäck Optical Flow** (dense) — estimates per-pixel motion and visualizes it in HSV.
* **MeanShift & CAMShift** — color histogram back-projection tracking initialized from a detected face.
* **OpenCV Tracker APIs** — interactive ROI selection + tracker of your choice (MIL/KCF/TLD/Boosting/MedianFlow).

---

## Requirements

* **Python** ≥ 3.8
* **OpenCV (contrib build)**

  ```bash
  pip install opencv-contrib-python
  ```
* **NumPy**

  ```bash
  pip install numpy
  ```

### Optional assets (required for MeanShift/CAMShift)

Download OpenCV’s face cascade and place it here:

```
tracking-algorithms-opencv-master/data/haarcascade_frontalface_default.xml
```

You can get it from the official OpenCV repo.

---

## How to run

All scripts read from your **default webcam (index 0)** and render a window. Press **Esc** to quit.

### 1) Lucas–Kanade (sparse optical flow)

Tracks up to 10 Shi–Tomasi corners and draws tracks.

```bash
python src/LucasKanadeOpticalFlow.py
```

**Tips**

* Add more texture to the scene for better features.
* Tweak `corner_track_params` and `lk_params` inside the script.

---

### 2) Farnebäck (dense optical flow)

Visualizes motion direction (Hue) and magnitude (Value) in an HSV image.

```bash
python src/GunnarFarnebackDenseOpticalFlow.py
```

**Tips**

* Larger motions benefit from a higher `winsize` and `numIters`.
* Camera motion will “move everything”; try a static camera for clearer object motion.

---

### 3) MeanShift (color histogram tracking)

Auto-initializes on the first detected face; tracks via histogram back-projection.

```bash
python src/MeanShiftTracking.py
```

### 4) CAMShift (adaptive window)

Similar to MeanShift but adapts window size/rotation; visualized with a rotated box.

```bash
python src/CamShiftTracking.py
```

**Notes for Mean/CAMShift**

* Requires `data/haarcascade_frontalface_default.xml`.
* If no face is found, the script will fail at `face_rects[0]`. See “Troubleshooting” to switch to a manual ROI.

---

### 5) Tracker APIs (MIL / KCF / TLD / Boosting / MedianFlow)

Interactive ROI selection; choose a tracker from the menu.

```bash
python src/TrackingAPIs.py
```

**Controls**

* When the first frame appears, select an ROI with the mouse and press **Enter** or **Space** to confirm.
* Press **Esc** to exit.

---

## OpenCV version notes (important)

OpenCV’s tracker API names moved across versions. If you see errors like **`AttributeError: module 'cv2' has no attribute 'TrackerKCF_create'`**, try the **legacy** namespace:

Replace e.g.

```python
tracker = cv2.TrackerKCF_create()
```

with

```python
tracker = cv2.legacy.TrackerKCF_create()
```

Likewise for others: `TrackerMIL_create`, `TrackerMedianFlow_create`, `TrackerBoosting_create`.
Some builds **do not ship TLD** anymore; if `TrackerTLD_create` fails, pick a different tracker (e.g., KCF or MIL).

---

## Troubleshooting

* **Black window / no camera**
  Your webcam might not be index 0. Edit the scripts: `cv2.VideoCapture(1)` or another index. Close any app already using the camera.

* **Face detector not found**
  Ensure the cascade file exists at `data/haarcascade_frontalface_default.xml` (relative to repo root).
  Or change the path in the scripts:

  ```python
  cv2.CascadeClassifier('ABSOLUTE/PATH/TO/haarcascade_frontalface_default.xml')
  ```

* **No face detected → index error**
  Guard against empty detections:

  ```python
  face_rects = face_cascade.detectMultiScale(frame)
  if len(face_rects) == 0:
      # Fallback: let the user pick an ROI
      roi = cv2.selectROI(frame, False)
      track_window = tuple(map(int, roi))
  else:
      (x, y, w, h) = face_rects[0]
      track_window = (x, y, w, h)
  ```

  (You can paste that into MeanShift/CAMShift to make initialization robust.)

* **Import errors for trackers**
  Install the contrib build (`opencv-contrib-python`) and try the `cv2.legacy` variants as above.

* **Performance**
  Reduce frame size or parameters (e.g., Lucas–Kanade `winSize`, Farnebäck `pyr_scale`/`winsize`) for smoother results.

---

## Customization ideas

* Accept video files instead of webcam:

  ```python
  cap = cv2.VideoCapture("path/to/video.mp4")
  ```
* Save output videos with `cv2.VideoWriter`.
* Add command-line flags via `argparse` (e.g., to select tracker, set window sizes, or choose camera index).
* Compute basic metrics (track length, average speed) for a quick comparison.

---

## Acknowledgments

* OpenCV team and contributors for algorithms and pre-trained cascades.
* Classic references: Lucas & Kanade (1981), Farnebäck (2003), Comaniciu & Meer (2002), Bradski (1998/2000).
