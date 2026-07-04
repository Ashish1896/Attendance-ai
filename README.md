# 📸 SnapClass (Version 2.0.0)
### *AI-Powered Attendance Tracking System*

SnapClass is an intelligent, secure, and modern class attendance management application that integrates **AI-driven Face Recognition** and dynamic **QR Code Verification**. Built specifically for educational institutions, it eliminates manual roll calls and buddy punching, providing a seamless check-in experience for students and robust management tools for teachers.

---

## 🌟 Core Features

### 👨‍🏫 Teacher Portal
- **Create & Manage Subjects:** Organize classes by course codes, sections, and semesters.
- **Dynamic QR Code Sessions:** Start live attendance sessions displaying a secure, temporary QR code that students must scan to register.
- **Real-time Face-Matching Verification:** Verify student check-ins using processed face matches.
- **Attendance Records & Exports:** View detailed attendance history, search logs, and export reports directly to CSV/Excel formats.
- **Student Management:** View enrolled lists, add students, or unenroll them with one-click actions.

### 🧑‍🎓 Student Portal
- **Instant Enrollment:** Join new subjects easily using class invite codes shared by the teacher.
- **AI Face Profile Registration:** Students enroll by uploading 3 clear photos. The pipeline extracts 128-dimensional facial feature vectors using **dlib** and trains an **SVM (Support Vector Machine) classifier** on-the-fly.
- **Secure Log-in & Double-Factor Check-in:**
  1. Scan the teacher's active session QR code.
  2. Perform real-time camera face verification.

---

## 🛠️ Technology Stack

- **User Interface:** [Streamlit](https://streamlit.io/) with customized CSS injections for base layout styles.
- **AI/ML Pipelines:**
  - **Face Detection:** Dlib frontal face detector.
  - **Embedding Generator:** Dlib face recognition model v1 (128-d face descriptors).
  - **Classifier:** Scikit-learn's Support Vector Classifier (SVC) with linear kernel.
- **Database & Auth:** [Supabase](https://supabase.com/) (PostgreSQL backend for secure data storage).
- **Utility Libraries:**
  - **Segno:** High-fidelity QR code generation.
  - **Pillow & OpenCV:** Image processing, normalization, and lighting/contrast adjustments (CLAHE).

---

## 🚀 Installation & Setup

### Prerequisites
- Python 3.9 - 3.11 (Python 3.10 recommended)
- Git

### 1. Clone the Repository
```bash
git clone https://github.com/Ashish1896/Attendance.git
cd Attendance
```

### 2. Set Up a Virtual Environment
```bash
# Create a virtual environment
python -m venv venv

# Activate on Windows (cmd/PowerShell)
.\venv\Scripts\activate

# Or activate on macOS/Linux
source venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```
*Note: If installing `dlib` poses compiler issues on your machine, the `requirements.txt` relies on `dlib-bin` to fetch precompiled binaries. Make sure you have C++ Build Tools installed if you build from source.*

### 4. Database Setup & Streamlit Secrets
1. Create a project on [Supabase](https://supabase.com/).
2. Create a folder named `.streamlit` in the root directory (already ignored in `.gitignore`).
3. Create a `secrets.toml` file inside `.streamlit`:
   ```toml
   [supabase]
   url = "https://your-supabase-project.supabase.co"
   key = "your-anon-role-key-jwt"
   ```

### 5. Run the Application
```bash
streamlit run app.py
```

---

## 📂 Project Structure

```
Attendance/
├── .streamlit/
│   └── secrets.toml          # Database keys (Git ignored)
├── src/
│   ├── assets/               # Local static images & logos
│   ├── components/           # Reusable UI elements & dialog modals
│   │   ├── qr_scanner/       # Custom web RTC camera QR reader component
│   │   └── dialog_enroll.py  # Dialogs for class interactions
│   ├── database/
│   │   ├── config.py         # Supabase client instantiation
│   │   └── db.py             # Database CRUD helper queries
│   ├── pipelines/
│   │   ├── face_pipeline.py  # Face embeddings and SVC training pipeline
│   │   └── qr_pipeline.py    # QR code session verification logic
│   ├── screens/
│   │   ├── home_screen.py    # Main landing portal selector
│   │   ├── teacher_screen.py # Teacher flows, reports, and session control
│   │   └── student_screen.py # Student enrollments, QR scans, and face check-in
│   └── ui/
│       └── base_layout.py    # Global CSS injection styling scripts
├── app.py                    # Application entry point
├── requirements.txt          # Package dependencies
└── README.md                 # Project documentation
```

---

## 🔒 Security & Privacy Features
- **Single Session Checker:** Protects accounts from multiple logins on different devices by maintaining active sessions.
- **Git Shielding:** `.gitignore` blocks deployment of API credentials (`secrets.toml`), build caches (`__pycache__`), and local state stores (`active_sessions.json`).