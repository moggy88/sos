import streamlit as st
import pandas as pd
from urllib.parse import unquote

# ---------------- Streamlit Config ----------------
st.set_page_config(page_title="SOS Card Viewer", page_icon="🚨", layout="centered")
st.title("🚨 SOS Card Viewer")
st.caption("View emergency details from Excel data.")

# ---------------- Load Excel ----------------
@st.cache_data
def load_data():
    df = pd.read_excel("sos_data.xlsx")  # Make sure the Excel file is in your repo
    return df

df = load_data()

# ---------------- Capture URL Parameter ----------------
query_params = st.query_params
name_param = unquote(query_params.get("name", [""])[0]) if "name" in query_params else ""

# ---------------- User Input ----------------
name_query = st.text_input("Enter name to search:", value=name_param)

# ---------------- Render SOS Card ----------------
def render_sos_card(record):
    st.markdown(
        f"""
        <div style="
            background-color:#fff5f5;
            border: 2px solid #ff4b4b;
            border-radius:16px;
            padding:25px;
            margin-top:20px;
            box-shadow:0 4px 12px rgba(255,0,0,0.2);
            max-width:400px;
        ">
            <h2 style="color:#d00000;">🚨 {record.get('Name','Unknown')}</h2>
            <p><b>📞 Phone:</b> {record.get('Phone','N/A')}</p>
            <p><b>🏠 Address:</b> {record.get('Address','N/A')}</p>
            <p><b>🩺 Medical Conditions:</b> {record.get('MedicalConditions','None')}</p>
            <p><b>⚠️ Allergies:</b> {record.get('Allergies','None')}</p>
            <p><b>👩‍👩‍👧 Emergency Contact:</b> {record.get('EmergencyContact','N/A')}</p>
            <p><b>📝 Notes:</b> {record.get('Notes','')}</p>
        </div>
        """,
        unsafe_allow_html=True
    )

# ---------------- Auto-load if URL param ----------------
if name_param:
    st.info(f"Auto-loading record for **{name_param}**...")
    record = df[df["Name"].str.lower() == name_param.lower()]
    if not record.empty:
        for _, row in record.iterrows():
            render_sos_card(row)
    else:
        st.error(f"No record found for '{name_param}'.")

# ---------------- Manual Search ----------------
elif st.button("Search"):
    if not name_query.strip():
        st.warning("Please enter a name.")
    else:
        record = df[df["Name"].str.lower() == name_query.lower()]
        if not record.empty:
            for _, row in record.iterrows():
                render_sos_card(row)
        else:
            st.error(f"No record found for '{name_query}'.")
