import streamlit as st
import openai

openai.api_key = st.secrets["openai_key"]

st.set_page_config(page_title="SmartMedia AI", layout="centered")

st.title("🎬 SmartMedia AI")
st.subheader("Your AI Assistant + Video Generator")

tab1, tab2 = st.tabs(["💬 ChatGPT Assistant", "🎥 AI Video Generator"])

with tab1:
    st.info("Chat with a YouTube-savvy AI assistant.")
    user_input = st.text_input("Ask a question (e.g., How to grow my YouTube channel?)")

    if user_input:
        with st.spinner("Thinking..."):
            response = openai.ChatCompletion.create(
                model="gpt-3.5-turbo",
                messages=[
                    {"role": "system", "content": "You are a helpful assistant that gives YouTube tips, video ideas, and answers general questions."},
                    {"role": "user", "content": user_input}
                ]
            )
            st.success("Here's what I found:")
            st.write(response.choices[0].message.content)

with tab2:
    st.info("Describe a scene and generate an AI video (demo mode)")
    video_prompt = st.text_area("e.g., A dragon flying over a mountain at sunset")

    if st.button("🎬 Generate Video"):
        if video_prompt:
            st.success("Video generated (demo).")
            st.video("https://samplelib.com/lib/preview/mp4/sample-5s.mp4")
        else:
            st.warning("Please enter a scene description.")
