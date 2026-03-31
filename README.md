PROJECT DESCRIPTION

This project is an advanced, multi-functional Computer Vision application built in Python. It operates by capturing a live video feed from the user's webcam and simultaneously processing two distinct computer vision pipelines: Drowsiness Detection and an Interactive Air Canvas with an embedded Math Solver.

The application is designed to process frames at a capped rate of 15 frames per second to optimize system resources while maintaining real-time responsiveness.

Pipeline 1: Drowsiness Detection
This system is designed as a safety mechanism. It utilizes the Dlib library and a pre-trained 68-point facial landmark predictor to map the user's face. The algorithm isolates the regions corresponding to the left and right eyes. It calculates the Euclidean distance between specific vertical and horizontal eye landmarks to compute the Eye Aspect Ratio (EAR). If the EAR drops below a set threshold (0.25) for 20 consecutive frames, the system concludes that the user's eyes are closed or heavy. It will then trigger an on-screen warning and utilize Pygame to loop an audio alarm until the user opens their eyes and the EAR returns to a normal level.

Pipeline 2: Air Canvas and OCR Math Solver
This feature transforms the webcam feed into a virtual whiteboard. It utilizes Google's MediaPipe framework to detect and track the user's hand landmarks in real-time. By tracking the tip of the index finger (landmark 8) and the tip of the thumb (landmark 4), the application determines the user's intent. If the thumb and index finger are close together, drawing is paused. If the index finger is raised and the thumb is separated, the application records the finger's coordinates and draws lines on both the live video feed and a hidden white canvas.

The user can interact with a virtual toolbar at the top of the screen to change paint colors (Blue, Green, Red, Yellow) or clear the canvas. Every 30 frames, the application captures the hidden white canvas, applies image thresholding using OpenCV to isolate the drawn text, and passes it to the Tesseract Optical Character Recognition (OCR) engine. If Tesseract detects a mathematical expression (such as "2+2"), the Python script evaluates the expression and projects the calculated result back onto the live video feed.

PROJECT FILE: requirements.txt

opencv-python
numpy
mediapipe
dlib
scipy
pygame
pytesseract

README.txt

MULTI-CV ASSISTANT: AIR CANVAS AND DROWSINESS DETECTOR
Comprehensive Documentation

OVERVIEW
This software is a dual-purpose Python application. It serves as an active safety monitor by detecting signs of sleepiness, and as a productivity tool by allowing users to handwrite math equations in the air and have the computer automatically solve them.

HOW IT WORKS: TECHNICAL BREAKDOWN

    Facial Landmark Mapping
    The application converts the video frame to grayscale and shrinks it to speed up processing. Dlib scans the frame to find a face. Once a face is found, the shape_predictor_68_face_landmarks.dat model identifies 68 specific points on the face. Points 36-41 map the left eye, and points 42-47 map the right eye.

    The Eye Aspect Ratio (EAR)
    The EAR is a mathematical formula that compares the width of the eye to the height of the eye. When a person blinks or closes their eyes, the height drops to almost zero, causing the EAR value to plummet. The application constantly averages the EAR of both eyes.

    Hand Tracking and Deques
    MediaPipe maps 21 3D landmarks on the user's hand. The application focuses on the coordinates of the index finger. To draw smooth lines, the application stores the history of the finger's positions in a data structure called a "deque" (double-ended queue). As the finger moves, the application draws lines connecting the historical points in the deque.

    OCR Text Extraction
    To read the math equations, the application maintains a separate, pure-white virtual window where only the drawn lines are recorded. Every 30 frames, it converts this white window to grayscale, increases the contrast (thresholding), and asks Tesseract to read the text. The extracted text is then passed to Python's native eval() function to compute the math.

PREREQUISITES AND SYSTEM REQUIREMENTS

    Python version 3.7 or newer.

    A standard webcam (integrated or USB).

    C++ Build Tools (Required for installing Dlib on Windows).

    Tesseract OCR Engine. This is a separate software installed on your operating system, not just a Python package.

        Windows users must download the Tesseract installer from GitHub and add it to their system PATH.

        Mac users can install it via terminal using: brew install tesseract

        Linux users can install it via terminal using: sudo apt-get install tesseract-ocr

INSTALLATION INSTRUCTIONS

Step 1: Set up the project folder
Create a new folder on your computer and place the Python script (e.g., app.py) inside it.

Step 2: Install Python Packages
Open your command prompt or terminal, navigate to your project folder, and run the following command to install the necessary libraries:
pip install -r requirements.txt

Step 3: Download External Assets
The application requires two external files to function.

    You need the facial landmark model. Search for "shape_predictor_68_face_landmarks.dat" online, download it, and place it in your project folder.

    You need an audio file for the alarm. Find any short .mp3 or .wav sound effect (like a car horn or a digital beep) and place it in your project folder.

Step 4: Configure the Code Paths
Open the Python script in a text editor. You must manually update the file paths so the script knows where to find your assets. Locate these two variables near the top of the script and update the strings to match the exact locations on your computer:

    ALERT_SOUND = "C:/path/to/your/alarm.mp3"

    predictor = dlib.shape_predictor("C:/path/to/your/shape_predictor_68_face_landmarks.dat")

USAGE GUIDE

    Start the application by running the script in your terminal: python app.py

    Two windows will open: The main "Output" window showing your webcam feed, and a "Paint" window showing the raw canvas.

    Drowsiness Detector: This runs automatically in the background. If you close your eyes for more than a second, the alarm will trigger.

    Drawing: Raise your hand into the camera frame. Point your index finger straight up. Moving your finger will draw on the screen.

    Pausing: To move your hand without drawing a line, pinch your thumb and index finger together, or lower your hand completely out of the camera's view.

    Changing Colors: At the top of the video feed are rectangular boxes for Clear, Blue, Green, Red, and Yellow. Move your index finger into one of these boxes to select that color or clear the screen.

    Math Solver: Draw a simple math equation on the screen using your finger (for example, 5+5). Wait a moment, and the text "Result: 10" will appear on the screen.

    Exiting: Ensure the video window is selected, then press the 'q' key on your keyboard to close the application safely.

KNOWN LIMITATIONS AND TROUBLESHOOTING

    Lighting: Both the facial detector and the hand tracker require good lighting. If you are in a dark room, the application will struggle to find your face or hand, resulting in erratic behavior.

    OCR Accuracy: Handwriting in the air is difficult, and Tesseract may misread messy numbers. If the math solver fails, clear the screen and try writing the numbers larger and more clearly.

    Security Warning: The application uses the eval() function to solve the math. This function will execute whatever text it reads as Python code. Do not attempt to write complex system commands on the canvas.
