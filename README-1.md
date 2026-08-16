# 🏋️ AI Real-time GYM Coach

A real-time AI fitness coaching application that uses computer vision and pose estimation to track exercises, count repetitions, analyze exercise form, and monitor workout progress through a Streamlit interface.

## 🎥 Demo

> Demo video will be added here after the project is pushed and the GitHub video asset is uploaded.

The demo showcases:
- Real-time camera-based pose detection
- Exercise-specific pose analysis
- Automatic repetition counting
- Set and workout progress tracking
- Real-time exercise metrics and visual overlays
- Workout history

## ✨ Features

- **Real-time Pose Detection** using MediaPipe Pose Landmarker
- **Exercise Recognition/Selection** for:
  - Squats
  - Push-ups
  - Biceps Curls (Dumbbell)
  - Shoulder Press
  - Lunges
- **Automatic Rep Counting** using exercise-specific angle and movement thresholds
- **Form Analysis** using body landmarks, joint angles, visibility checks, and exercise-specific rules
- **Real-time Visual Feedback** with pose skeletons and exercise metrics over the camera feed
- **Workout Planning** with configurable sets and reps per set
- **Goal Progress Tracking** for current-set reps, completed sets, and overall workout completion
- **Workout History** stored locally using SQLite
- **User Login** with session-based state management
- **Responsive Camera Processing** using `streamlit-webrtc`
- **Modular Exercise Architecture** using a shared `BaseExercise` abstract class and individual detector classes

## 🧠 How It Works

The application processes the camera stream frame by frame:

```text
Camera
   ↓
WebRTC Video Stream
   ↓
Video Processor
   ↓
MediaPipe Pose Landmarker
   ↓
33 Body Landmarks
   ↓
Exercise Detector
   ↓
Joint Angles + Visibility + Movement Rules
   ↓
Rep / Form Metrics
   ↓
Streamlit UI + Progress Tracking
   ↓
SQLite Workout History
```

Each exercise has its own detector class while common functionality such as angle calculation and landmark access is provided by the `BaseExercise` class.

## 🏗️ Project Architecture

```text
ai-real-time-gym-coach/
│
├── core/
│   └── base_exercise.py
│
├── detectors/
│   ├── squat.py
│   ├── pushup.py
│   ├── biceps_curl.py
│   ├── shoulder_press.py
│   └── lunges.py
│
├── ml_models/
│   └── pose_landmarker_full.task
│
├── pages/
│
├── services/
│   ├── auth/
│   │   └── login_wall.py
│   │
│   ├── coaching/
│   │
│   ├── config/
│   │   └── workout_config.py
│   │
│   ├── persistence/
│   │   └── exercise_repository.py
│   │
│   ├── state/
│   │   └── session_defaults.py
│   │
│   ├── tracking/
│   │   └── metrics.py
│   │
│   ├── ui/
│   │   └── style_loader.py
│   │
│   └── vision/
│       └── exercise_video_processor.py
│
├── static/
│   ├── AdobeClean.otf
│   └── style.css
│
├── main.py
├── requirements.txt
├── .env
└── data.db
```

> `data.db` is a local SQLite database and should not be committed to GitHub.

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| Python | Core application logic |
| Streamlit | Web application interface |
| MediaPipe | Pose landmark detection |
| OpenCV | Frame processing and visual overlays |
| NumPy | Image/frame array processing |
| streamlit-webrtc | Real-time camera/video streaming |
| SQLite | Local workout persistence |
| PyAV | Video frame conversion |
| CSS / HTML | UI styling |

## 🔍 Pose & Exercise Analysis

MediaPipe provides body landmarks containing coordinates and visibility information.

For example, squat analysis uses landmarks such as:

```text
Hip → Knee → Ankle
```

The detector calculates the knee angle and uses predefined movement thresholds to determine whether the user is moving down or returning to the standing position.

A simplified squat cycle is:

```text
Standing
   ↓
Knee angle decreases
   ↓
DOWN
   ↓
Knee angle increases
   ↓
UP
   ↓
Rep counted
```

Visibility thresholds are also used to avoid making decisions when important body landmarks are not reliably detected.

## 🧩 Modular Detector Design

The project uses an abstract base class:

```python
class BaseExercise(ABC):
```

Common functionality is kept in the base class, such as:

- Angle calculation
- Landmark point extraction
- Rep/state storage

Individual exercises extend the base class:

```python
class SquatDetector(BaseExercise):
```

This makes it easier to add new exercises without duplicating common pose-processing logic.

## 📊 Workout Progress

The application synchronizes metrics from the real-time video processor with Streamlit session state.

For a workout with:

```text
Target Sets = 3
Reps per Set = 10
```

the application can track:

```text
Total Reps:       17
Current Set:      7 / 10
Sets Completed:   1 / 3
```

The same progress system works across the supported exercises.

## 💾 Workout History

Completed exercise sets are stored in a local SQLite database.

The database keeps information such as:

- User
- Exercise name
- Repetitions
- Sets
- Time taken
- Creation timestamp

Workout history is then retrieved and displayed through the Streamlit application.

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/adityagosavi2005-cmyk/ai-real-time-gym-coach.git
cd ai-real-time-gym-coach
```

### 2. Create a virtual environment

Windows:

```bash
python -m venv .venv
```

Activate it:

```bash
.venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the application

```bash
streamlit run main.py
```

The application will open in your browser.

## 📋 Usage

1. Open the application.
2. Log in with a username.
3. Select an exercise.
4. Set the target number of sets.
5. Set the repetitions per set.
6. Click **Start Workout**.
7. Allow camera access.
8. Perform the selected exercise in front of the camera.
9. View real-time pose landmarks, exercise metrics, rep counts, and progress.
10. End the workout to finish the session.

## 🔐 Local Configuration

If environment variables are required, create a `.env` file locally.

Do not commit secrets or private configuration files to GitHub.

## 📌 Current Limitations

- Pose detection quality depends on camera quality, lighting, visibility, and body positioning.
- Exercise thresholds are rule-based and may need calibration for different users and camera angles.
- The application is intended as a software/fitness demonstration and is not a substitute for professional medical or fitness advice.
- SQLite is currently used for local persistence.

## 🔮 Future Improvements

- More exercise detectors
- Personalized exercise thresholds
- Improved form feedback
- More advanced voice coaching
- Cloud-based workout synchronization
- User authentication with stronger production security
- Exercise analytics and progress charts
- Mobile-friendly deployment

## 👨‍💻 Author

**Aditya Gosavi**

Information Technology Student  
Rajiv Gandhi Institute of Technology (RGIT), Mumbai

GitHub: https://github.com/adityagosavi2005-cmyk

## 📄 License

This project is intended for educational and portfolio purposes.
