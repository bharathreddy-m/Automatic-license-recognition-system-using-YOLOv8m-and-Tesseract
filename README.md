# Automated License Plate Recognition System using YOLOv8 and OCR

This project is an end-to-end **Automated License Plate Recognition (ALPR) System** that uses **YOLOv8** for object detection and **OCR (EasyOCR)** for text extraction. It includes database integration for identifying lost or flagged vehicles and raises alerts when matches are found.

---

## 🔍 Project Overview
The ALPR system processes images (or video frames), detects license plates, extracts text using OCR, and matches it with entries in a database of known license plates (e.g., lost/stolen vehicles). It can be used in real-time surveillance applications.

---

## 📁 Folder Structure
ALPR_Project/
├── yolov8_detection/         # YOLOv8 model training and detection
├── cropped_plates/           # Cropped plate images
├── extracted_texts.txt       # OCR results from Tesseract
├── database.db               # SQLite database for license plates
├── app.py                    # Flask app for adding lost vehicles
├── alpr_main_pipeline.ipynb  # Jupyter notebook for main pipeline
└── README.md
---

## ⚙️ Components and Workflow

### 1. **License Plate Detection (YOLOv8)**
- Trained **YOLOv8m** model on a license plate dataset using Roboflow.
- Training configuration:
  - **Image Size**: 416x416
  - **Epochs**: 40
- **Model Summary**:
  - 72 layers, 3,005,843 parameters, 8.1 GFLOPs
- Achieved excellent results:

#### 📊 YOLOv8m License Plate Detection Metrics
| Dataset    | Precision | Recall | mAP@0.5 | mAP@0.5:0.95 |
|------------|-----------|--------|---------|--------------|
| Train      | 0.981     | 0.884  | 0.940   | 0.850        |
| Validation | 0.974     | 0.887  | 0.940   | 0.842        |
| Test       | 0.970     | 0.859  | 0.924   | 0.819        |

- Inference extracts bounding boxes of plates from each frame/image.

### 2. **License Plate OCR (YOLOv8m + OCR Dataset)**
- Also trained **YOLOv8m** on a License Plate OCR dataset to identify and isolate text regions.

#### 📊 YOLOv8m OCR Model Metrics
| Dataset    | Precision | Recall | mAP@0.5 | mAP@0.5:0.95 |
|------------|-----------|--------|---------|--------------|
| Training   | 0.962     | 0.957  | 0.960   | 0.846        |
| Validation | 0.955     | 0.950  | 0.953   | 0.831        |
| Testing    | 0.956     | 0.952  | 0.957   | 0.834        |

- Ultimately, **EasyOCR** was used for OCR to achieve better results in text extraction.

### 3. **Plate Cropping**
- Used the trained YOLOv8 detection model to identify bounding boxes around license plates.
- Cropped each detected plate region from the original image using OpenCV.
- Saved all cropped outputs into a dedicated folder (`cropped_plates/`).
- Visualized the cropped images using Matplotlib for confirmation.

### 4. **OCR (Text Extraction)**
- Both **Tesseract OCR** and **EasyOCR** were tested for text extraction.
  - **Tesseract OCR Results**:
    ```
    Text from plate_2_0.jpg: SL#593 LM
    Text from plate_1_0.jpg: ae.
    Text from plate_0_0.jpg: EW 588: |
    ```
  - **EasyOCR Results**:
    ```
    Comparing text from plate_2_0.jpg: SL 593 LM -> Match Found: SL 593 LM
    Comparing text from plate_1_0.jpg: ILACEMWL Lanorc GDKARHHF PLT MFEAW -> No match found.
    Comparing text from plate_0_0.jpg: EW 588 -> Match Found: EW 588
    ```
- **EasyOCR** provided more reliable results, accurately recognizing the license plate numbers.

### 5. **Database Integration**
- SQLite database (`database.db`) with a `license_plates` table.
- Preloaded with 20+ dummy entries and known plates like:
  - SL 593 LM
  - EW 588
- Compared OCR output with the database.
- If a match is found, logs the detection and optionally raises an alert.

### 6. **Web Form (Flask)**
- Flask web interface to **add new license plates** (e.g., lost vehicles) to the database.
- Form accessible via browser and submits entries directly to SQLite.

---

## 🧪 Sample Output
OCR Detection Log:
- **Text**: plate_2_0.jpg: SL 593 LM → **Match Found**: SL 593 LM  
- **Text**: plate_1_0.jpg: ILACEMWL Lanorc GDKARHHF PLT MFEAW → **No match found**.  
- **Text**: plate_0_0.jpg: EW 588 → **Match Found**: EW 588  

---

## 🚀 Getting Started
1. Clone the Repository
bash
Copy code
git clone https://github.com/yourusername/ALPR_Project.git
cd ALPR_Project
2. Install Requirements
bash
Copy code
pip install -r requirements.txt
3. Run the Main Pipeline
Launch the Jupyter notebook:
bash
Copy code
jupyter notebook alpr_main_pipeline.ipynb
Or open it via Google Colab.

4. Start the Web App
bash
Copy code
python app.py
Then open: http://127.0.0.1:5000
---
## 🧰 Technologies Used
YOLOv8m: Plate detection and OCR bounding box training

Easy OCR: Text recognition from cropped images

SQLite: Local database for matching

Flask: Web app for manual plate entry

Python Libraries: OpenCV, PIL, os, sqlite3, etc.
---
## 📈 Future Improvements
Real-time video stream processing

Timestamp and location logging

Alert systems via email/SMS

Dashboard for visualization and monitoring
---
## 🙌 Acknowledgements
Ultralytics (YOLOv8)

Easy OCR

Roboflow (Dataset support)
---
## 📄 License
This project is intended for educational and demonstration purposes only.


