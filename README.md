# 🚦 EOTRNet: Smart Traffic Security & Surveillance System

EOTRNet watches traffic footage, tracks every vehicle, and flags the ones driving **the wrong way**. Each violation is saved to a database so it can be reviewed and analysed later.

Built with **YOLOv8n** (fine-tuned via transfer learning), **ByteTrack** multi-object tracking, **Streamlit**, and **SQLite**.

## ✨ What the app does

The Streamlit app has four tabs:

| Tab | What it's for |
|---|---|
| 🎯 **Calibration** | Grab a frame from the video source and draw the allowed direction lines for each lane |
| 📡 **Live Monitor** | Run detection + tracking on the feed and count correct-way vs wrong-way crossings in real time |
| 📊 **Analytics** | Charts of traffic and violation trends from the stored records |
| 📂 **Tracking Logs** | Browse every logged crossing and violation |

Supported video sources: a webcam, a local video file, or a YouTube stream URL.

## 🧠 How it works

1. **Detection:** a YOLOv8n model fine-tuned on vehicle data runs every few frames at a reduced resolution, so inference stays fast enough for real-time video.
2. **Tracking:** ByteTrack keeps a stable ID on each vehicle across frames, even when detections briefly drop.
3. **Direction check:** each calibrated line is a `supervision` line zone wrapped in a depth-confirmation buffer, so a vehicle only counts as crossing once it is clearly past the line. Crossing against the allowed direction is a violation.
4. **Logging:** crossings and violations are written to SQLite for later monitoring, analysis and evaluation.

The `Notebooks/` folder compares a **baseline** detector (5-fold cross-validation) against the **transfer learning** approach (warm-up + fine-tune), with the training and evaluation results in `Baseline Evaluation/` and `Transfer Learning/`.

## 🚀 Run it

**Locally**

```bash
pip install -r requirements.txt
streamlit run "Working Application/App.py"
```

**With Docker** (the image serves on port 7860)

```bash
docker build -t eotrnet .
docker run -p 7860:7860 eotrnet
```

## 📁 Repository structure

```
Application Model/          final YOLOv8n weights used by the app
Working Application/App.py  the Streamlit application
Notebooks/                  EDA, baseline and transfer-learning experiments
Baseline Evaluation/        baseline training curves and test results
Transfer Learning/          warm-up and fine-tune results
Data/                       sample video clips + SQLite database
```

## 👥 Team

University project at BINUS University.

- **Raditya Arya Pradipta** ([@Raxzien](https://github.com/Raxzien)): code and development
- **Justin Christian** ([@utinn](https://github.com/utinn)): teammate, first published the project at [utinn/EOTRNet](https://github.com/utinn/EOTRNet---Smart-Traffic-Security-and-Surveillance-System)
