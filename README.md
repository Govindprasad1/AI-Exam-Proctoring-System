AI-Exam-Proctoring-System

An AI-driven online examination proctoring system designed to assist in monitoring candidates during remote examinations using computer vision, real-time video processing, head pose estimation, and audio analysis.

The project is developed as a B.Tech Computer Science Engineering main project and demonstrates how AI-based techniques can be integrated with a FastAPI backend and real-time WebSocket communication to build an automated examination monitoring system.

---

📌 Overview

Online examinations make traditional physical supervision difficult, especially when candidates are taking examinations remotely.

This project aims to provide an AI-assisted monitoring system that analyzes the candidate's environment and behavior during an examination. The system processes video and audio streams and identifies activities that may indicate suspicious behavior.

The main monitoring components include:

- Real-time video processing
- Face detection
- Object detection
- Head pose and head movement monitoring
- Voice activity detection
- Real-time WebSocket communication
- FastAPI backend
- Detection event processing

The system is designed primarily for desktop and laptop environments, where sufficient computational resources are available for real-time AI processing.

---

🎯 Objectives

The main objectives of the system are:

- To provide AI-assisted monitoring for online examinations
- To detect potentially suspicious activities in real time
- To monitor candidate head movements and orientation
- To identify the presence of multiple people or faces
- To detect potentially unauthorized objects
- To analyze audio for human speech
- To provide real-time communication between the client and backend
- To reduce the dependency on continuous physical supervision

---

✨ Features

1. Real-Time Video Processing

The system accepts video frames from a client and processes them using computer vision techniques.
Video processing can be used for:

- Face detection
- Face presence monitoring
- Head movement analysis
- Head direction estimation
- Object detection

---

2. Face Detection

The backend provides an image analysis endpoint that detects faces using OpenCV Haar Cascade classifiers.

For each analyzed frame, the system can return:

- Number of detected faces
- Bounding-box coordinates of detected faces

This can help identify situations such as:

- No candidate visible
- Multiple people present
- Candidate moving away from the camera

---

3. Head Direction Detection

The system uses MediaPipe Face Mesh to analyze facial landmarks and estimate the candidate's head direction.

The current implementation can classify head direction into categories such as:

- Looking Left
- Looking Right
- Looking Straight

This can help identify frequent or prolonged deviations from the examination screen.

---

4. Object Detection

The proposed system architecture includes YOLO-based object detection for identifying potentially unauthorized objects in the examination environment.

Examples include:

- Mobile phones
- Books
- Other unauthorized objects

YOLO is suitable for this application because of its real-time object detection capability.

«The availability of YOLO detection depends on the model and implementation present in the repository.»

---

5. Voice Activity Detection

The proposed system incorporates Silero Voice Activity Detection (VAD) for analyzing audio streams.

Silero VAD can distinguish between:

- Human speech
- Silence
- Background audio

Speech detected during an examination can be flagged for further investigation.

«Audio monitoring depends on the corresponding audio-processing implementation being enabled in the deployed version.»

---

6. WebSocket Communication

The system uses WebSockets for real-time communication between the client and FastAPI backend.

The "/video" WebSocket endpoint can receive binary image frames from a client.

This provides a foundation for continuous transmission of:

- Video frames
- Detection results
- Monitoring information
- Alerts

---

7. FastAPI Backend

FastAPI acts as the backend framework for the system.

It handles:

- HTTP requests
- WebSocket connections
- Image/frame processing
- Face detection
- Head direction analysis
- Communication with the client

---

--

🔄 System Workflow

The general workflow of the system is:

1. The candidate accesses the online examination through the web interface.
2. The webcam captures the candidate's video.
3. The microphone can capture the examination environment's audio.
4. Video frames are transmitted to the FastAPI backend.
5. The backend decodes and processes the incoming frames.
6. Face detection identifies the number and location of visible faces.
7. MediaPipe analyzes facial landmarks and estimates head direction.
8. YOLO can be used to identify potentially unauthorized objects.
9. Silero VAD can analyze the audio stream for human speech.
10. Detection results are processed by the monitoring system.
11. Suspicious events can be flagged for further review.

📂 Repository Structure

AI-Exam-Proctoring-System/
│
├── app.py
├── main.py
├── backend.py
├── detection.py
│
├── requirements.txt
│
└── README.md

"app.py"

FastAPI application containing a WebSocket endpoint that receives video frames and uses MediaPipe Face Mesh to estimate head direction.

"main.py"

A lightweight FastAPI WebSocket implementation that receives video frames and validates incoming frame data.

"backend.py"

Provides the REST API endpoint:

POST /analyze_frame

It accepts an uploaded image and uses OpenCV Haar Cascade face detection to return the number of detected faces and their coordinates.

"detection.py"

Standalone webcam-based demonstration using:

- OpenCV
- MediaPipe Face Mesh

It detects facial landmarks and displays the estimated head direction.

---

🛠️ Technologies Used

Technology| Purpose
Python| Core programming language
FastAPI| Backend web framework
WebSocket| Real-time communication
OpenCV| Computer vision and image processing
MediaPipe| Face landmarks and head pose/direction estimation
YOLO| Real-time object detection
Silero VAD| Voice activity detection
NumPy| Numerical and array processing
MySQL| Relational database
HTML/CSS/JavaScript| Web-based client interface
Git/GitHub| Version control

