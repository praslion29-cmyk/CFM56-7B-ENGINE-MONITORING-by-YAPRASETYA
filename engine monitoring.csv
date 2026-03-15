import streamlit as st
import pandas as pd
import plotly.graph_objects as go
from PIL import Image
from datetime import datetime

# =======================
# Page config
# =======================
st.set_page_config(
    page_title="CFM56-7B EHM Skywise",
    page_icon="CFM_logo_Core_color.png", 
    layout="wide"
)

# =======================
# Sidebar Logo
# =======================
try:
    logo = Image.open("CFM_logo_Core_color.png") 
    st.sidebar.image(logo, width=150)
except:
    st.sidebar.write("Logo not found")

# =======================
# Header
# =======================
st.title("CFM56-7B ENGINE HEALTH MONITORING")
st.subheader("Skywise-style Dashboard - Created by Y.A. Prasetya")

# =======================
# Limits
# =======================
limits = {
    "N1":102,
    "N2":105,
    "Oil Pressure":60,
    "Oil Temp":150,
    "Fuel Flow":5200,
    "EGT":950,
    "Vibration":4
}

# =======================
# Functions
# =======================
def check_status(value, limit):
    caution = limit*0.95
    if value > limit:
        return "WARNING","red"
    elif value >= caution:
        return "CAUTION","orange"
    else:
        return "Normal","green"

def maintenance_advice(param):
    advice = {
        "EGT":"Possible Cause: Combustion inefficiency / Dirty fuel nozzle\nRecommendation: Borescope & Performance check",
        "Fuel Flow":"Possible Cause: Combustion inefficiency / Fuel control degradation\nRecommendation: Inspect FCU & fuel nozzle",
        "Vibration":"Possible Cause: Rotor imbalance / Bearing wear\nRecommendation: Vibration analysis & inspect fan/blades",
        "N1":"Possible Cause: Overspeed / FADEC issue\nRecommendation: Engine & FADEC inspection",
        "N2":"Possible Cause: High core speed\nRecommendation: Turbine inspection",
        "Oil Pressure":"Possible Cause: Pump malfunction / blockage\nRecommendation: Lubrication system check",
        "Oil Temp":"Possible Cause: Cooling inefficiency / High load\nRecommendation: Inspect oil cooler"
    }
    return advice.get(param,"Engine inspection recommended")

def compute_health_score(params):
    # simple scoring 0-100
    score = 100
    for p,v in params.items():
        limit = limits.get(p,100)
        if v >= limit:
            score -= 20
        elif v >= limit*0.95:
            score -= 10
    if score<0: score=0
    return score

# =======================
# Session storage
# =======================
if "data" not in st.session_state:
    st.session_state.data = pd.DataFrame()

# =======================
# Aircraft Input
# =======================
st.sidebar.header("Aircraft Information")
date = st.sidebar.date_input("Date", datetime.today())
reg = st.sidebar.text_input("Aircraft Registration")
engine = st.sidebar.text_input("Engine No")
route = st.sidebar.text_input("Route")

# =======================
# Engine Start Monitoring
# =======================
st.sidebar.subheader("Engine Start")
egt_start = st.sidebar.number_input("EGT Start (°C)")
if egt_start > 725:
    st.sidebar.error("HOT START DETECTED")
    st.sidebar.write("Check fuel nozzle / combustion chamber")

# =======================
# Takeoff Parameters Input
# =======================
st.sidebar.subheader("Takeoff Parameters")
N1 = st.sidebar.number_input("N1 (%)")
N2 = st.sidebar.number_input("N2 (%)")
EGT = st.sidebar.number_input("EGT Takeoff (°C)")
FuelFlow = st.sidebar.number_input("Fuel Flow (pph)")
OilP = st.sidebar.number_input("Oil Pressure (psi)")
OilT = st.sidebar.number_input("Oil Temp (°C)")
Vib = st.sidebar.number_input("Vibration (ips)")

params = {"N1":N1,"N2":N2,"Oil Pressure":OilP,"Oil Temp":OilT,"Fuel Flow":FuelFlow,"EGT":EGT,"Vibration":Vib}

# =======================
# Health Score
# =======================
health_score = compute_health_score(params)
if health_score>=80:
    score_color="green"
elif health_score>=60:
    score_color="orange"
else:
    score_color="red"
st.markdown(f"### Engine Health Score: <span style='color:{score_color}'>{health_score}</span>",unsafe_allow_html=True)

# =======================
# Engine Status Display
# =======================
st.header("Engine Status")
for p,v in params.items():
    status,color = check_status(v,limits[p])
    st.markdown(f"**{p} : {v} → :{color}[{status}]**")
    if status!="Normal":
        st.warning(maintenance_advice(p))

# =======================
# Add Aircraft Data
# =======================
if st.button("Add Aircraft Data"):
    new_data = {"Date":date,"Aircraft":reg,"Engine":engine,"Route":route}
    new_data.update(params)
    new_data["Health Score"]=health_score
    st.session_state.data = pd.concat([st.session_state.data,pd.DataFrame([new_data])],ignore_index=True)
    st.success("Aircraft data added!")

# =======================
# Fleet Table with Ranking
# =======================
st.header("Fleet Monitoring")
if not st.session_state.data.empty:
    df_sorted = st.session_state.data.sort_values("Health Score")
    st.dataframe(df_sorted)

# =======================
# Plotly Trend Graphs
# =======================
st.header("Trend Charts")
if not st.session_state.data.empty:
    col1,col2,col3 = st.columns(3)
    with col1:
        st.subheader("EGT Takeoff")
        fig = go.Figure()
        fig.add_trace(go.Scatter(y=st.session_state.data["EGT"],mode='lines+markers',name="EGT"))
        st.plotly_chart(fig,use_container_width=True)
    with col2:
        st.subheader("Fuel Flow")
        fig = go.Figure()
        fig.add_trace(go.Scatter(y=st.session_state.data["Fuel Flow"],mode='lines+markers',name="Fuel Flow"))
        st.plotly_chart(fig,use_container_width=True)
    with col3:
        st.subheader("Vibration")
        fig = go.Figure()
        fig.add_trace(go.Scatter(y=st.session_state.data["Vibration"],mode='lines+markers',name="Vibration"))
        st.plotly_chart(fig,use_container_width=True)

# =======================
# Save / Print
# =======================
st.header("Export")
if st.button("Save Data"):
    st.session_state.data.to_csv("engine_monitoring_skywise.csv",index=False)
    st.success("CSV saved successfully")
if st.button("PRINT REPORT"):
    st.info("Use browser print (CTRL + P)")