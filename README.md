# 📊 People Flow Detection & Heatmap Analysis

## 🚀 Project Overview

People Flow Detection is a computer vision system that analyzes pedestrian movement in video footage. It detects and tracks individuals, counts entries and exits using virtual lines, and generates a heatmap highlighting high-traffic areas.

This project demonstrates practical skills in object detection, multi-object tracking, movement analytics, and visual data interpretation, making it well-suited for real-world applications such as crowd monitoring, retail analytics, and smart surveillance systems.

## ✨ Key Features

✅ Person detection using YOLOv8

🔁 Robust multi-object tracking with DeepSort

➕ Accurate IN / OUT counting based on movement direction

🔥 Movement heatmap generation

🎥 Fully annotated video output

📓 Implemented as a Jupyter Notebook for clarity and experimentation


## 🧠 System Architecture

```
Video Input
    ↓
YOLOv8 (Person Detection)
    ↓
DeepSort (Tracking & IDs)
    ↓
Movement Direction Analysis
    ↓
IN / OUT Counting
    ↓
Heatmap Accumulation
    ↓
Visual Output (Video + Heatmap)
```

## 🧍 Detection & Tracking

- Object Detection:

    - Model: YOLOv8 Nano (yolov8n.pt)

    - Framework: Ultralytics YOLO

    - Classes Tracked: Person only (COCO class 0)

    - Each frame is processed independently to detect pedestrians.

- Object Tracking:

    - Tracker: deep_sort_realtime

    - Assigns a persistent track_id to each person

    - Enables trajectory tracking and prevents double counting

## 📏 IN / OUT Line Logic

Two horizontal virtual lines define entry and exit events.

|Line	               |Purpose	       |Default Position     |
| ---------------------|---------------|---------------------|
| Upper Line (Green)   | IN counting   |35% of frame height  |
|                      |               |                     |
| Lower Line (Red)	   | OUT counting  |65% of frame height  | 

```
y_upper = int(height * 0.35), 

y_lower = int(height * 0.65)
```

## 🔄 Movement & Counting Logic

- Track History:

    - Each person’s bounding box center is stored in a deque (length = 8)

    - Used to compute movement direction

- Direction Estimation:
```
dy > 0 → Moving downward

dy < 0 → Moving upward
```
- Counting Rules:
```
# IN Count

Crosses upper line downward

Moving from above → below

dy > 0
```
```
# OUT Count

Crosses lower line upward

Moving from below → above

dy < 0
```

- Each track_id is counted only once per direction.

## 🔥 Heatmap Generation

The heatmap visualizes areas with the highest pedestrian activity.

#### Method:

- Extract center points of tracked people

- Accumulate positions in a heatmap grid

- Normalize & smooth using Gaussian blur

- Apply JET colormap

- Overlay on a video frame


## 🔖 Output

### 🎥 Visual Outputs

📦 Bounding boxes with track IDs

📊 Live IN / OUT counters

📏 Virtual lines

🔥 Movement heatmap

### 🛠 Technologies Used

- Python

- OpenCV

- PyTorch

- Ultralytics YOLOv8

- DeepSort (deep_sort_realtime)

- NumPy

- Matplotlib
  

## ▶️ How to Run
```
git clone https://github.com/Methila-Meem/People-flow-detection.git
```
```
cd people-flow-detection
```
```
pip install -r requirements.txt
```
```
jupyter notebook

# Open People Flow Detection.ipynb and run the cells sequentially.
```

## 🎯 Use Cases

- Retail footfall analytics

- Crowd flow monitoring

- Building entry/exit analysis

- Smart surveillance systems

- Public space utilization studies


## ⚠️ Limitations

- Performance depends on camera angle and lighting

- Occlusions can affect tracking accuracy

- Line placement may need tuning for different scenes
  

## 🔮 Future Improvements

- Live webcam / RTSP stream support

- Multi-line or zone-based counting

- Directional flow visualization

- Database logging & dashboards

- Re-identification across multiple cameras
