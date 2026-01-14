# 🚚 Swiggy Delivery Time Prediction

## 📌 Project Overview
This project focuses on building a **Delivery Time Prediction system for Swiggy** using a **Stacking Regression** approach.  
The model predicts food delivery duration by learning from historical **order, traffic, weather, and distance** data.

A **RESTful API was developed using FastAPI** to serve real-time delivery time predictions, enabling seamless integration with client applications and production deployment workflows.

---

## 🧠 Modeling Approach
A **Stacking Regression model** was implemented using:

- Random Forest Regressor  
- LightGBM Regressor  
- Linear Regression as the meta-learner  

This ensemble strategy improves prediction accuracy by combining the strengths of multiple models.

### 🔍 Model Selection & Hyperparameter Tuning
- Performed **hyperparameter tuning using Optuna** to optimize model performance
- Logged **parameters, metrics, and artifacts using MLflow** for experiment tracking
- Compared multiple experiment runs to select the **best-performing model** based on evaluation metrics
- Promoted the selected model through MLflow model stages for downstream deployment

---

## 📊 Dataset
- **45,000+ delivery records**
- Features include:
  - Restaurant and delivery location coordinates  
  - Traffic conditions  
  - Weather conditions  
  - Order time & day  
  - Restaurant and delivery metadata  

---

## 🔍 Exploratory Data Analysis & Feature Engineering
- Performed data cleaning to handle missing values and outliers  
- Conducted EDA to identify key factors affecting delivery time  

### 🧠 Feature Extraction Logic

The following feature extraction steps were applied to enrich the dataset with meaningful **location-based** and **time-based** information.

---

### 🏙️ City Extraction from Rider ID
- Extracted **city name** from the `rider_id` column
- The city code was parsed by splitting the rider ID string
- This feature helps capture **regional delivery patterns**

---

### 📅 Date-Based Feature Extraction
From the order date, the following features were derived:

- **Day** – Calendar day of the month  
- **Day Name** – Name of the day (e.g., Monday, Tuesday)  
- **Day of Week** – Numerical representation of the weekday  
- **Is Weekend** – Binary flag indicating Saturday or Sunday  
- **Month** – Month of the year  
- **Year** – Year of the order  

These features help the model learn **weekly and seasonal trends** in delivery time.

---

### ⏰ Time-Based Feature Extraction
From the order time, the following features were extracted:

- **Hour of Order** – Hour when the order was placed (0–23)  
- Captures **peak vs non-peak delivery hours**

---

### 📐 Distance Calculation (Haversine Formula)
- Calculated the **Haversine distance** between the restaurant and delivery location using:
  - `restaurant_latitude`
  - `restaurant_longitude`
  - `delivery_latitude`
  - `delivery_longitude`
- This geospatial feature represents the **straight-line distance (in kilometers)** between two points on Earth and serves as a key predictor of delivery duration.

---

### ✅ Why These Features Matter
- City-level patterns capture **urban vs semi-urban delivery behavior**
- Date and time features help model **traffic rush hours and weekend effects**
- Improves model generalization and prediction accuracy

---

## 📈 Model Performance
Evaluated on unseen test data:

| Metric | Score |
|------|------|
| **R² Score** | **0.83** |
| **MAE** | **3.13 minutes** |

---
## 🚀 Model Serving
- Built a REST API using **FastAPI** for real-time inference
- Implemented **input validation using Pydantic** to ensure type safety, schema enforcement, and reliable API requests
- Containerized the API using **Docker**
- Deployed on **AWS EC2** using **AWS CodeDeploy** with a **Rolling Deployment** strategy

---

## 🏗️ MLOps & Production Pipeline
Designed a **production-grade ML pipeline** with the following tools:

- **MLflow** – Experiment tracking & model staging  
- **DVC** – Data & model versioning  
- **Docker** – Containerized training & inference  
- **GitHub Actions** – CI/CD automation  
### ☁️ AWS Services
- **Amazon S3** – Artifact storage for datasets, models, and pipeline outputs  
- **Amazon ECR** – Docker image registry for versioned model images  
- **Amazon EC2** – Model hosting and inference service  
- **AWS CodeDeploy** – Automated deployment using a **Rolling Deployment strategy**, enabling zero/minimal downtime by gradually updating instances


---

