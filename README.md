#  SnapClass - Intelligent AI Attendance System

<p align="center">
  <b>An AI-powered smart attendance platform that automates classroom attendance using Face Recognition, Voice Biometrics, Machine Learning, and Cloud-based Data Management.</b>
</p>

<p align="center">
  <a href="https://snapclass.streamlit.app/"><b>🚀 Live Application</b></a>
  &nbsp; • &nbsp;
  <a href="https://github.com/bibekn414/ai-attendance-project-app"><b>🤖 Application Repository</b></a>
  &nbsp; • &nbsp;
  <a href="https://github.com/bibekn414/ai-attendance-project-landing"><b>🌐 Landing Page Repository</b></a>
</p>

---

##  Project Overview

Traditional classroom attendance systems are often time-consuming, dependent on manual roll calls, and vulnerable to errors or proxy attendance.

**SnapClass** is an intelligent AI-based attendance management system designed to automate this process using multiple biometric technologies.

The system allows students to register their **facial features** and optionally their **voice signature**, while teachers can automatically record attendance using either:

*  Classroom photographs for **Face Recognition**
*  Student voice samples for **Voice Identification**

The platform combines **Computer Vision, Machine Learning, Speaker Recognition, Streamlit, Flask, and Supabase** into a complete attendance-management workflow.

The project is divided into two major applications:

```text
Intelligent-AI-Attendance-System
│
├── ai-attendance-project-app
│   └── Main AI-powered Streamlit application
│
└── ai-attendance-project-landing
    └── Public Flask-based project landing page
```

---

#  Project Objective

The objective of this project is to develop an intelligent attendance management platform that can:

* Reduce the time required for classroom attendance.
* Automatically identify students from classroom images.
* Support voice-based attendance using speaker embeddings.
* Minimize manual attendance entry.
* Provide centralized attendance records for teachers.
* Allow students to view their enrolled courses and attendance history.
* Provide digital course enrollment using subject codes and QR-based workflows.
* Store student, teacher, subject, biometric, and attendance information using a cloud database.

---

#  Key Features

##  AI-Based Face Recognition

SnapClass uses facial biometrics to identify students from classroom photographs.

During student registration, the system extracts a **128-dimensional facial embedding** representing the unique facial characteristics of the student.

For attendance:

1. A teacher uploads one or more classroom photographs.
2. Faces are detected from each photograph.
3. Facial landmarks are extracted.
4. Face embeddings are generated.
5. A trained Machine Learning classifier predicts the corresponding student.
6. Embedding-distance verification is performed.
7. Recognized students are marked present.
8. Attendance is stored in the database.

The implementation uses:

* **Dlib**
* **Face Recognition Models**
* **NumPy**
* **Scikit-learn SVM**

---

##  Voice Biometric Attendance

SnapClass also supports attendance using student voice signatures.

During voice enrollment, the system converts a student's recorded speech into a numerical **speaker embedding**.

When voice attendance is performed:

1. Audio is captured or uploaded.
2. Audio is resampled and preprocessed.
3. Speech segments are detected.
4. Voice embeddings are generated.
5. New embeddings are compared against stored student embeddings.
6. The closest matching speaker is identified using similarity scoring.
7. Matching students are marked present.

The voice pipeline uses:

* **Resemblyzer**
* **Librosa**
* **NumPy**
* Speaker Embeddings
* Similarity-based speaker matching

---

##  Teacher Dashboard

Teachers receive a dedicated dashboard where they can:

* Register a teacher account
* Securely log in
* Create new subjects
* Manage courses
* View enrolled students
* Share subject enrollment codes
* Generate QR-assisted enrollment workflows
* Upload classroom photographs
* Run AI Face Attendance
* Run Voice Attendance
* View historical attendance records
* Track present and total student counts

---

##  Student Dashboard

Students can:

* Register through Face Recognition
* Store facial embeddings
* Optionally register their voice
* Login using FaceID
* Join subjects using enrollment codes
* View enrolled courses
* View total conducted classes
* View attended classes
* Monitor their attendance history
* Unenroll from courses when required

