
# CFM56-7B Engine Health Monitoring - Skywise Style  
**Created by Y.A. Prasetya**

## Overview
This is a **Skywise-style Engine Health Monitoring (EHM) dashboard** built with **Python and Streamlit** for monitoring **CFM56-7B engines**.  
It provides **engine health scores, fleet ranking, trend charts, and maintenance recommendations** in a professional airline-like interface.

Ideal for:
- Aircraft Maintenance Engineers  
- Reliability Engineers  
- MRO Technical Teams  
- Aviation Data Analysts  

---

## Features

### 1. Aircraft Information Input
- Date
- Aircraft Registration
- Engine Number
- Route

### 2. Engine Start Monitoring
- Monitor **EGT Start**  
- **HOT START detection** if EGT > 725°C  
- Shows **possible cause** and **maintenance recommendation**

### 3. Takeoff Parameter Monitoring
| Parameter     | Max Limit | Status Logic (Normal / CAUTION / WARNING) |
|---------------|-----------|------------------------------------------|
| N1            | 102 %     | CAUTION ≥ 95% limit                     |
| N2            | 105 %     | CAUTION ≥ 95% limit                     |
| Oil Pressure  | 60 psi    | CAUTION ≥ 95% limit                     |
| Oil Temp      | 150 °C    | CAUTION ≥ 95% limit                     |
| Fuel Flow     | 5200 pph  | CAUTION ≥ 95% limit                     |
| EGT Takeoff   | 950 °C    | CAUTION ≥ 95% limit                     |
| Vibration     | 4 ips     | CAUTION ≥ 95% limit                     |

- **Status colors:**  
  - **Normal** = Green  
  - **CAUTION** = Orange  
  - **WARNING** = Red  
- Automatic **maintenance advice** for CAUTION / WARNING

### 4. Engine Health Score
- Calculates **0–100 score** based on all monitored parameters  
- Color-coded **Green / Orange / Red**  

### 5. Fleet Monitoring & Ranking
- Add multiple aircraft entries  
- Table sorted by **lowest health score** first  

### 6. Trend Charts (Interactive)
- **EGT Takeoff**, **Fuel Flow**, **Vibration**  
- Plotly interactive charts (hover, zoom)

### 7. Data Export
- **Save CSV** → `engine_monitoring_skywise.csv`  
- **Print report** → use browser print (CTRL+P)  

---

## Technologies Used
- **Python 3.x**  
- **Streamlit** – dashboard web interaktif  
- **Pandas** – data storage & processing  
- **Plotly** – interactive trend charts  
- **Pillow** – logo image support  
- **FPDF** – optional PDF export  

---

## Installation

Install required libraries:

```bash
pip install -r requirements.txt
