# People-flow-detection

This project detects people in a video, tracks their movements, counts them as they cross designated lines (IN/OUT), and generates a heatmap showing areas with the most movement.

Detection Method:
Object Detection: YOLOv8 (yolov8n.pt) is used to detect objects in each video frame.
Class Filter: Only the person class (COCO class 0) is tracked.
Tracking: DeepSort from deep_sort_realtime tracks individual people across frames using their bounding boxes.
Bounding Boxes: Each detected person gets a bounding box and a unique track_id.

Line Coordinates:
Two horizontal lines are used to determine IN/OUT counting:
Upper Line (y_upper): Default at ~35% of frame height
People moving downward across this line are counted as IN
Lower Line (y_lower): Default at ~65% of frame height
People moving upward across this line are counted as OUT
The exact coordinates are automatically computed based on video frame height if not manually set.
y_upper = int(height * 0.35) # green line y_lower = int(height * 0.65) # red line

IN/OUT Counting Logic
Track History: For each person (track_id), store the last few center points of their bounding box (deque with max length 8).

Movement Direction:
Compute dx, dy as the difference between the first and last center in the history.
dy > 0 → moving downward, dy < 0 → moving upward.
IN Counting:

If the person was above y_upper and now is below y_upper and moving downward (dy > 0) → IN.

OUT Counting:

If the person was below y_lower and now is above y_lower and moving upward (dy < 0) → OUT.

Avoid Double Counting: Each track_id is counted only once per direction.

Heatmap Generation

Each person's center coordinates are scaled down and accumulated in a heatmap array.
The heatmap is normalized, blurred, color-mapped (JET), and overlaid on a frame for visualization.
Shows areas with the highest density of people movement.

Visual Output

Video: Annotated with bounding boxes, track IDs, lines, and IN/OUT counts.

Heatmap: Saved as outputs/people_heatmap.png.