---

#  Machine Learning Architecture

## Face Recognition Pipeline

The face-recognition workflow can be represented as:

```text
Student/Classroom Image
        │
        ▼
Dlib Face Detector
        │
        ▼
Facial Landmark Detection
        │
        ▼
128-D Face Embedding
        │
        ▼
Stored Student Embeddings
        │
        ▼
Linear SVM Classifier
        │
        ▼
Predicted Student ID
        │
        ▼
Embedding Distance Verification
        │
        ▼
Recognized / Unknown Student
        │
        ▼
Attendance Record
```

---

##  Face Embedding Generation

Dlib's face-recognition model converts each detected face into a numerical representation containing **128 values**.

Conceptually:

```text
Face Image
      ↓
Face Detection
      ↓
Facial Landmark Alignment
      ↓
Deep Face Representation
      ↓
128-dimensional embedding
```

Instead of comparing raw images directly, SnapClass compares these numerical representations.

This improves recognition because the embedding captures distinguishing facial characteristics while reducing the original high-dimensional image data.

---

#  SVM-Based Student Classification

The stored facial embeddings are used as features for a **Support Vector Machine (SVM)** classifier.

The model uses:

```python
SVC(
    kernel="linear",
    probability=True,
    class_weight="balanced"
)
```

Conceptually:

```text
X = Student Face Embeddings
y = Student IDs
```

For example:

```text
Embedding 1 → Student 101
Embedding 2 → Student 102
Embedding 3 → Student 103
```

The SVM learns the decision boundaries between registered students.

When a new face is detected, its embedding is passed to the trained classifier to predict the corresponding student ID.

---

#  Face Verification

The project does not rely only on the classifier prediction.

After predicting a student, the system calculates the distance between the detected face embedding and the registered embedding.

The implementation uses a resemblance threshold of approximately:

```text
0.60
```

If:

```text
Embedding Distance ≤ Threshold
```

the student is accepted as a match.

Otherwise, the detected face is treated as unknown.

This additional verification reduces false recognition.

---

#  Voice Recognition Pipeline

The voice-processing architecture follows:

```text
Student Audio
      │
      ▼
Librosa Audio Loading
      │
      ▼
16 kHz Resampling
      │
      ▼
Audio Preprocessing
      │
      ▼
Resemblyzer Voice Encoder
      │
      ▼
Speaker Embedding
      │
      ▼
Stored Voice Embeddings
      │
      ▼
Similarity Calculation
      │
      ▼
Best Matching Student
      │
      ▼
Attendance Record
```

---

#  Speaker Identification

Voice identification is performed by comparing the new voice embedding with stored student voice embeddings.

The project calculates similarity using a vector dot product.

Conceptually:

```text
Similarity = New Voice Embedding · Stored Voice Embedding
```

The system searches for the student producing the highest similarity score.

A matching threshold of approximately:

```text
0.65
```

is used.

If:

```text
Similarity ≥ Threshold
```

the speaker is accepted as the corresponding student.

Otherwise, the voice remains unidentified.

---

#  Bulk Voice Processing

The system can also process longer audio containing multiple spoken segments.

The pipeline:

```text
Complete Audio Recording
        ↓
Detect Non-Silent Segments
        ↓
Remove Very Short Segments
        ↓
Generate Speaker Embedding
        ↓
Compare With Registered Students
        ↓
Identify Speaker
        ↓
Store Highest Confidence Match
```

Librosa's audio splitting is used to separate meaningful speech regions before speaker identification.

---

#  Database Architecture

SnapClass uses **Supabase** as the cloud database backend.

Supabase provides PostgreSQL-based cloud data storage for:

* Teachers
* Students
* Subjects
* Student enrollments
* Face embeddings
* Voice embeddings
* Attendance records

A conceptual database structure is:

```text
Teachers
│
├── teacher_id
├── name
├── username
└── password

Students
│
├── student_id
├── name
├── face_embedding
└── voice_embedding

Subjects
│
├── subject_id
├── subject_code
├── name
├── section
└── teacher_id

Subject_Students
│
├── student_id
└── subject_id

Attendance_Logs
│
├── student_id
├── subject_id
├── timestamp
└── is_present
```

