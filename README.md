# Acoustic-Echo-Cancellation-using-PBFDA-filter

This project implements Acoustic Echo Cancellation (AEC) in MATLAB using a custom-designed Graphical User Interface (GUI).
The system loads an audio signal containing echo, applies an adaptive frequency-domain filter, and produces a cleaned (echo-removed) output signal.

The GUI visualizes both time-domain and frequency-domain signals, including a stick-like frequency spectrum with peak markers, similar to a spectrum analyzer.

## Features

- Load any `.wav` or `.mp3` audio file

- Visualize:
  - ✔ **Original signal (time domain)**
  - ✔ **Original spectrum (frequency domain – stick spectrum)**
  - ✔ **Echo-cancelled signal (time domain)**
  - ✔ **Processed spectrum (frequency domain – stick spectrum)**

- Playback:
  - ✔ **Play original audio**
  - ✔ **Play echo-cancelled output**

- Adaptive echo cancellation using:
  - **Frequency-domain filtering**
  - **NLMS coefficient updates**

- **Auto-scaling frequency axis (±200 Hz view)**

- **Clean, interactive MATLAB UI**

- ## Applications

- **Hands-free communication systems**  
  Removes echo in speakerphone mode for clearer voice transmission.

- **Video conferencing platforms**  
  Improves call quality by eliminating room echo and feedback.

- **Hearing aids & assistive listening devices**  
  Reduces environmental echo to enhance speech intelligibility.

- **Voice-controlled smart devices**  
  Helps microphones isolate clean speech from reflected echoes.

- **Audio recording & post-processing**  
  Cleans up recordings made in echo-prone rooms or halls.

- **Automotive infotainment systems**  
  Suppresses cabin echo in in-car voice commands and calls.

- **Telepresence robots & IoT communication nodes**  
  Ensures accurate voice pickup in real-time remote operations.

- **Virtual reality & augmented reality audio**  
  Improves immersion by cleaning unwanted echo artifacts.

- **Broadcasting and live streaming**  
  Removes acoustic feedback for clearer voice transmission.

- **DSP education and research**  
  Demonstrates real-time adaptive filtering and NLMS behavior.

