# Drowsiness_Detection_Live
# 🚗 Real-Time Driver Drowsiness Detection System 👁️

### Detect Drowsiness. Generate Alerts. Promote Safer Driving.

A real-time **Driver Drowsiness Detection System** built using Python, OpenCV, and MediaPipe. This project monitors a person's eyes through a webcam and detects prolonged eye closure using the **Eye Aspect Ratio (EAR)** technique.

When the eyes remain closed beyond a predefined time threshold, the system triggers a visual warning and an audible alert to notify the user.

The project demonstrates how computer vision can be used to develop real-time monitoring systems that may help improve driver awareness.

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Project Objectives](#-project-objectives)
- [Key Features](#-key-features)
- [Technologies Used](#-technologies-used)
- [How It Works](#-how-it-works)
- [Eye Aspect Ratio (EAR)](#-eye-aspect-ratio-ear)
- [Project Workflow](#-project-workflow)
- [Installation](#-installation-and-setup)
- [How to Run](#-how-to-run-the-project)
- [Controls](#-keyboard-controls)
- [Project Structure](#-project-structure)
- [Applications](#-real-world-applications)
- [Limitations](#-limitations)
- [Learning Outcomes](#-learning-outcomes)
- [Author](#-author)

---

## 📖 Overview

Driver fatigue and drowsiness can reduce attention, reaction time, and awareness while driving.

This project implements a computer vision-based approach to monitor eye closure in real time using a webcam.

The system uses **MediaPipe Face Landmarker** to identify facial landmarks around the eyes. These landmarks are used to calculate the Eye Aspect Ratio (EAR), which helps estimate whether the eyes are open or closed.

The system also includes a calibration phase that estimates the user's normal eye openness before starting continuous monitoring.

If the eyes remain closed for approximately 1.5 seconds, the application activates a drowsiness warning with a flashing red visual overlay and an audible beep.

**Project Type:** Computer Vision | Real-Time Detection | Python Project

**Interface:** OpenCV webcam window

**Implementation:** Jupyter Notebook

---

## 🎯 Project Objectives

The main objectives of this project are:

- Detect facial landmarks using MediaPipe.
- Monitor eye movements through a live webcam feed.
- Calculate the Eye Aspect Ratio to estimate eye closure.
- Implement automatic eye calibration for individual users.
- Distinguish short eye closures from prolonged closures.
- Generate visual and audible warnings when prolonged eye closure is detected.
- Display real-time eye measurements and monitoring statistics.

---

## ✨ Key Features

### 1. 📷 Real-Time Webcam Monitoring
Captures live video from the webcam and processes each frame to monitor the user's eyes.

### 2. 👁️ Facial Landmark Detection
Uses MediaPipe Face Landmarker to identify key points around both eyes.

### 3. 📐 Eye Aspect Ratio Calculation
Calculates the Eye Aspect Ratio (EAR) using six landmark points for each eye.

### 4. ⚙️ Automatic Eye Calibration
Observes eye openness for approximately three seconds to estimate a personalized eye-closure threshold.

### 5. ⏱️ Prolonged Eye-Closure Detection
Tracks how long the eyes remain closed and identifies prolonged closures using a configurable time threshold.

### 6. 🔔 Audio Alert
Generates a beep when a drowsiness alert is activated. The sound is played in a separate thread to avoid intentionally blocking video processing.

### 7. 🚨 Visual Warning System
Displays a warning message and a flashing red overlay when prolonged eye closure is detected.

### 8. 📊 Live Monitoring Statistics
Displays useful information, including:
- Current Eye Aspect Ratio (EAR)
- Eye-closure threshold
- Number of detected blinks
- Number of drowsiness alerts
- Duration of the current eye closure

### 9. 🔄 Recalibration Support
Allows the user to restart calibration while the application is running.

### 10. 🛠️ Configurable Parameters
Allows important detection parameters to be adjusted according to the user's requirements.

---

## 🧰 Technologies Used

| Technology | Purpose |
|---|---|
| Python | Core programming language |
| OpenCV | Webcam access, image processing, visual overlays, and video display |
| MediaPipe | Real-time facial landmark detection |
| NumPy | Numerical operations and landmark-distance calculations |
| Matplotlib | Visual demonstration of open-eye and closed-eye EAR |
| Threading | Plays audio alerts without deliberately blocking the video loop |
| Jupyter Notebook | Development, experimentation, and code execution |

---

## ⚙️ How It Works

The system follows a sequence of image-processing and decision-making steps.

### Step 1: Webcam Initialization
The application opens the default webcam and continuously captures video frames.

### Step 2: Image Preprocessing
Each frame is flipped horizontally to provide a mirror-like view and converted from BGR to RGB for MediaPipe processing.

### Step 3: Facial Landmark Detection
MediaPipe identifies facial landmarks, including the points surrounding the left and right eyes.

### Step 4: Eye Landmark Extraction
Six landmark points are selected for each eye to calculate its Eye Aspect Ratio.

### Step 5: Eye Aspect Ratio Calculation
The system calculates the EAR for both eyes and takes their average to estimate eye openness.

### Step 6: Automatic Calibration
During the initial calibration period, the system collects EAR measurements and estimates a threshold based on the average measured EAR.

### Step 7: Eye-Closure Monitoring
After calibration, the system compares the current EAR against the threshold to estimate whether the eyes are open or closed.

### Step 8: Blink and Drowsiness Assessment
Short eye closures are counted as blinks when their duration falls within the configured range. If eye closure continues for the configured drowsiness duration, the system activates an alert.

### Step 9: Visual and Audio Alerts
When a drowsiness alert is activated, the application displays a warning and generates an audible beep.

### Step 10: Live Statistics
The application updates the displayed EAR, blink count, alert count, and eye-closure progress bar.

---

## 📐 Eye Aspect Ratio (EAR)

The **Eye Aspect Ratio (EAR)** is a geometric measure used to estimate eye openness from facial landmarks.

It compares vertical distances between the eyelids with the horizontal width of the eye.

The formula is:

\[
EAR = \frac{\|P_2-P_6\|+\|P_3-P_5\|}{2\|P_1-P_4\|}
\]

Where:

- \(P_1\) and \(P_4\) represent the horizontal corners of the eye.
- \(P_2\) and \(P_3\) represent upper-eyelid landmarks.
- \(P_5\) and \(P_6\) represent lower-eyelid landmarks.
- \(\|\cdot\|\) represents Euclidean distance.

### How EAR Is Used

- **Higher EAR:** Generally indicates a more open eye.
- **Lower EAR:** Generally indicates a more closed eye.
- **Prolonged low EAR:** Can indicate a prolonged eye closure and trigger the warning system.

The exact EAR values vary across individuals and conditions. Therefore, this project estimates a personalized threshold during calibration rather than relying exclusively on a fixed threshold.

### Detection Parameters

The current implementation uses the following configuration:

| Parameter | Value | Purpose |
|---|---:|---|
| `DROWSY_SECONDS` | 1.5 seconds | Duration of eye closure required to trigger an alert |
| `MIN_BLINK_SECONDS` | 0.05 seconds | Minimum closure duration counted as a blink |
| `CALIBRATION_SECONDS` | 3 seconds | Initial calibration duration |
| `THRESHOLD_FRACTION` | 0.75 | Fraction of average calibration EAR used as the threshold |
| `DEFAULT_EAR_THRESHOLD` | 0.21 | Fallback threshold if calibration is unavailable |
| `CAMERA_NUMBER` | 0 | Default webcam index |

These values can be adjusted in the notebook to experiment with different detection settings.

---

## 🔄 Project Workflow

```text
          Start Application
                  |
                  v
          Initialize Webcam
                  |
                  v
         Capture Video Frame
                  |
                  v
       Detect Facial Landmarks
                  |
                  v
        Extract Eye Landmarks
                  |
                  v
        Calculate Average EAR
                  |
                  v
          Calibration Phase
                  |
                  v
         Compare EAR with
        Personalized Threshold
                  |
                  v
         Estimate Eye-Closure
              Duration
                  |
           +------+------+
           |             |
           v             v
       Eyes Open     Eyes Closed
           |             |
           v             v
     Reset Closure   Track Duration
        Timer             |
                          v
                Closure >= 1.5 sec?
                          |
                    +-----+-----+
                    |           |
                    v           v
                   No          Yes
                    |           |
                    v           v
                Continue    Trigger Alert
                Monitoring  + Audio Warning
                                |
                                v
                         Update Statistics
```

---

## 💻 Installation and Setup

### Prerequisites

Before running the project, make sure you have:

- Python installed on your computer.
- A working webcam.
- An internet connection for downloading the MediaPipe face-landmark model on the first run.
- Jupyter Notebook or an environment capable of running `.ipynb` files.

### Step 1: Clone the Repository

Replace `YOUR_USERNAME` and `YOUR_REPOSITORY` with your actual GitHub username and repository name.

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
```

Navigate to the project directory:

```bash
cd YOUR_REPOSITORY
```

### Step 2: Create a Virtual Environment

```bash
python -m venv venv
```

Activate it on Windows:

```bash
venv\Scripts\activate
```

### Step 3: Install the Required Libraries

```bash
python -m pip install --upgrade pip
python -m pip install mediapipe opencv-python numpy matplotlib notebook
```

**Important:** Install the packages in the same Python environment used by your Jupyter Notebook. If OpenCV is already installed in conflicting variants, resolve the conflict before running the project.

### Step 4: Launch Jupyter Notebook

```bash
jupyter notebook
```

Open `Drowsiness_Detection_Live.ipynb`.

Run the notebook cells in the correct order.

---

## ▶️ How to Run the Project

1. Install the required Python libraries.
2. Open `Drowsiness_Detection_Live.ipynb`.
3. Execute the import and initialization cells.
4. Run the Eye Aspect Ratio demonstration cell.
5. Configure the detection parameters if needed.
6. Run the MediaPipe model initialization cells.
7. Execute the cell that starts webcam monitoring.
8. Look toward the webcam with your eyes open during the calibration period.
9. Keep the application running to observe real-time monitoring.
10. Press `q` in the OpenCV video window to exit.

On the first run, the notebook downloads the MediaPipe face-landmark model file named `face_landmarker.task` if it is not already present.

**Note:** The live webcam window requires a graphical desktop environment. Some hosted notebook environments do not support direct webcam access or OpenCV GUI windows without additional configuration.

---

## ⌨️ Keyboard Controls

| Key | Action |
|---|---|
| `q` | Quit the live webcam monitoring window |
| `c` | Restart the eye-calibration process |

Click the OpenCV video window before pressing these keys.

---


## 🌍 Real-World Applications

Potential applications of this type of computer vision system include:

- 🚘 Driver attention monitoring prototypes
- 🚛 Fatigue-monitoring research for commercial drivers
- 🖥️ Computer vision learning and experimentation
- 🔬 Human-computer interaction research
- 🎓 Academic projects involving real-time image processing
- 🤖 Prototyping intelligent driver-assistance features

These are potential applications, not validated capabilities of this particular implementation.

---

## ⚠️ Limitations

This project is an educational prototype and has several limitations:

- Performance depends on webcam quality, lighting, and camera position.
- Facial landmark detection may be less reliable when the face is partially obstructed.
- Glasses, reflections, and head movements may affect eye landmark measurements.
- Eye closure alone cannot reliably establish whether someone is sleepy.
- Calibration may be inaccurate if the user closes their eyes or looks away during the calibration period.
- The current implementation monitors one face.
- The system has not been established as a safety-certified driver-monitoring solution.

**Safety Notice:** This project must not be relied upon as a substitute for adequate rest, responsible driving, or certified vehicle safety systems. Never deliberately test drowsiness while driving. If you feel sleepy behind the wheel, stop in a safe location and rest.

---


## 📚 Learning Outcomes

Through this project, the following concepts can be explored:

- Python programming and modular functions
- Real-time webcam processing using OpenCV
- Facial landmark detection using MediaPipe
- NumPy-based mathematical calculations
- Euclidean distance and geometric feature extraction
- Eye Aspect Ratio (EAR)
- Threshold-based event detection
- Time-based state management
- Multithreading for audio alerts
- Real-time visual feedback and monitoring

This project is a practical example of applying computer vision concepts to a real-time monitoring problem.

---

## 👨‍💻 Author

**Dheeraj Chouhan**

Aspiring AI/ML Engineer | Computer Vision Enthusiast | Python Developer

Interested in Artificial Intelligence, Machine Learning, Deep Learning, Generative AI, and real-world AI applications.

### Connect With Me

- **GitHub:** [dheeraj24-coder](https://github.com/dheeraj24-coder)
---

## ⭐ Support

If you find this project interesting or useful for learning computer vision, consider giving the repository a ⭐ on GitHub.

Feedback and suggestions for improving the project are welcome.

---

### Built with Python, OpenCV, and MediaPipe ❤️