---

#  Authentication & Security

Teacher passwords are not stored directly as plain text.

The application uses:

```text
bcrypt
```

for password hashing.

The workflow is:

```text
Teacher Password
      ↓
bcrypt Hashing
      ↓
Hashed Password
      ↓
Supabase Database
```

During login, bcrypt compares the submitted password against the stored hashed password.

---

#  Complete System Workflow

## 1️⃣ Student Registration

```text
Student Opens SnapClass
        ↓
Select Student Portal
        ↓
Capture Face Using Camera
        ↓
AI Checks Existing Face
        ↓
Face Not Recognized
        ↓
New Student Registration
        ↓
Generate Face Embedding
        ↓
Optional Voice Recording
        ↓
Generate Voice Embedding
        ↓
Save Student Profile
        ↓
Retrain / Refresh Face Classifier
```

---

## 2️⃣ Student Login

```text
Student Camera Input
        ↓
Face Detection
        ↓
Face Embedding
        ↓
SVM Prediction
        ↓
Distance Verification
        ↓
Student Identified
        ↓
Student Dashboard
```

Students therefore do not need a conventional username/password for their normal FaceID workflow.

---

## 3️⃣ Teacher Registration and Login

```text
Teacher Registration
        ↓
Username Validation
        ↓
Password Hashing
        ↓
Supabase Storage
        ↓
Teacher Login
        ↓
Password Verification
        ↓
Teacher Dashboard
```

---

## 4️⃣ Subject Creation

Teachers can create subjects with:

```text
Subject Name
Subject Code
Section
Teacher ID
```

Once created, the subject appears in the teacher dashboard.

---

## 5️⃣ Student Enrollment

Students can enroll into courses through a subject enrollment workflow.

```text
Teacher Creates Subject
        ↓
Unique Subject Code
        ↓
Share Code / QR
        ↓
Student Opens Enrollment Link
        ↓
Student Authentication
        ↓
Student Added to Subject
```

---

#  Face-Based Attendance Workflow

The face-attendance system allows teachers to process multiple classroom photographs.

```text
Teacher Dashboard
        ↓
Select Subject
        ↓
Upload Classroom Photos
        ↓
Convert Images to NumPy Arrays
        ↓
Detect Faces
        ↓
Generate Embeddings
        ↓
Predict Student IDs
        ↓
Compare With Enrolled Students
        ↓
Present / Absent Classification
        ↓
Review Attendance Results
        ↓
Save Attendance
```

Students detected in at least one uploaded classroom photo are marked present.

Students enrolled in the course but not detected are marked absent.

---

#  Voice-Based Attendance Workflow

```text
Teacher Selects Subject
        ↓
Start Voice Attendance
        ↓
Capture Student Voice
        ↓
Audio Preprocessing
        ↓
Speaker Embedding
        ↓
Similarity Matching
        ↓
Identify Registered Student
        ↓
Mark Attendance
        ↓
Store Result
```

This provides an alternative biometric attendance mechanism when facial recognition is not preferred.

---

#  Attendance Analytics

Teacher attendance records are grouped by:

* Attendance session
* Date and time
* Subject
* Subject code

For each class, the teacher can view information similar to:

```text
Date & Time | Subject | Subject Code | Attendance
-------------------------------------------------
10:00 AM    | ML      | ML101        | 42 / 50
```

Student dashboards separately calculate:

```text
Total Classes
Attended Classes
```

for each enrolled subject.

---

#  System Architecture