## 📌 Tech Stack
- Python  
- Scikit-learn  
- LightGBM  
- MLflow  
- DVC  
- Docker
- FASTAPI  
- GitHub Actions
- LightGBM  
- AWS (S3, EC2, ECR, CodeDeploy)

---


```
swiggy-time-delivery-prediction/
│
├── .github/
│   └── workflows/
│       ├── ci-cd.yml                <- CI-CD pipeline (lint, test, build, Dockebuild & deploy) 
│                              
│
├── deploy/
│   └── scripts/
│       ├── install_dependencies.sh <- Install system & Python dependencies
│       └── start_docker.sh         <- Run Docker containers
│
│
├── data/
│   ├── external/       <- Third-party data
│   ├── interim/        <- Cleaned intermediate data
│   ├── processed/      <- Final ML-ready datasets
│   └── raw/            <- Original unmodified data
│
├── docs/
│   └── READme.md
│
├── models/
│   ├── model.joblib
│   ├── power_transformer.joblib
│   └── preprocessor.joblib
│   └── stacking_regressor.joblib
│
├── notebooks/ 
│   │
│   ├── Food_Delivery_Baseline_Model.ipynb
│   ├── Food_Delivery_Exp_1_drop_vs_impute.ipynb
│   ├── Food_Delivery_Exp_2_missing_indicator.ipynb
│   ├── Food_Delivery_LGBM_HP_Tuning.ipynb
│   ├── Food_Delivery_Model_Selection.ipynb
│   ├── Food_Delivery_RF_HP_Tuning.ipynb
│   ├── Food_Delivery_Stacking_Regressor_HP_Tuning.ipynb
│   ├── random_forest_hp_tuning.ipynb
│   ├── data_cleaning.ipynb
│   └── food_delivery_EDA.ipynb
│   # Jupyter notebooks & raw data exploration
│
├── references/                   # Reference materials
├── reports/                      # Generated reports & analysis
│
├── scripts/
│   ├── __init__.py
│   ├── data_clean_utils.py
│   └── promote_model_to_prod.py
│   └── sample_predictions.py                     
├── src/
│   ├── data/
│   │   ├── .gitkeep
│   │   ├── __init__.py
│   │   ├── data_cleaning.py
│   │   └── data_preparation.py
│   │
│   ├── features/
│   │   ├── .gitkeep
│   │   ├── __init__.py
│   │   └── data_preprocessing.py
│   │
│   ├── models/
│   │   ├── .gitkeep
│   │   ├── __init__.py
│   │   ├── train.py
│   │   ├── evaluation.py
│   │   └── register_model.py
│   │
│   └── visualization/
│        └── __init__.py
│    
├── tests/
│     ├── test_api_endpoint.py
│     ├── test_model_perf.py
│     └── test_model_registry.py                       # Unit & integration tests
│
├── .dvcignore                     # DVC ignore rules
├── .gitignore                     # Git ignore rules
├── Dockerfile                    # Docker image definition
├── LICENSE                       # Project license
├── Makefile                      # Automation commands
├── README.md                     # Project overview & usage
│
├── app.py                        # Application entry point
├── appspec.yml                   # Deployment specification
│
├── dvc.yaml                      # DVC pipeline definition
├── dvc.lock                      # DVC pipeline lock file
├── params.yaml                   # Model & pipeline parameters
│
├── requirements.txt              # Production dependencies
├── requirements-dev.txt          # Development dependencies
├── requirements-dockers.txt      # Docker-specific dependencies
│
├── setup.py                      # Package setup
├── test_environment.py           # Environment validation
└── tox.ini                       # Testing configuration

```

--- 
## 🚀 Key Highlights
- End-to-end ML lifecycle automation from data preprocessing to production deployment  
- Experiment tracking, model selection, and versioning using **MLflow**  
- Hyperparameter optimization using **Optuna** to improve model performance  
- Production-ready **FastAPI** inference service with **Pydantic-based input validation**  
- Scalable deployment architecture on **AWS** with **Docker** and **Rolling Deployments**
 

--- 
## 🧑‍💻 Author

**Sourav Raj**  
Data Scientist | Data Analyst

Feel free to connect on LinkedIn or explore my other projects.

🔗 LinkedIn: https://www.linkedin.com/in/sourav664
