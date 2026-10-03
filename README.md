# 🚀 Predictive Maintenance Automation System

<p align="center">

  <img src="https://img.shields.io/badge/Python-3.x-blue?style=for-the-badge&logo=python" />
  <img src="https://img.shields.io/badge/FastAPI-API-green?style=for-the-badge&logo=fastapi" />
  <img src="https://img.shields.io/badge/Machine%20Learning-scikit--learn-orange?style=for-the-badge&logo=scikit-learn" />
  <img src="https://img.shields.io/badge/XGBoost-ML-red?style=for-the-badge" />
  <img src="https://img.shields.io/badge/MySQL-Database-blue?style=for-the-badge&logo=mysql" />
  <img src="https://img.shields.io/badge/UiPath-RPA-orange?style=for-the-badge&logo=uipath" />

</p>

<p align="center">

  <strong>AI-Powered Predictive Maintenance + Intelligent RPA Automation</strong>

</p>

---

## 📌 Overview

The **Predictive Maintenance Automation System** is an AI-powered industrial maintenance solution that combines **Machine Learning, FastAPI, MySQL, and UiPath RPA** to predict machine health and automatically initiate appropriate maintenance actions.

Traditional maintenance approaches often rely on:

- Scheduled maintenance
- Manual inspection
- Reactive repairs
- Human monitoring
- Static maintenance rules

These approaches can result in unnecessary maintenance, unexpected machine failures, downtime, and increased operational costs.

This project introduces a smarter approach.

The system analyzes machine operating conditions and sensor readings to estimate **Remaining Useful Life (RUL)**, detect anomalies, determine machine risk, store prediction results, and automatically execute maintenance workflows using **UiPath RPA**.

---

# 🎯 Project Objective

The primary objective is to build an automated predictive maintenance pipeline that can:

1. Collect machine sensor data.
2. Process and prepare the input data.
3. Predict the machine's Remaining Useful Life.
4. Detect abnormal machine behavior.
5. Classify machine risk.
6. Store prediction results in MySQL.
7. Automatically trigger appropriate maintenance workflows.
8. Prevent duplicate maintenance tickets.
9. Check spare-part availability.
10. Send maintenance notifications.
11. Generate an updated prediction CSV without modifying the original dataset.

---

# 🧠 Core Idea

The system follows a simple concept:

