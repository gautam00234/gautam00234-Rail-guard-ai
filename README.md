# RailGuard AI

RailGuard AI is a college-project prototype that analyses recorded station video for two warning signals:

- pedestrian crowd density; and
- movement within a configurable track-exclusion band at the bottom of the video.

It uses YOLO11n, a pretrained deep-learning person detector, plus OpenCV video processing. A small Kaggle pedestrian dataset is included through a reproducible conversion script for future fine-tuning. This is a working academic prototype, not a safety-certified railway system.

## Run locally

Use Python 3.10 or later from this folder:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
uvicorn app.main:app --reload
```

Then open `http://127.0.0.1:8000` and upload a short station/platform video.

After the first setup, the shortest way to launch it is:

```powershell
.\start.ps1
```

## Use in VS Code

1. In VS Code, select **File > Open Folder**.
2. Open this `RailGuard-AI` folder.
3. Open the integrated terminal and complete the one-time setup above.
4. Select **Terminal > Run Task > Run RailGuard AI**. The dashboard opens at `http://127.0.0.1:8000`.

## Important prototype limits

The lowermost 18% of every uploaded frame is treated as the track area. This is a visual demo assumption, not actual track recognition. Before any production setting, use authorized cameras, define camera-specific safe zones, validate performance across lighting/crowd conditions, protect video data, and retain trained human control over every safety decision.

## Dataset and training evidence

The downloaded Kaggle pedestrian dataset is converted into 187 labelled person frames: 148 training and 39 validation frames. The CSV-to-YOLO conversion is reproducible:

```powershell
python training\convert_kaggle_pedestrian.py --sample-every 8
```

To fine-tune later on a GPU-enabled Python environment, run:

```powershell
.\training\train_kaggle_pedestrian.ps1 -Epochs 10
```

The dashboard currently uses the included pretrained `models/yolo11n.pt` baseline. Do not claim a fine-tuning accuracy value until a run completes and produces `results.csv`.
