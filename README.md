# MOMs
Creating Minutes of Meeting
import os
from dotenv import load_dotenv
load_dotenv()
import google.generativeai as genai
from PIL import Image 
import streamlit as st 

# Genai Configuration
genai.configure(api_key=os.getenv("Google-API-Key"))
model = genai.GenerativeModel("gemini-2.0-flash")

# Front End Aspect
st.header(":blue[M.O.M] Generator", divider=True)
st.markdown("Minutes of Meeting Generator: Upload your handwritten MOMs")
uploaded_file = st.file_uploader("Upload your Image", 
                 type= ["jpg", "jpeg","png"])

if uploaded_file is not None:
    img = Image.open(uploaded_file)
    st.image(img, use_container_width=True)

prompt = f'''

You are an AI assistant designed to convert **handwritten meeting note images**—including text, to‑dos, deadlines, and status indicators—into a polished **Minutes of Meeting**, with a focus on **action items** structured in a table.

2. **Identify each action item** (to‑do) and determine:
   - **Particular** (task description)
   - **Deadline** (if present)
   - **Status** (Completed / Pending / Not Started)
   - **Percent complete** (if written, else mark as “not specified”)
   - **Owner** if specified, else “unknown”)'''



with st.spinner("Extracting and Analysing the Image...."):
    response = model.generate_content([img, prompt])
    st.success("Extraction Complete")
    st.markdown(response.text)
