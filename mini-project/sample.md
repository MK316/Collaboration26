# Multi-language TTS App

This is a simple multi-language text-to-speech app made with Gradio and gTTS.

+ [Gradio](https://gradio.app) is a simple Python tool for building interactive web apps for machine learning or language-learning projects. It allows beginners to create apps with text boxes, buttons, audio, and images without needing complex web programming.
+ [Huggingface](https://huggingface.co/spaces) is an online platform where people can share, test, and publish AI models and web apps. Students can use Hugging Face Spaces to upload and run Gradio apps on the web so that others can open and use them through a link.
+ [streamlit]() is a simple Python framework for building interactive web apps, especially for data exploration, education, and demos. It helps beginners turn Python scripts into usable web pages with buttons, text input boxes, charts, and media display, without needing advanced web development skills.

#### 1. Tool Functions

| Tool | Main function |
|---|---|
| Gradio | Build a simple interactive app with Python |
| Streamlit | Build and also deploy a Python web app |
| Hugging Face Spaces | Host and share apps online |

#### 2. Function comparisons to make a web app

Difficulty scale:  
- 🟢 Easy = very beginner-friendly  
- 🟡 Moderate = possible, but needs some learning  
- ⚪ Not the main role

| Item | Gradio | Hugging Face Spaces | Streamlit |
|---|:---:|:---:|:---:|
| Main role | App-building framework | Hosting / deployment platform | App-building framework |
| Write Python code for the app | 🟢 | ⚪ | 🟢 |
| Run the app locally | 🟢 | ⚪ | 🟢 |
| Make a simple UI with little code | 🟢 | ⚪ | 🟢 |
| Audio input/output apps | 🟢 | ⚪ | 🟡 |
| Data tables / charts / dashboard-style apps | 🟡 | ⚪ | 🟢 |
| Put the app on the web | 🟡 | 🟢 | 🟢 |
| Best fit for beginners making AI or media demo apps | 🟢 | 🟢 | 🟡 |
| Best fit for beginners making classroom tools or data apps | 🟡 | 🟡 | 🟢 |

## One-line guide
- **Gradio**: best for simple interactive apps, especially audio and AI demos  
- **Hugging Face Spaces**: best for putting an app online and sharing it  
- **Streamlit**: best for classroom tools, dashboards, and data-based apps

## App Link
[Open the app on Hugging Face](https://huggingface.co/spaces/YOUR-USERNAME/YOUR-SPACE-NAME)

## What this app does
- Type a short sentence
- Choose a language
- Click the button
- Listen to the audio

## Languages
- English
- Korean
- Japanese
- Spanish

## Simple source code

```python
import gradio as gr
from gtts import gTTS
import tempfile
import os

LANGUAGE_OPTIONS = {
    "English": "en",
    "Korean": "ko",
    "Japanese": "ja",
    "Spanish": "es"
}

def make_tts(text, language_name):
    if not text.strip():
        return None, "Please enter some text."

    lang_code = LANGUAGE_OPTIONS[language_name]

    try:
        tts = gTTS(text=text, lang=lang_code)

        with tempfile.NamedTemporaryFile(delete=False, suffix=".mp3") as tmp_file:
            temp_path = tmp_file.name

        tts.save(temp_path)
        return temp_path, "Audio created successfully."

    except Exception as e:
        return None, f"Error: {e}"

with gr.Blocks() as demo:
    gr.Markdown("# Multi-language TTS App")
    gr.Markdown("Type a sentence, choose a language, and generate speech.")

    text_input = gr.Textbox(
        label="Text",
        placeholder="Type a short sentence here"
    )

    language_input = gr.Dropdown(
        choices=list(LANGUAGE_OPTIONS.keys()),
        value="English",
        label="Language"
    )

    generate_btn = gr.Button("Generate Audio")

    audio_output = gr.Audio(label="Audio Output")
    message_output = gr.Textbox(label="Message")

    generate_btn.click(
        fn=make_tts,
        inputs=[text_input, language_input],
        outputs=[audio_output, message_output]
    )

demo.launch()