---

💻 Requirements

- Python 3.8 or higher
- Webcam
- Microphone for audio-based monitoring
- 8 GB RAM or higher recommended
- Desktop or laptop
- Stable network connection

Python Dependencies

The core implementation uses packages such as:

fastapi
uvicorn
opencv-python
numpy
mediapipe
python-multipart
websockets

Additional packages may be required depending on the enabled YOLO, audio-processing, or database modules.

---

🚀 Installation

1. Clone the Repository

git clone https://github.com/YOUR-USERNAME/AI-Exam-Proctoring-System.git

Navigate to the project:

cd AI-Exam-Proctoring-System

---

2. Create a Virtual Environment

Windows

python -m venv .venv

Activate it:

.venv\Scripts\activate

Linux / macOS

python3 -m venv .venv

Activate it:

source .venv/bin/activate

---

3. Install Dependencies

If "requirements.txt" is available:

pip install -r requirements.txt

Otherwise, install the core dependencies:

pip install fastapi uvicorn opencv-python numpy mediapipe python-multipart websockets

---

▶️ Running the Project

Run "main.py"

The basic WebSocket server can be started with:

uvicorn main:app --reload --host 0.0.0.0 --port 8000

The WebSocket endpoint is:

ws://127.0.0.1:8000/video

---

Run "app.py"

The MediaPipe-based WebSocket server can be started using:

uvicorn app:app --reload --host 0.0.0.0 --port 8000

This version performs MediaPipe-based processing on incoming video frames.

---

Run "backend.py"

The REST-based frame analysis server can be started using:

uvicorn backend:app --reload --host 0.0.0.0 --port 8001

The API will be available at:

http://127.0.0.1:8001

---

🔌 API Endpoints

WebSocket — "/video"

ws://127.0.0.1:8000/video

The endpoint accepts binary messages containing encoded image frames, such as JPEG image data.

The received frames can then be processed by the backend.

---

POST — "/analyze_frame"

POST /analyze_frame

Input

The endpoint accepts an image using:

multipart/form-data

with the field name:

file

Example using cURL

curl -X POST "http://127.0.0.1:8001/analyze_frame" \
     -F "file=@/path/to/image.jpg"

Example response

{
    "faces": 1,
    "coordinates": [
        [120, 80, 200, 220]
    ]
}

Where:

- "faces" = number of detected faces
- "coordinates" = bounding-box coordinates of detected faces

---

📡 WebSocket Frame Flow

A client can send encoded video frames to the WebSocket server.

Webcam
   │
   ▼
Capture Frame
   │
   ▼
Encode as JPEG
   │
   ▼
WebSocket
   │
   ▼
FastAPI Server
   │
   ▼
Decode Frame
   │
   ▼
Computer Vision Processing
   │
   ▼
Detection Result

---

🧪 Local Detection Demo

The "detection.py" script provides a standalone webcam demonstration.

Run:

python detection.py

The application captures frames from the webcam and uses MediaPipe Face Mesh to detect facial landmarks and estimate the candidate's head direction.

Possible outputs include:

Looking Left
Looking Right
Looking Straight

---

🗄️ Database

The proposed complete system can use a relational SQL database such as MySQL for storing examination-related information.

Potential database records include:

- User information
- Examination sessions
- Detected events
- Violation logs
- System logs
- Monitoring results

The database allows detected events to be stored and reviewed later.

---

🖥️ Environment

The system is primarily designed for desktop and laptop platforms.

Supported operating systems include:

- Windows
- Linux
- macOS

Desktop and laptop systems are preferred because real-time video processing and machine learning inference can require considerable computational resources.

---

📊 Proposed Detection Capabilities

Monitoring Area| Technology| Purpose
Face Detection| OpenCV| Detect candidate faces
Face Landmarks| MediaPipe| Extract facial landmarks
Head Direction| MediaPipe| Detect left/right/straight orientation
Object Detection| YOLO| Detect potentially unauthorized objects
Voice Activity| Silero VAD| Detect human speech
Video Communication| WebSocket| Stream frames in real time
Backend| FastAPI| Process requests and detection data
Data Storage| MySQL| Store users, sessions and events

---

🔮 Future Enhancements

The current repository provides the foundational components for an AI-based proctoring system. The following features can be further integrated or improved:

- Real-time YOLO object detection
- Candidate face recognition and identity verification
- Multiple-person detection and tracking
- Eye-gaze tracking
- Advanced behavioral analysis
- Improved audio analysis
- Automated violation scoring
- Examination session reports
- Administrator dashboard
- Database integration for monitoring events
- Cloud deployment
- Integration with online examination platforms

---

🎓 Academic Information

Project: AI-Driven Online Exam Proctoring System

Project Type: B.Tech Main Project

Domain: Artificial Intelligence / Machine Learning / Computer Vision

Core Technologies:

Python
FastAPI
OpenCV
YOLO
MediaPipe
Silero VAD
WebSocket
MySQL

The project was developed to explore the use of artificial intelligence and machine learning techniques for automated monitoring of remote online examinations.

---

👨‍💻 Author

Govind Prasad

B.Tech Computer Science Engineering

---

📜 License

This project is developed primarily for academic and educational purposes.
