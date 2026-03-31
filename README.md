# Multi-CV Assistant: Air Canvas and Drowsiness Detector

## Overview
This repository contains a Python application that uses a webcam to monitor user drowsiness and provides a virtual drawing interface capable of solving handwritten math equations.

## Features
* Drowsiness Detection: Monitors the user's Eye Aspect Ratio (EAR) and plays an audio alarm if sleepiness is detected.
* Air Canvas: Tracks index finger movements via MediaPipe to draw directly on the video feed.
* Math Solver: Reads the virtual canvas using Tesseract OCR and evaluates mathematical strings in real-time.

## Prerequisites
* Python 3.7+
* A working webcam
* Tesseract OCR engine installed directly on your operating system (the Python package alone is insufficient).

## Installation
1. Clone this repository to your local machine.
2. Install the required Python dependencies:
   pip install -r requirements.txt
3. Download `shape_predictor_68_face_landmarks.dat` from the Dlib model repository and place it in the project directory.
4. Add an audio file (e.g., `beep.mp3` or `alarm.wav`) to the project directory to serve as the wake-up alarm.
5. Open `index.py` and update the `ALERT_SOUND` and `predictor` variables with the absolute file paths to your audio and .dat files.

## Usage
Start the application by running the following command in your terminal:

python index.py

## Controls
* Draw: Raise your hand and point your index finger upwards.
* Pause Drawing: Pinch your thumb and index finger together, or drop your hand out of the camera frame.
* Change Color / Clear Canvas: Hover your index finger over the respective rectangular buttons at the top of the video feed.
* Quit: Select the video window and press the 'q' key.

## Important Notes
* The Math Solver uses Python's `eval()` function to compute equations. Do not attempt to draw complex code or system commands.
* Ensure you are in a well-lit environment for optimal facial landmark and hand tracking accuracy.
