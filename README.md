# MonReader
## Introduction
**Background:**
- Our company develops innovative Artificial Intelligence and Computer Vision solutions that revolutionize industries. Machines that can see: We pack our solutions in small yet intelligent devices that can be easily integrated to your existing data flow. Computer vision for everyone: Our devices can recognize faces, estimate age and gender, classify clothing types and colors, identify everyday objects and detect motion. Technical consultancy: We help you identify use cases of artificial intelligence and computer vision in your industry. Artificial intelligence is the technology of today, not the future.

- MonReader is a new mobile document digitization experience for the blind, for researchers and for everyone else in need of fully automatic, very fast and high-quality document scanning in bulk. It is composed of a mobile app and all the user needs to do is flip pages and everything is handled by MonReader: it detects page flips from low-resolution camera preview and takes a high-resolution picture of the document, recognizing its corners and crops it accordingly, and it dewarps the cropped document to obtain a bird's eye view, sharpens the contrast between the text and the background and finally recognizes the text with formatting kept intact, being further corrected by MonReader's ML powered redactor.

**Data Description:**
- We collected page flipping video from smart phones and labelled them as flipping and not flipping.
- We clipped the videos as short videos and labelled them as flipping or not flipping. The extracted frames are then saved to disk in a sequential order with the following naming structure: VideoID_FrameNumber

**Download Data:**
- https://drive.google.com/file/d/1KDQBTbo5deKGCdVV_xIujscn5ImxW4dm/view?usp=sharing

**Goal(s):**
- Predict if the page is being flipped using a single image.

**Success Metrics:**
- Evaluate model performance based on F1 score, the higher the better.

**Bonus(es):**
- Predict if a given sequence of images contains an action of flipping.

## Project structure
```
MR/
├── images/
│   ├── training/{flip, notflip}/   # training frames (download separately)
│   └── testing/{flip, notflip}/    # held-out frames
├── models/                         # saved Keras weights (*.weights.h5)
├── notebooks/
│   └── notebook_mr.ipynb           # main analysis notebook
├── requirements.txt
└── README.md
```
The repository ships with a small sample of frames so the notebook can be
inspected; the full dataset must be downloaded from the link above and
extracted into `images/` before training.

## Setup
```bash
python -m venv .venv
# Windows:  .venv\Scripts\activate
# macOS/Linux:  source .venv/bin/activate
pip install -r requirements.txt
```

## Running the notebook
```bash
jupyter lab notebooks/notebook_mr.ipynb
```
All paths inside the notebook are relative to the `notebooks/` folder
(e.g. `../images/training`), so launch Jupyter from the project root.

## Models trained
Three classifiers are compared in the notebook. On the current data split
all three exceed 95% test F1, so the choice between them is dominated by
model size more than by accuracy:

| Model        | Test F1 (latest run) | Saved size |
|--------------|----------------------|------------|
| Custom CNN   | ~0.9804 (best)       | ~43 MB     |
| ResNet50     | ~0.9676              | ~102 MB    |
| MobileNetV2  | ~0.9592              | ~17 MB     |

MobileNetV2's weights (`models/mobilenet_model.weights.h5`) are the
checked-in artifact: it gives competitive F1 at roughly 1/6 the size of
the custom CNN and 1/6 the size of ResNet50, which is the right
trade-off for the on-device, mobile-app deployment MonReader targets.
Re-running the notebook regenerates the other two artifacts locally.