```text
                         ┌─────────────────────────┐
                         │       SnapClass         │
                         │     Landing Website     │
                         │       Flask App         │
                         └────────────┬────────────┘
                                      │
                                      ▼
                         ┌─────────────────────────┐
                         │      Streamlit App      │
                         └────────────┬────────────┘
                                      │
                     ┌────────────────┴────────────────┐
                     │                                 │
                     ▼                                 ▼
            ┌─────────────────┐               ┌─────────────────┐
            │ Student Portal  │               │ Teacher Portal  │
            └────────┬────────┘               └────────┬────────┘
                     │                                 │
           ┌─────────┴─────────┐             ┌─────────┴──────────┐
           │                   │             │                    │
           ▼                   ▼             ▼                    ▼
      FaceID Login       Voice Enrollment  Face Attendance   Voice Attendance
           │                   │             │                    │
           └──────────────┬────┘             └──────────┬─────────┘
                          │                              │
                          ▼                              ▼
                    AI/ML Pipelines
                          │
          ┌───────────────┴─────────────────┐
          │                                 │
          ▼                                 ▼
   Face Recognition                 Voice Recognition
   Dlib + SVM                       Resemblyzer + Librosa
          │                                 │
          └─────────────────┬───────────────┘
                            │
                            ▼
                      Supabase Database
                            │
          ┌─────────────────┼──────────────────┐
          ▼                 ▼                  ▼
       Students          Subjects         Attendance Logs
```

---

#  Technology Stack

| Category             | Technologies             |
| -------------------- | ------------------------ |
| Programming Language | Python                   |
| Main Application     | Streamlit                |
| Landing Website      | Flask                    |
| Computer Vision      | Dlib                     |
| Face Recognition     | Face Recognition Models  |
| Machine Learning     | Scikit-learn             |
| ML Algorithm         | Support Vector Machine   |
| Voice Biometrics     | Resemblyzer              |
| Audio Processing     | Librosa                  |
| Numerical Computing  | NumPy                    |
| Data Processing      | Pandas                   |
| Database             | Supabase / PostgreSQL    |
| Password Security    | bcrypt                   |
| QR Generation        | Segno                    |
| Image Processing     | Pillow                   |
| Deployment           | Streamlit Cloud / Vercel |
| Version Control      | Git & GitHub             |

---

#  Python Dependencies

The main application uses the following dependencies:

```txt
streamlit
numpy
pandas
scikit-learn
dlib-bin
face_recognition_models
supabase
bcrypt
segno
pillow
librosa
resemblyzer
```

The complete dependencies are available in:

```text
ai-attendance-project-app/requirements.txt
```

---

#  Repository Structure

The project uses a parent repository containing two Git submodules.

```text
Intelligent-AI-Attendance-System/
│
├── README.md
├── .gitmodules
│
├── ai-attendance-project-app/
│   │
│   ├── app.py
│   ├── requirements.txt
│   ├── README.md
│   │
│   └── src/
│       │
│       ├── components/
│       │
│       ├── database/
│       │   ├── config.py
│       │   └── db.py
│       │
│       ├── pipelines/
│       │   ├── face_pipeline.py
│       │   └── voice_pipeline.py
│       │
│       ├── screens/
│       │   ├── home_screen.py
│       │   ├── teacher_screen.py
│       │   └── student_screen.py
│       │
│       └── ui/
│
└── ai-attendance-project-landing/
    │
    ├── app.py
    ├── requirements.txt
    ├── vercel.json
    │
    ├── templates/
    │   └── index.html
    │
    └── static/
        ├── css/
        └── img/
```

---

#  Installation and Local Setup

## Prerequisites

Before running SnapClass locally, make sure you have:

```text
Python 3.10+
Git
pip
A Supabase account/project
Webcam for FaceID functionality
Microphone for voice functionality
```

---

# 1️⃣ Clone the Main Repository

Because the main repository uses submodules, clone it using:

```bash
git clone --recurse-submodules https://github.com/bibekn414/Intelligent-AI-Attendance-System.git
```

Move into the repository:

```bash
cd Intelligent-AI-Attendance-System
```

If you already cloned the repository without submodules, run:

```bash
git submodule update --init --recursive
```

---

# 2️⃣ Open the Main Application

```bash
cd ai-attendance-project-app
```

---

# 3️⃣ Create a Virtual Environment

### Windows

```bash
python -m venv venv
venv\Scripts\activate
```

### Linux / macOS

```bash
python3 -m venv venv
source venv/bin/activate
```

