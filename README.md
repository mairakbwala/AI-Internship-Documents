import streamlit as st
from keras.models import load_model
from PIL import Image, ImageOps
import numpy as np
from google import genai

# -------------------------------
# TM MODEL (Printed vs Handwritten)
# -------------------------------

np.set_printoptions(suppress=True)

model = load_model("keras_Model.h5", compile=False)
class_names = open("labels.txt", "r").readlines()

# -------------------------------
# GEMINI CLIENT
# -------------------------------

client = genai.Client(api_key=st.secrets.get("GEMINI_API_KEY"))

# -------------------------------
# STREAMLIT UI
# -------------------------------

st.title("Bloom Checker")

uploaded_file = st.file_uploader("Upload Question Image (Printed or Handwritten)", type=["jpg", "png", "jpeg"])

if uploaded_file is not None:
    image = Image.open(uploaded_file).convert("RGB")
    st.image(image, caption="Uploaded Image", use_column_width=True)

    # Prepare image for TM model
    size = (224, 224)
    image_resized = ImageOps.fit(image, size, Image.Resampling.LANCZOS)
    image_array = np.asarray(image_resized)
    normalized_image = (image_array.astype(np.float32) / 127.5) - 1
    data = np.ndarray(shape=(1, 224, 224, 3), dtype=np.float32)
    data[0] = normalized_image

    prediction = model.predict(data)
    index = np.argmax(prediction)
    confidence = float(prediction[0][index]) * 100

    if index == 0:
        modality = "Printed"
    else:
        modality = "Handwritten"

    st.subheader("Detected Input Type")
    st.write(f"→ **{modality}** ({confidence:.2f}% confidence)")

st.subheader("Type the Question")
user_question = st.text_area("Enter the exact question text here:")

if st.button("Analyze Bloom Level") and user_question.strip() != "":
    prompt = f"""
Classify the cognitive level of the following question using Bloom's Taxonomy.
Then provide one suggestion to increase the cognitive level.

Question: "{user_question}"

Respond in this JSON format only:
{{
  "bloom_level": "<Remember/Understand/Apply/Analyze/Evaluate/Create>",
  "suggestion": "<short suggestion>"
}}
"""
    result = client.models.generate_content(model="gemini-1.5-flash", contents=prompt)
    st.subheader("Bloom Output")
    st.write(result.text)
