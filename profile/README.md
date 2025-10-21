# OrthoCannon
OrthoCannon is an AI-powered orthopaedic learning and diagnostic platform built with Flutter.
It integrates case-based learning, fracture detection in X-rays, and an AI chatbot grounded in orthopaedic research from PubMed.
This project combines mobile app development, deep learning, and cloud-based AI pipelines — all under the Ortho-Cannon GitHub organization.
 
## Core Functionalities
### 1. 🧠 Case-Based Learning
•	Interactive orthopaedic case library.
•	Data sourced from curated PubMed datasets.
•	Users can browse, learn, and test their understanding of orthopaedic principles.
•	JSON dataset stored locally in the Flutter app.
### 2. 🩻 Fracture Detection (X-ray Upload)
•	Users upload an X-ray image.
•	The image is sent to a Cloud Run function backed by a YOLO (You Only Look Once) model.
•	The function returns bounding box coordinates marking suspected fractures.
•	Bounding boxes are drawn directly on the X-ray in the Flutter app.
### 3. 💬 AI Chatbot (Grounded in PubMed)
•	AI chatbot fine-tuned to orthopaedic topics.
•	Chat sessions managed through Firebase Functions and Firestore.
•	Responses are grounded in vetted PubMed literature.
•	Backed by Vertex AI for natural language generation.
 
## Repository Structure
Repository	Description	Technologies	Link
OrthoCannon
Flutter mobile app (frontend). Contains UI, logic, and case-based JSON data.	[Flutter, Dart	View →](https://github.com/Ortho-Cannon/OrthoCannon)

OrthoCannonYOLO
Cloud Run backend for fracture detection. Includes YOLO models and inference code.	[Python, PyTorch, TFLite, Google Cloud Run	View →](https://github.com/Ortho-Cannon/OrthoCannonYOLO)

OrthoCannonAI
Dataset preparation pipeline for AI grounding. Fetches orthopaedic research data from PubMed.	[Python, Vertex AI, Cloud Storage	View →](https://github.com/Ortho-Cannon/OrthoCannonAI)

OrthoCannonFirebase
Firebase Functions for managing chat sessions and invoking Vertex AI responses.	[Node.js, Firebase Functions, Firestore	View →](https://github.com/Ortho-Cannon/OrthoCannonFirebase)

yolo_docker
(Deprecated) Containerized YOLO model deployment system. Replaced by Cloud Run functions for cost efficiency.	[Docker, PyTorch	View →](https://github.com/Ortho-Cannon/yolo_docker)

 
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
git clone https://github.com/Ortho-Cannon/OrthoCannon.git

#### Backend & AI components (optional if you're not modifying them)
git clone https://github.com/Ortho-Cannon/OrthocannonYOLO.git
git clone https://github.com/Ortho-Cannon/OrthoCannonAI.git
git clone https://github.com/Ortho-Cannon/OrthoCannonFirebase.git
 
### 2. Run the Flutter App
cd OrthoCannon
flutter pub get
flutter run
Make sure your environment has Flutter SDK installed and connected to a device or emulator.
 
### 3. Deploy Backend Components (Optional)
#### YOLO Cloud Function
•	Found in OrthoCannonYOLO
•	Deploy to Google Cloud Run:
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
 #### Firebase Chatbot Function
•	Found in OrthoCannonFirebase
•	Deploy using Firebase CLI:
firebase deploy --only functions
#### Dataset Preparation
•	Found in OrthoCannonAI
•	Run locally or as a scheduled Cloud Function to update the dataset.
 
Cloud Integrations
Service	Purpose
Google Cloud Run	Hosts YOLO model inference API
Firebase Functions	Manages chat sessions and interacts with Vertex AI
Firestore	Stores user chats and session history
Vertex AI	Generates grounded AI chatbot responses
Cloud Storage	Hosts PubMed-filtered orthopaedic dataset
PubMed API	Source of medical literature data
 
## Technologies Used
### Frontend
•	Flutter / Dart
•	Firebase Auth / Firestore
•	REST API integration
### Backend
•	Python (YOLO + Data Preparation)
•	Node.js (Firebase Functions)
•	Google Cloud Platform (Run, Storage, Vertex AI)
### Machine Learning
•	YOLOv8 (Fracture detection)
•	Vertex AI (Chat completion)
•	PubMed (Data grounding)
 
### Use Cases
•	Medical students studying orthopaedics.
•	Radiologists verifying fracture predictions.
•	Doctors reviewing similar cases from literature.
•	Researchers querying orthopaedic AI knowledge.
 
## Future Improvements
•	Real-time fracture detection using on-device TFLite models.
•	Support for segmentation masks and 3D visualizations.
•	Integration with PACS or DICOM viewers.
•	Expanded dataset to include trauma and joint imaging.
## Contributors
•	Project Owner: Anthony Piland
•	Project Developers: Mike England(OrthoCannonAI & OrthoCannonFirebase) and Anderson Macharia Kinyua(OrthoCannon, OrthoCannonYOLO & yolo_docker)
•	Organization: Ortho-Cannon on GitHub

<img width="451" height="673" alt="image" src="https://github.com/user-attachments/assets/11af0e23-29cc-4e37-ba4d-79cf1c15adb2" />