---

# 4️⃣ Install Required Packages

```bash
pip install -r requirements.txt
```

---

# 5️⃣ Configure Supabase

The application connects to Supabase using Streamlit secrets.

Create:

```text
.streamlit/
```

Inside it, create:

```text
secrets.toml
```

The directory should look like:

```text
ai-attendance-project-app/
│
├── .streamlit/
│   └── secrets.toml
│
├── app.py
└── ...
```

Inside `secrets.toml`, add:

```toml
SUPABASE_URL = "YOUR_SUPABASE_PROJECT_URL"
SUPABASE_KEY = "YOUR_SUPABASE_API_KEY"
```

Do **not** upload real private keys to GitHub.

Add the secrets file to `.gitignore`.

Example:

```gitignore
.streamlit/secrets.toml
.env
venv/
__pycache__/
```

---

# 6️⃣ Configure the Database

Create the required tables in Supabase for:

```text
teachers
students
subjects
subject_students
attendance_logs
```

The application expects relationships between:

```text
Teacher → Subjects
Student → Subjects
Student → Attendance Logs
Subject → Attendance Logs
```

Face and voice embeddings are stored with student records.

---

# 7️⃣ Run the Streamlit Application

From:

```text
ai-attendance-project-app/
```

run:

```bash
streamlit run app.py
```

The application will normally be available locally at:

```text
http://localhost:8501
```

---

# 🌐 Running the Landing Website Locally

Move to:

```bash
cd ../ai-attendance-project-landing
```

Create or activate a virtual environment if required.

Install dependencies:

```bash
pip install -r requirements.txt
```

Run:

```bash
python app.py
```

The Flask landing website provides the public project presentation and redirects users toward the main SnapClass application.

---

# 🚀 Live Demo

### 🤖 SnapClass Application

👉 **https://snapclass.streamlit.app/**

Use the live application to explore:

* Student FaceID
* Student registration
* Teacher authentication
* Subject management
* AI attendance
* Attendance records

---

#  Application Modules

## `app.py`

The main entry point of the Streamlit application.

It controls navigation between:

```text
Home
Teacher Portal
Student Portal
```

and also handles enrollment links containing a `join-code`.

---

## `src/pipelines/face_pipeline.py`

Responsible for:

* Dlib model loading
* Face detection
* Facial landmark processing
* Face embedding generation
* SVM training
* Student prediction
* Distance-based identity verification

---

## `src/pipelines/voice_pipeline.py`

Responsible for:

* Voice preprocessing
* Resampling audio
* Speaker embedding generation
* Speaker similarity comparison
* Bulk audio segmentation
* Student identification using voice biometrics

---

## `src/database/db.py`

Provides the database-access layer for:

* Teacher registration
* Teacher authentication
* Student creation
* Subject creation
* Student enrollment
* Subject management
* Attendance insertion
* Attendance retrieval

---

## `src/screens/teacher_screen.py`

Contains the teacher-facing workflow including:

```text
Teacher Login
Teacher Registration
Teacher Dashboard
Subject Management
Face Attendance
Voice Attendance
Attendance Records
```

---

## `src/screens/student_screen.py`

Contains:

```text
Student FaceID Login
Student Registration
Face Enrollment
Voice Enrollment
Subject Enrollment
Attendance Statistics
Student Dashboard
```

---

#  Why Multi-Modal Attendance?

A single biometric method may not work perfectly in every environment.

For example:

### Face Recognition

Works well when:

* Classroom images are clear
* Faces are visible
* Lighting conditions are reasonable

### Voice Recognition

Can provide an alternative when:

* Facial images cannot be captured properly
* Students participate in a roll-call workflow
* Audio-based identity verification is preferred

Combining both methods makes the system more flexible than a traditional single-method attendance solution.

---

#  Data Science & Machine Learning Concepts Used

The project applies several Data Science and ML concepts, including:

### Feature Extraction

Raw images and audio are transformed into numerical embeddings.

```text
Image → Face Embedding
Audio → Voice Embedding
```

### Classification

