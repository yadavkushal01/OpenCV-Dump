# OpenCV 🖼️

A hands-on collection of OpenCV experiments and implementations covering core computer vision concepts — from fundamental image processing to real-time face and object detection using Haar Cascades.

---

## 📁 Repository Structure

```
OpenCV-Dump/
└── OpenCV/
    ├── images/                   # Sample images used for testing
    ├── opencv.ipynb              # Part 1: OpenCV fundamentals & image processing
    ├── OpenCV_2.ipynb            # Part 2: Face & object detection with Haar Cascades
    ├── haarcascade_car.xml                   # Pre-trained car detector
    ├── haarcascade_eye.xml                   # Pre-trained eye detector
    ├── haarcascade_frontalface_default.xml   # Pre-trained face detector
    ├── haarcascade_fullbody.xml              # Pre-trained full body detector
    └── haarcascade_smile.xml                 # Pre-trained smile detector
```

---

## 📓 Notebook 1 — `opencv.ipynb` : OpenCV Fundamentals

Covers the core building blocks of image processing using OpenCV and NumPy.

### Topics Covered

| Topic | What it does |
|---|---|
| Reading & displaying images | `cv2.imread()`, `cv2.imshow()`, `cv2.waitKey()` |
| Color space conversion | BGR → Grayscale, BGR → HSV using `cv2.cvtColor()` |
| Image resizing & cropping | `cv2.resize()`, NumPy array slicing |
| Drawing on images | `cv2.rectangle()`, `cv2.circle()`, `cv2.putText()` |
| Blurring & smoothing | Gaussian blur, median blur — noise reduction |
| Edge detection | Canny edge detector — `cv2.Canny()` |
| Thresholding | Binary, Otsu's thresholding — `cv2.threshold()` |
| Contour detection | `cv2.findContours()`, `cv2.drawContours()` |
| Morphological operations | Erosion, dilation, opening, closing |
| Image arithmetic | Addition, subtraction, bitwise operations |

### Key Concepts

**Why BGR and not RGB?**
OpenCV loads images in BGR (Blue-Green-Red) channel order by default — the opposite of most other libraries like matplotlib or PIL. Always convert with `cv2.cvtColor(img, cv2.COLOR_BGR2RGB)` before displaying with matplotlib.

**Canny Edge Detection — how it works:**
1. Gaussian blur to remove noise
2. Compute intensity gradient using Sobel filters
3. Non-maximum suppression to thin edges
4. Double threshold — strong, weak, and non-edges
5. Edge tracking by hysteresis — keeps only connected strong edges

**Thresholding:**
Converts a grayscale image to binary (black/white). Otsu's method automatically finds the optimal threshold value — useful when you don't know the right cutoff in advance.

---

## 📓 Notebook 2 — `OpenCV_2.ipynb` : Haar Cascade Detection

Covers real-time object detection using pre-trained Haar Cascade classifiers.

### Topics Covered

| Topic | What it does |
|---|---|
| Loading Haar cascades | `cv2.CascadeClassifier()` with `.xml` files |
| Face detection | `detectMultiScale()` on grayscale frames |
| Eye detection | Nested detection — eyes found within detected face ROI |
| Drawing bounding boxes | `cv2.rectangle()` around detected regions |
| Webcam / video capture | `cv2.VideoCapture()`, frame-by-frame processing loop |
| ROI (Region of Interest) | Cropping detected face area for further processing |
| Parameter tuning | `scaleFactor`, `minNeighbors`, `minSize` |

### Key Concepts

**What is a Haar Cascade?**
A machine learning based approach proposed by Viola and Jones (2001). The classifier is trained on thousands of positive (face) and negative (non-face) images. It uses "Haar features" — rectangular patterns that capture edges, lines, and texture — and a cascade of classifiers that quickly rejects non-face regions, making detection fast enough for real-time use.

**The `.xml` files:**
These are pre-trained classifier models provided by OpenCV. You load them instead of training from scratch:
```python
face_cascade = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')
eye_cascade  = cv2.CascadeClassifier('haarcascade_eye.xml')
```

**`detectMultiScale()` parameters explained:**
```python
faces = face_cascade.detectMultiScale(
    gray,
    scaleFactor=1.3,    # How much image is reduced at each scale (1.3 = 23% reduction)
    minNeighbors=5,     # How many neighbors each candidate rectangle needs — higher = fewer false positive
)
```

**Detection pipeline:**
```
Input frame
    → Convert to Grayscale           (detection works on intensity, not color)
    → detectMultiScale() on full image   (find faces → returns list of (x, y, w, h))
    → For each face ROI
        → detectMultiScale() again       (find eyes within face region)
    → Draw rectangles on original color frame
    → Display result
```

---

## 🛠️ Tech Stack

| Library | Version | Purpose |
|---|---|---|
| `opencv-python` | 4.x | Core computer vision operations |
| `numpy` | latest | Image array manipulation |
| `matplotlib` | latest | Inline image display in notebooks |
| `Jupyter Notebook` | latest | Interactive development environment |

---

## ⚙️ Setup & Installation

```bash
# Clone the repository
git clone https://github.com/yadavkushal01/OpenCV-Dump.git
cd OpenCV-Dump/OpenCV

# Install dependencies
pip install opencv-python numpy matplotlib jupyter

# Launch notebooks
jupyter notebook
```

---

## 🚀 How to Run

### For image processing (opencv.ipynb):
Just run all cells sequentially. Sample images from the `images/` folder are used throughout.

### For face detection (OpenCV_2.ipynb):
Make sure the `.xml` Haar cascade files are in the same directory as the notebook, then run. For webcam-based detection:
```python
cap = cv2.VideoCapture(0)   # 0 = default webcam
```
Press `k` to quit the video window.

---

## 💡 Concepts Demonstrated

- **Image as a NumPy array** — every image is a matrix of pixel values; understanding this is fundamental to all CV work
- **Color spaces** — BGR, RGB, Grayscale, HSV each serve different purposes (HSV is better for color-based detection)
- **Cascade classifiers** — a fast, practical approach to object detection without deep learning
- **ROI-based processing** — processing sub-regions of an image (e.g. finding eyes only inside a detected face)
- **Real-time video processing** — reading frames in a loop and applying operations frame-by-frame

---

## 📌 Limitations & What Could Be Improved

- Haar Cascades are sensitive to lighting conditions and face angle — they work best with frontal, well-lit faces
- Deep learning-based detectors (like MTCNN or OpenCV's DNN module with a pre-trained SSD/YOLO model) give much better accuracy
- Could be extended with face recognition (not just detection) using `face_recognition` library or FaceNet embeddings

---

## 👤 Author

**Kushal Yadav**
B.Tech — [Dr.Akhilesh Das Gupta Institute Of Professional Stuides]

[GitHub](https://github.com/yadavkushal01)
[LinkedIn](https://www.linkedin.com/in/kushal-yadav-067ab4334/)

---

*Part of a computer vision learning series. Notebooks document hands-on exploration of OpenCV during academic coursework and self-study.*