```text
Machine Sensor Data
        ↓
      FastAPI
        ↓
 Machine Learning Model
        ↓
 ┌───────────────────────┐
 │ Remaining Useful Life │
 │ Anomaly Detection     │
 │ Risk Classification   │
 └───────────────────────┘
        ↓
       MySQL
        ↓
     UiPath RPA
        ↓
 ┌──────────┬───────────┬───────────┐
 │ NORMAL   │ WARNING   │ CRITICAL  │
 │ Log Only │ Email     │ Ticket +  │
 │          │ Alert     │ Spare Part│
 │          │           │ + Email   │
 └──────────┴───────────┴───────────┘

✨ Key Features
🤖 1. Machine Learning Prediction

The system uses machine learning models to estimate:

Remaining Useful Life (RUL)
Machine anomaly status
Machine risk level

The project contains trained models including:

Random Forest
XGBoost
Isolation Forest
Feature scaler
📉 2. Remaining Useful Life Prediction

RUL (Remaining Useful Life) estimates how many operational cycles remain before a machine may require maintenance.

Example:

Engine: E019
Cycle: 229
Predicted RUL: 2.3

A very low RUL indicates that the machine requires immediate attention.

🚨 3. Risk Classification

The system converts the prediction into three operational risk levels.

Risk	RUL Condition	Automation
🟢 NORMAL	RUL > 200	Log only
🟡 WARNING	50 < RUL ≤ 200	Send warning email
🔴 CRITICAL	RUL ≤ 50	Maintenance ticket + spare-part check + urgent email

An anomaly can also escalate the machine's risk level.

🔍 4. Anomaly Detection

The project uses an Isolation Forest model to identify unusual machine behavior.

The system produces an anomaly flag:

Anomaly = True

or

Anomaly = False

This allows the automation layer to consider both:

Predicted RUL
Abnormal machine behavior

when determining the maintenance response.

⚙️ 5. FastAPI Backend

FastAPI provides the communication layer between the frontend, machine-learning models, and database.

Main API functionality includes:

POST /predict
GET  /health
POST /process-csv
Prediction Request

The API receives:

Engine ID
Cycle
Operating settings
Sensor readings

and returns:

{
  "engine_id": "E019",
  "predicted_rul": 2.3,
  "anomaly": false,
  "risk": "CRITICAL",
  "recommended_action": "Immediate maintenance required."
}
🗄️ 6. MySQL Database

MySQL is used to persist machine predictions and maintenance information.

Predictions Table

The prediction table stores:

engine_id
cycle
predicted_rul
anomaly
risk
prediction_timestamp

The database allows the RPA workflow to retrieve the latest prediction without directly accessing the ML model.

🤖 7. UiPath RPA Automation

UiPath acts as the automation layer of the project.

After the ML system stores the prediction in MySQL, UiPath retrieves the latest prediction and determines what action should be performed.

NORMAL
Prediction
    ↓
Risk = NORMAL
    ↓
Log machine status
    ↓
No maintenance action
WARNING
Prediction
    ↓
Risk = WARNING
    ↓
Send warning email
CRITICAL
Prediction
    ↓
Risk = CRITICAL
    ↓
Check existing ticket
    ↓
Prevent duplicate ticket
    ↓
Create maintenance ticket
    ↓
Check spare-part availability
    ↓
Send urgent email
🎫 8. Automatic Maintenance Ticket Creation

For CRITICAL predictions, UiPath checks whether an active maintenance ticket already exists for the machine.

This prevents duplicate tickets.

Example:

Engine: 19
Cycle: 229
RUL: 2.3
Risk: CRITICAL
Priority: HIGH

Generated ticket:

TKT-20261001111253

The system checks for existing tickets with statuses such as:

PLANNED
URGENT

before creating a new ticket.

🔧 9. Spare-Part Availability

For critical maintenance cases, the system checks the spare-parts inventory stored in MySQL.

Example inventory:

Part ID	Part	Quantity	Minimum Stock
P001	Bearing	0	2
P002	Belt	0	2
P003	Motor	3	1

UiPath checks whether the required component is available.

If the component is unavailable, an alert email is generated.

Example:

Predictive Maintenance Alert

Engine: 19
Required Spare Part: Bearing
Status: OUT OF STOCK
Available Quantity: 0
📧 10. Automated Email Notifications

The system automatically sends emails depending on machine risk.

WARNING

A warning email informs the maintenance team that the machine requires attention during the next planning cycle.

CRITICAL

An urgent email is sent when immediate maintenance is required.

The email can include:

Engine ID
Cycle
Predicted RUL
Risk
Anomaly status
Recommended action
Spare-part availability
📊 11. Prediction CSV Output

The original sensor dataset remains unchanged.

Original:

data/sample_sensor_data.csv

The system produces a separate file:

data/sample_sensor_data_with_mysql_prediction.csv

The output file contains the machine data along with the prediction information retrieved from MySQL.

Example:

engine_id | cycle | predicted_rul | anomaly | risk
----------------------------------------------------
E019      | 229   | 2.3           | False   | CRITICAL

This separation ensures that the original dataset remains intact.

🖥️ System Architecture
                     ┌────────────────────────┐
                     │   Machine Sensor Data  │
                     │        CSV / UI        │
                     └────────────┬───────────┘
                                  │
                                  ▼
                     ┌────────────────────────┐
                     │       FastAPI          │
                     │      REST API          │
                     └────────────┬───────────┘
                                  │
                                  ▼
                  ┌───────────────────────────────┐
                  │       Machine Learning        │
                  │                               │
                  │  Random Forest / XGBoost      │
                  │  Isolation Forest             │
                  │  Feature Scaling              │
                  └───────────────┬───────────────┘
                                  │
                                  ▼
                  ┌───────────────────────────────┐
                  │        Prediction Result      │
                  │                               │
                  │  RUL                          │
                  │  Anomaly                      │
                  │  Risk                         │
                  │  Recommended Action           │
                  └───────────────┬───────────────┘
                                  │
                                  ▼
                     ┌────────────────────────┐
                     │        MySQL           │
                     │     Predictions        │
                     │ Maintenance Tickets    │
                     │     Spare Parts        │
                     └────────────┬───────────┘
                                  │
                                  ▼
                     ┌────────────────────────┐
                     │       UiPath RPA       │
                     │   Automation Engine    │
                     └────────────┬───────────┘
                                  │
               ┌──────────────────┼──────────────────┐
               ▼                  ▼                  ▼
          🟢 NORMAL          🟡 WARNING         🔴 CRITICAL
          Log Only           Email Alert        Ticket
                                                  ↓
                                            Spare Part Check
                                                  ↓
                                             Urgent Email
🔄 End-to-End Workflow
1. User enters machine information
             ↓
2. Frontend sends request to FastAPI
             ↓
3. FastAPI validates input
             ↓
4. Machine-learning preprocessing
             ↓
5. RUL prediction
             ↓
6. Anomaly detection
             ↓
7. Risk classification
             ↓
8. Prediction stored in MySQL
             ↓
9. UiPath retrieves latest prediction
             ↓
10. Risk-based decision
             ↓
     ┌───────┬─────────┬──────────┐
     ↓       ↓         ↓
   NORMAL WARNING   CRITICAL
     ↓       ↓         ↓
    Log     Email    Ticket
                      ↓
                  Spare Part
                      ↓
                    Email
🧩 Project Structure
PredictiveMaintenance/
│
├── api/
│   ├── app.py
│   ├── database.py
│   ├── schemas.py
│   └── static/
│       └── index.html
│
├── config/
│   └── config.yaml
│
├── data/
│   ├── generate_synthetic_data.py
│   ├── sample_sensor_data.csv
│   ├── sample_sensor_data_with_mysql_prediction.csv
│   ├── processed/
│   │   ├── train.csv
│   │   └── test.csv
│   └── raw/
│
├── database/
│   ├── schema.sql
│   └── queries.sql
│
├── inventory/
│   └── spare_parts.csv
│
├── ml/
│   ├── anomaly_detection.py
│   ├── calculate_rul.py
│   ├── evaluate_models.py
│   ├── preprocess.py
│   ├── risk_engine.py
│   ├── train_random_forest.py
│   ├── train_xgboost.py
│   │
│   └── models/
│       ├── feature_columns.json
│       ├── isolation_forest.pkl
│       ├── random_forest_rul.pkl
│       ├── scaler.pkl
│       └── xgboost_rul.json
│
├── UiPath/
│   ├── Main.xaml
│   ├── UpdatePredictionCSV.xaml
│   ├── CreateMaintenanceTicket.xaml
│   ├── CheckSpareParts.xaml
│   └── SendEmailAlert.xaml
│
├── reports/
│
├── tests/
│
├── .env
├── requirements.txt
└── README.md
🛠️ Technologies Used
Technology	Purpose
🐍 Python	Backend and ML
⚡ FastAPI	REST API
🤖 scikit-learn	Machine Learning
🚀 XGBoost	RUL prediction
🌲 Random Forest	RUL prediction
🔍 Isolation Forest	Anomaly detection
🗄️ MySQL	Prediction and maintenance database
🤖 UiPath	RPA automation
📄 CSV	Machine data and prediction output
🔗 REST / JSON	System communication
🌐 HTML / CSS / JavaScript	Frontend interface
📦 Installation
1. Clone the Repository
git clone https://github.com/MNT237/PEP-RPA.git

Move into the project directory:

cd PEP-RPA
🐍 2. Create Python Virtual Environment

Windows:

python -m venv venv

Activate it:

venv\Scripts\activate
📥 3. Install Dependencies
pip install -r requirements.txt
🗄️ 4. Configure MySQL

Create the required database:

CREATE DATABASE predictive_maintenance;

Then execute the database schema provided in:

database/schema.sql

Configure your database credentials using environment variables.

Example:

DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=predictive_maintenance

Never commit real passwords, API keys, tokens, or other credentials to GitHub.

🤖 5. Start FastAPI

From the project root:

python -m uvicorn api.app:app --host 127.0.0.1 --port 8000

The API will run at:

http://127.0.0.1:8000

API documentation:

http://127.0.0.1:8000/docs
❤️ Health Check

Open:

http://127.0.0.1:8000/health

The endpoint can be used to verify that the API service is running.

🧪 API Testing

The /predict endpoint accepts machine information and returns the prediction.

Example request:

{
  "engine_id": "E019",
  "cycle": 229,
  "operating_setting_1": 0,
  "operating_setting_2": 0,
  "operating_setting_3": 0,
  "sensor_1": 0,
  "sensor_2": 0,
  "sensor_3": 0
}

The actual request should include the complete set of required sensor values expected by the API.

Example response:

{
  "engine_id": "E019",
  "predicted_rul": 2.3,
  "anomaly": false,
  "risk": "CRITICAL",
  "recommended_action": "Immediate maintenance required."
}
📈 Machine Learning Pipeline

The machine-learning pipeline consists of several stages.

Step 1 — Data Preparation

Machine data is loaded from the CSV dataset.

The data contains:

engine_id
cycle
operating settings
sensor readings
RUL
Step 2 — Preprocessing

The preprocessing pipeline:

Sorts data by engine and cycle.
Handles missing values.
Performs feature engineering.
Calculates rolling sensor statistics.
Scales model features.

The project uses a rolling window for sensor-based feature engineering.

Step 3 — RUL Prediction

The trained RUL model estimates:

Remaining Useful Life

Example:

RUL = 206.9 cycles
Step 4 — Anomaly Detection

Isolation Forest evaluates whether the current machine state is unusual.

Output:

True

or:

False
Step 5 — Risk Engine

The predicted RUL and anomaly information are converted into an operational risk.

RUL > 200
      ↓
   NORMAL

50 < RUL ≤ 200
      ↓
   WARNING

RUL ≤ 50
      ↓
  CRITICAL

An anomaly may increase the resulting risk level.

🤖 UiPath Workflow

The UiPath automation is divided into modular workflows.

Main.xaml

The main workflow controls the complete automation.

It:

Connects to MySQL.
Retrieves the latest prediction.
Updates the prediction CSV.
Determines machine risk.
Executes the appropriate automation branch.
UpdatePredictionCSV.xaml

This workflow updates:

sample_sensor_data_with_mysql_prediction.csv

It retrieves prediction results from MySQL and keeps the original CSV unchanged.

CreateMaintenanceTicket.xaml

This workflow creates a maintenance ticket for CRITICAL machines.

Ticket information includes:

Ticket ID
Engine ID
Predicted RUL
Risk
Priority
Description
Status
Created Time
CheckSpareParts.xaml

This workflow checks whether the required spare part is available in inventory.

Possible results:

Available

or:

Out of Stock
SendEmailAlert.xaml

This workflow sends automated maintenance notifications.

It supports:

WARNING
CRITICAL
Spare Part Out of Stock

alerts.

🛡️ Duplicate Ticket Prevention

A major feature of the automation is duplicate prevention.

Before creating a CRITICAL maintenance ticket, UiPath checks MySQL for an existing active ticket.

The workflow checks statuses such as:

PLANNED
URGENT

If an active ticket already exists:

Existing Ticket Found
        ↓
Skip Ticket Creation
        ↓
Continue Workflow

This prevents multiple tickets from being generated for the same machine condition.

📊 Example System Result

Example live prediction:

Engine      : 19
Cycle       : 229
Predicted RUL : 2.3
Anomaly     : False
Risk        : CRITICAL

UiPath response:

CRITICAL BRANCH
      ↓
Duplicate Ticket Check
      ↓
No Active Ticket
      ↓
Create HIGH Priority Ticket
      ↓
Check Spare Parts
      ↓
Send Urgent Email
📧 Automation Decision Matrix
Machine Status	Database	Ticket	Spare Part	Email
🟢 NORMAL	✅	❌	❌	❌
🟡 WARNING	✅	❌	❌	✅ Warning
🔴 CRITICAL	✅	✅	✅	✅ Urgent

This makes the automation behavior predictable and easy to audit.

🔐 Security

The project should not expose sensitive credentials.

Do not commit:

.env
database passwords
API keys
SMTP passwords
access tokens
private credentials

Use environment variables instead.

Example:

DB_PASSWORD=your_password
SMTP_PASSWORD=your_password

and add sensitive files to .gitignore.

🧪 Testing

The project can be tested at multiple levels.

API Testing

Test:

/health
/predict
/process-csv
Database Testing

Verify:

SELECT * FROM predictions;

and:

SELECT * FROM maintenance_tickets;
RPA Testing

Test all three risk paths:

NORMAL
WARNING
CRITICAL

Also test:

Duplicate Ticket
Spare Part Available
Spare Part Out of Stock
🐛 Troubleshooting
API Does Not Start

Try:

python -m uvicorn api.app:app --host 127.0.0.1 --port 8000

Check that the required Python packages are installed.

MySQL Connection Error

Verify:

MySQL Server
Host
Port
Username
Password
Database

Typical MySQL port:

3306
UiPath Cannot Retrieve Prediction

Check:

FastAPI is running.
MySQL is running.
The prediction exists in the database.
UiPath has the correct database connection.
The latest prediction query returns a row.
Duplicate Maintenance Ticket

The system intentionally checks for existing active tickets before creating another ticket.

Verify:

SELECT *
FROM maintenance_tickets
WHERE engine_id = '19';
📌 Important Data Integrity Rule

The original dataset:

sample_sensor_data.csv

is never modified by the prediction workflow.

Instead, the system creates/updates:

sample_sensor_data_with_mysql_prediction.csv

This ensures that the original source data remains available for:

Training
Testing
Reproducibility
Comparison
Future experiments
🌟 Project Benefits
⚡ Early Failure Detection

Machine-learning predictions provide an estimate of remaining useful life before a potential failure.

🤖 Automation

UiPath automatically performs maintenance-related actions based on machine risk.

💰 Reduced Manual Work

The system reduces the need for manual monitoring and repetitive maintenance tasks.

🎯 Risk-Based Maintenance

Different risk levels trigger different operational responses.

🛡️ Duplicate Prevention

The system avoids creating multiple active tickets for the same machine condition.

📊 Centralized Data

Prediction and maintenance information is stored in MySQL.

🔄 End-to-End Integration

The project integrates:

Frontend
   ↓
FastAPI
   ↓
Machine Learning
   ↓
MySQL
   ↓
UiPath
   ↓
Email / Ticket / Inventory
🚀 Future Enhancements

Possible future improvements include:

Real-time IoT sensor integration
Live machine monitoring dashboard
Power BI analytics
Advanced deep-learning RUL models
More sophisticated component-failure prediction
Automatic inventory replenishment
Mobile maintenance notifications
Predictive spare-part demand
Maintenance cost optimization
Cloud deployment
Multi-machine monitoring
Historical trend visualization
Role-based authentication
Production-grade monitoring and logging
🎓 Project Use Case

This project can be applied to industries where machine downtime can cause operational or financial losses.

Potential use cases include:

Manufacturing
Aviation
Automotive
Industrial machinery
Power plants
Robotics
Heavy equipment
Production facilities
🏆 Project Highlights
✅ Machine Learning
✅ Remaining Useful Life Prediction
✅ Anomaly Detection
✅ Risk Classification
✅ FastAPI REST API
✅ MySQL Database
✅ UiPath RPA
✅ Automated Maintenance Tickets
✅ Duplicate Ticket Prevention
✅ Spare-Part Verification
✅ Automated Email Alerts
✅ CSV Prediction Output
✅ Modular Architecture
👨‍💻 Project

Predictive Maintenance Automation System

Built using:

Python
Machine Learning
FastAPI
MySQL
UiPath RPA
REST API
CSV
📜 Disclaimer

This project is intended for educational, demonstration, and prototype purposes.

Machine-learning predictions should be validated against real machine data and domain-specific maintenance procedures before being used in safety-critical or production environments.

⭐ Support

If you find this project useful, consider giving the repository a ⭐ on GitHub.


🚀 Predict Failures. Automate Maintenance. Reduce Downtime.