A Support Vector Machine maps facial embeddings to registered student IDs.

### Similarity Analysis

Voice identity is determined using similarity between embedding vectors.

### Threshold-Based Decision Making

Both biometric pipelines use thresholds to determine whether a detected identity should be accepted.

### Data Processing

Pandas is used to organize and summarize attendance records.

### Feature Storage

Instead of storing and repeatedly analyzing only raw media, biometric embeddings are stored in the database for efficient comparison.

---

#  Example Use Case

Consider a classroom containing 50 students.

Instead of calling each student's name individually, the teacher can:

```text
1. Open SnapClass
2. Login to Teacher Dashboard
3. Select the required subject
4. Upload classroom images
5. Click "Run Face Analysis"
6. AI detects registered students
7. System compares them with the subject roster
8. Present/Absent status is generated
9. Teacher reviews the result
10. Attendance is saved
```

The recorded session can later be viewed from the teacher's Attendance Records section.

---

#  Future Improvements

Potential extensions of SnapClass include:

* Anti-spoofing / liveness detection
* Improved multi-face recognition models
* Deep-learning-based face classifiers
* More robust speaker diarization
* Automatic duplicate attendance prevention
* Attendance percentage dashboards
* Student attendance alerts
* Faculty analytics dashboard
* Email notifications
* Mobile application integration
* Role-based administrative dashboard
* Timetable integration
* Automated low-attendance alerts
* Multi-institution support
* Advanced visualization using Power BI
* Docker-based deployment
* REST API using FastAPI

---

#  Limitations

Like most biometric systems, recognition performance may be affected by:

* Poor image quality
* Extreme lighting
* Occluded faces
* Large variations in camera angle
* Background noise
* Very short voice samples
* Microphone quality
* Similar speaker characteristics
* Limited biometric enrollment data

The current project is designed primarily as an AI/ML engineering and attendance-management prototype rather than a production-grade biometric security system.

---

#  Privacy Considerations

Face and voice embeddings represent biometric information and should be handled securely.

For real-world deployment, the application should include:

* Explicit user consent
* Data-retention policies
* Secure database permissions
* Encrypted communication
* Controlled biometric access
* Secure API key handling
* Appropriate institutional privacy policies

Raw biometric data and secret credentials should never be exposed publicly.

---

#  Skills Demonstrated

This project demonstrates practical experience with:

```text
Python
Machine Learning
Computer Vision
Face Recognition
Speaker Recognition
Biometric Authentication
Feature Engineering
Vector Embeddings
Support Vector Machines
Similarity Modeling
Audio Processing
Data Processing
Pandas
NumPy
Scikit-learn
Dlib
Librosa
Resemblyzer
Streamlit
Flask
Supabase
PostgreSQL
Cloud Deployment
Git
GitHub
```

---

#  Project Highlights

* Developed an end-to-end AI attendance management platform integrating **Computer Vision and Voice Biometrics**.
* Implemented facial identification using **128-dimensional Dlib face embeddings and SVM classification**.
* Developed speaker identification using **Resemblyzer embeddings and similarity-based matching**.
* Built independent **teacher and student dashboards** using Streamlit.
* Implemented **FaceID-based student authentication**.
* Developed classroom photo processing for automatic multi-student attendance detection.
* Integrated **Supabase PostgreSQL** for cloud-based storage of students, teachers, subjects and attendance records.
* Implemented **bcrypt password hashing** for teacher authentication.
* Added subject enrollment and QR/share-code based course workflows.
* Built a separate **Flask landing website** for product presentation and application access.

---

#  Author

**Bibek Nayak**

Chemical Engineering Undergraduate
National Institute of Technology, Rourkela

Interested in:

```text
Data Science
Machine Learning
Artificial Intelligence
Data Analytics
Computer Vision
Deep Learning
```

GitHub:

**https://github.com/bibekn414**

---

#  Support

If you found this project useful or interesting, consider giving the repository a ⭐.

It helps support continued development and improvement of the project.

---

<p align="center">
  <b>SnapClass — Making Attendance Faster with AI.</b>
</p>
