# OrthoCannon
OrthoCannon is an AI-powered orthopaedic learning and diagnostic platform built with Flutter.<br>
It integrates case-based learning, fracture detection in X-rays, and an AI chatbot grounded in orthopaedic research from PubMed.<br>
This project combines mobile app development, deep learning, and cloud-based AI pipelines — all under the Ortho-Cannon GitHub organization.<br>
 
## Core Functionalities
### 1. 🧠 Case-Based Learning
•	Interactive orthopaedic case library.<br>
•	Data sourced from curated PubMed datasets.<br>
•	Users can browse, learn, and test their understanding of orthopaedic principles.<br>
•	JSON dataset stored locally in the Flutter app.<br>
### 2. 🩻 Fracture Detection (X-ray Upload)
•	Users upload an X-ray image.<br>
•	The image is sent to a Cloud Run function backed by a YOLO (You Only Look Once) model.<br>
•	The function returns bounding box coordinates marking suspected fractures.<br>
•	Bounding boxes are drawn directly on the X-ray in the Flutter app.<br>
### 3. 💬 AI Chatbot (Grounded in PubMed)
•	AI chatbot fine-tuned to orthopaedic topics.<br>
•	Chat sessions managed through Firebase Functions and Firestore.<br>
•	Responses are grounded in vetted PubMed literature.<br>
•	Backed by Vertex AI for natural language generation.<br>
 
## Repository Structure
Repository	Description	Technologies	Link<br>
OrthoCannon<br>
Flutter mobile app (frontend). Contains UI, logic, and case-based JSON data.	[Flutter, Dart	View →](https://github.com/Ortho-Cannon/OrthoCannon)<br>

OrthoCannonYOLO<br>
Cloud Run backend for fracture detection. Includes YOLO models and inference code.	[Python, PyTorch, TFLite, Google Cloud Run	View →](https://github.com/Ortho-Cannon/OrthoCannonYOLO)<br>

OrthoCannonAI<br>
Dataset preparation pipeline for AI grounding. Fetches orthopaedic research data from PubMed.	[Python, Vertex AI, Cloud Storage	View →](https://github.com/Ortho-Cannon/OrthoCannonAI)<br>

OrthoCannonFirebase<br>
Firebase Functions for managing chat sessions and invoking Vertex AI responses.	[Node.js, Firebase Functions, Firestore	View →](https://github.com/Ortho-Cannon/OrthoCannonFirebase)<br>

yolo_docker<br>
(Deprecated) Containerized YOLO model deployment system. Replaced by Cloud Run functions for cost efficiency.	[Docker, PyTorch	View →](https://github.com/Ortho-Cannon/yolo_docker)<br>

 
## Architecture Overview
                          ┌──────────────────────────┐
                          │       PubMed API         │
                          └────────────┬─────────────┘
                                       │
                                       ▼
                     ┌────────────────────────────────────┐
                     │  OrthoCannonAI (Data Preparation)  │
                     │ - Fetches & filters ortho data     │
                     │ - Stores dataset in Cloud Storage  │
                     └────────────────────────────────────┘
                                       │
                                       ▼
           ┌──────────────────────────────┐
           │ Vertex AI / Model Registry   │
           │ - Chatbot & Fracture models  │
           └──────────────┬───────────────┘
                          │
                          ▼
     ┌──────────────────────────────────────────────┐
     │ OrthoCannonFirebase (Cloud Functions)        │
     │ - Manages chat sessions & Firestore storage  │
     │ - Queries Vertex AI for grounded responses   │
     └──────────────────────────────────────────────┘
                          │
                          ▼
     ┌──────────────────────────────────────────────┐
     │ OrthoCannonYOLO (Cloud Run API)              │
     │ - Receives X-ray uploads                     │
     │ - Runs YOLO model to detect fractures        │
     │ - Returns bounding box coordinates           │
     └──────────────────────────────────────────────┘
                          │
                          ▼
     ┌──────────────────────────────────────────────┐
     │ OrthoCannon (Flutter App)                    │
     │ - Case-based learning                        │
     │ - X-ray upload & bounding box display         │
     │ - AI Chatbot with PubMed grounding            │
     └──────────────────────────────────────────────┘
 
## Setup Instructions
### 1. Clone the Repositories
#### Main Flutter app
```
git clone https://github.com/Ortho-Cannon/OrthoCannon.git

#### Backend & AI components (optional if you're not modifying them)
git clone https://github.com/Ortho-Cannon/OrthocannonYOLO.git
git clone https://github.com/Ortho-Cannon/OrthoCannonAI.git
git clone https://github.com/Ortho-Cannon/OrthoCannonFirebase.git
```
 
### 2. Run the Flutter App
```
cd OrthoCannon
flutter pub get
flutter run
```
Make sure your environment has Flutter SDK installed and connected to a device or emulator.
 
### 3. Deploy Backend Components (Optional)
#### YOLO Cloud Function
•	Found in OrthoCannonYOLO
•	Deploy to Google Cloud Run:
```
gcloud functions deploy predict_fracture \
  --gen2 \
  --runtime=python311 \
  --region=us-central1 \
  --source=. \
  --entry-point=predict_fracture \
  --trigger-http \
  --memory=8Gi \
  --cpu=4 \
  --timeout=540s
```
 #### Firebase Chatbot Function
•	Found in OrthoCannonFirebase<br>
•	Deploy using Firebase CLI:<br>
```
firebase deploy --only functions
```
#### Dataset Preparation
•	Found in OrthoCannonAI<br>
•	Run locally or as a scheduled Cloud Function to update the dataset.<br>
 
## Cloud Integrations - Service	Purpose
Google Cloud Run	- Hosts YOLO model inference API<br>
Firebase Functions - 	Manages chat sessions and interacts with Vertex AI<br>
Firestore -	Stores user chats and session history<br>
Vertex AI -	Generates grounded AI chatbot responses<br>
Cloud Storage	- Hosts PubMed-filtered orthopaedic dataset<br>
PubMed API	- Source of medical literature data<br>
 
## Technologies Used
### Frontend
•	Flutter / Dart<br>
•	Firebase Auth / Firestore<br>
•	REST API integration<br>
### Backend
•	Python (YOLO + Data Preparation)<br>
•	Node.js (Firebase Functions)<br>
•	Google Cloud Platform (Run, Storage, Vertex AI)<br>
### Machine Learning
•	YOLOv8 (Fracture detection)<br>
•	Vertex AI (Chat completion)<br>
•	PubMed (Data grounding)<br>
 
### Use Cases
•	Medical students studying orthopaedics.<br>
•	Radiologists verifying fracture predictions.<br>
•	Doctors reviewing similar cases from literature.<br>
•	Researchers querying orthopaedic AI knowledge.<br>
 
## Future Improvements
•	Real-time fracture detection using on-device TFLite models.<br>
•	Support for segmentation masks and 3D visualizations.<br>
•	Integration with PACS or DICOM viewers.<br>
•	Expanded dataset to include trauma and joint imaging.<br>
## Contributors
•	Project Owner: Anthony Piland<br>
•	Project Developers: Mike England(OrthoCannonAI & OrthoCannonFirebase) and Anderson Macharia Kinyua(OrthoCannon, OrthoCannonYOLO & yolo_docker)<br>
•	Organization: Ortho-Cannon on GitHub<br>

