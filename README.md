# Attendance Logging System Using Facial Recognition

A desktop attendance logging application that uses facial recognition to identify students and record their attendance in Excel files. Built with Python, OpenCV, and the `face_recognition` library.

## Features

- **Student Registration** — Register new students by capturing face images via webcam or uploading photos manually. Images are stored per-student and encoded into a shared pickle file for recognition.
- **Face-Based Login** — Students log in by looking into the camera. The system captures video for ~15 seconds, identifies the most frequently recognized face across all frames, and logs that student's attendance.
- **Attendance Logging** — Each login is recorded in a date-stamped Excel file (`attendance_logs/YYYY-MM-DD.xlsx`) with the student's name and login time.
- **GUI** — Simple Tkinter-based interface for registration, image management, and login.

## Project Structure

```
├── main.py                        # Entry point
├── main_gui.py                    # Tkinter GUI
├── encoding_module.py             # Student registration, image capture, and face encoding
├── facial_recognition_module.py   # Face recognition and attendance logging
├── dataset/                       # Student face images (one folder per student)
├── attendance_logs/               # Date-stamped Excel attendance records
└── misc_files/
    ├── encodings.pickle           # Serialized face encodings
    ├── processed_folders.txt      # Tracks which students have been encoded
    ├── deploy.prototxt.txt        # Caffe model architecture (face detection)
    └── res10_300x300_ssd_iter_140000.caffemodel  # Pre-trained face detection model
```

## Requirements

- Python 3.8+
- macOS, Linux, or Windows
- A webcam

### Python Dependencies

```
face_recognition
opencv-python
pandas
imutils
numpy
openpyxl
```

Install all dependencies:

```bash
pip install face_recognition opencv-python pandas imutils numpy openpyxl
```

> **Note:** `face_recognition` requires `dlib`, which is compiled from source during installation. On macOS, you may need CMake installed (`brew install cmake`).

## Usage

```bash
python main.py
```

### Registering a New Student

1. Click **Register** and enter the student's first and last name.
2. In the Encoding Menu:
   - **Pose for image collection** — Opens the webcam and automatically captures 30 snapshots of your face. Slowly rotate your head for varied angles.
   - **Manually upload images** — Select image files from your computer.
   - **Crop your images** — Detects and crops faces from uploaded images for cleaner encoding.
   - **Encode new images** — Processes all new images and adds the face encodings to the pickle file.

### Logging Attendance

1. Click **Login** and then **Ready**.
2. Look into the camera for ~15 seconds.
3. The system identifies you and records your attendance in the corresponding date's Excel file.

> **Note:** Make sure the Excel file for today's date is closed before logging in, otherwise the program won't be able to write to it.

## How It Works

1. **Face Detection** uses a pre-trained Caffe SSD model (`res10_300x300_ssd_iter_140000.caffemodel`) to locate faces in video frames during registration.
2. **Face Encoding** uses the `face_recognition` library (powered by dlib) to generate 128-dimensional face embeddings, stored in `misc_files/encodings.pickle`.
3. **Face Recognition** compares live face encodings against stored encodings using `face_recognition.compare_faces()` with a CNN model. The most frequently matched name across all captured frames is selected as the identified student.

## License

This project was created for academic purposes (CS124P M2 Assessment).
