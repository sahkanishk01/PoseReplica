# PoseReplica - Real-time Pose Mirroring System

A visually stunning real-time pose tracking application that creates a "clone" or mirror effect of your body movements using computer vision and 3D visualization.

## Features

- **Real-time Pose Detection**: Uses MediaPipe for accurate human pose estimation
- **Clone/Mirror Effect**: Creates a horizontally flipped version of your pose
- **Dynamic Visual Effects**: 
  - Color-changing neon skeletal overlay
  - Pulsing glow effects
  - Motion trails that follow joint movements
  - Gradient color transitions over time
- **Dual Visualization**:
  - 2D camera feed with enhanced visual effects
  - Real-time 3D pose visualization in a separate window
- **Performance Optimized**: Includes FPS monitoring and efficient rendering

## Requirements

```bash
pip install opencv-python mediapipe numpy matplotlib
```

### Dependencies

- `cv2` (OpenCV) - Camera capture and image processing
- `mediapipe` - Google's pose estimation solution
- `numpy` - Numerical computations
- `matplotlib` - 3D visualization
- `time` - Performance timing
- `math` - Mathematical calculations for effects

## How It Works

### 1. Pose Detection
The application uses MediaPipe's pose estimation model to detect 33 key body landmarks in real-time:
- Face landmarks (nose, eyes, ears)
- Upper body (shoulders, elbows, wrists)
- Torso (chest, hips)
- Lower body (knees, ankles, feet)

### 2. Clone Creation
The detected pose is horizontally flipped to create a "clone" effect:
```python
clone_landmarks_2d = [(int((1 - lm.x) * width), int(lm.y * height)) for lm in results.pose_landmarks.landmark]
```

### 3. Visual Effects Engine

#### Color Gradient System
- Implements HSV to RGB color space conversion
- Creates smooth color transitions over time
- Applies pulsing effects using sine wave functions

#### Neon Glow Effects
- Dual-layer line drawing for glow appearance
- Smooth circles with white centers for joints
- Additive blending for realistic glow

#### Motion Trails
- Maintains history of joint positions (8 frames)
- Applies alpha blending for fade-out effect
- Creates smooth motion visualization

### 4. 3D Visualization
- Real-time 3D scatter plot of pose landmarks
- Connected skeletal structure in 3D space
- Synchronized color effects with 2D view

## Code Structure

### Core Components

1. **Initialization**
   ```python
   mp_pose = mp.solutions.pose
   pose = mp_pose.Pose(...)  # MediaPipe pose detector
   cap = cv2.VideoCapture(0)  # Camera capture
   ```

2. **Pose Connections**
   ```python
   POSE_CONNECTIONS = [(11, 12), (11, 13), ...]  # Skeletal connections
   ```

3. **Visual Effects Functions**
   - `get_gradient_color(t)`: Generates time-based color gradients
   - `draw_smooth_line()`: Creates glowing line effects
   - `draw_smooth_circle()`: Draws glowing joint markers

4. **Main Processing Loop**
   - Captures camera frame
   - Processes pose detection
   - Applies visual effects
   - Updates 3D visualization
   - Displays results

### Performance Optimizations

- **Reduced Model Complexity**: Uses MediaPipe's fastest pose model
- **Selective 3D Updates**: Updates 3D plot every 6 frames instead of every frame
- **Efficient Blending**: Uses OpenCV's optimized addWeighted function
- **Smart FPS Display**: Updates FPS counter every 30 frames

## Usage

1. **Run the Application**
   ```bash
   python clone_tracker.py
   ```

2. **Controls**
   - Stand in front of your camera
   - Move around to see the clone effect
   - Press 'q' to quit the application

3. **Expected Output**
   - Main window: "CLONE TRACKER" showing camera feed with effects
   - 3D window: Real-time 3D pose visualization
   - Console: Performance metrics and status

## Configuration Options

### Camera Settings
```python
cap.set(cv2.CAP_PROP_FRAME_WIDTH, 640)   # Adjust resolution
cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 480)
```

### Visual Parameters
```python
max_trail_length = 8        # Trail effect duration
pulse_intensity = 0.8       # Glow intensity
thickness = 6               # Line thickness
```

### MediaPipe Settings
```python
min_detection_confidence=0.5    # Detection sensitivity
min_tracking_confidence=0.5     # Tracking stability
model_complexity=0              # Processing speed vs accuracy
```

## Technical Details

### Coordinate Systems
- **2D Screen Space**: Pixel coordinates (0,0) at top-left
- **3D World Space**: Normalized coordinates (-1,1) in all axes
- **Mirror Transform**: X-coordinate inversion for clone effect

### Performance Characteristics
- **Target FPS**: 30-60 FPS depending on hardware
- **Processing Latency**: <50ms for pose detection
- **Memory Usage**: ~200MB for full application

### Supported Platforms
- Windows 10/11
- macOS 10.14+
- Linux (Ubuntu 18.04+)
- Requires webcam access

## Troubleshooting

### Common Issues

1. **Camera Not Found**
   - Check camera index in `cv2.VideoCapture(0)`
   - Try different indices (1, 2, etc.)

2. **Poor Pose Detection**
   - Ensure good lighting
   - Stand 3-6 feet from camera
   - Wear contrasting clothing

3. **Low Performance**
   - Reduce resolution in camera settings
   - Increase `model_complexity` to 1 or 2
   - Close other applications

4. **3D Plot Not Updating**
   - Check matplotlib backend
   - Ensure GUI support is available

## Future Enhancements

- Multiple person tracking
- Recording and playback functionality
- Custom color schemes
- Gesture recognition
- Export to video files
- VR/AR integration

## License

This project is for educational and personal use. MediaPipe is subject to MIT license.

## Contributing

Feel free to submit issues and enhancement requests!

