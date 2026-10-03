# Predictive-Maintenance-Automation-System
The project combines Machine Learning and UiPath RPA to predict machine failures, assess risk, store predictions, and automatically trigger the appropriate maintenance workflow.

# 🔧 Predictive Maintenance Automation System

An intelligent predictive maintenance system that combines **Machine Learning, FastAPI, MySQL, UiPath RPA, CSV-based machine data, and an interactive frontend** to predict machine health and automate maintenance actions.

The system analyzes machine operating conditions and sensor readings to estimate **Remaining Useful Life (RUL)**, detect anomalies, classify machine risk, store predictions in MySQL, and trigger automated maintenance workflows through UiPath.

---

## 📌 Overview

Traditional maintenance approaches often depend on fixed schedules or manual inspection. This project uses a predictive approach where machine sensor data is analyzed by Machine Learning models to identify potential maintenance requirements before a failure occurs.

The system follows this pipeline:

```text
Machine Data
     ↓
AeroPulse AI Frontend
     ↓
FastAPI REST API
     ↓
Machine Learning
     ↓
RUL + Anomaly + Risk
     ↓
MySQL
     ↓
UiPath RPA
     ↓
┌──────────────┬──────────────┬──────────────┐
│   CRITICAL   │   WARNING    │    NORMAL    │
│              │              │              │
│ Ticket       │ Email        │ Log Only     │
│ Spare Parts  │ Only         │              │
│ Urgent Email │              │              │
└──────────────┴──────────────┴──────────────┘
     ↓
Prediction Output CSV
