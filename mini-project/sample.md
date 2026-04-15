# Multi-language TTS App

This is a simple multi-language text-to-speech app made with Gradio and gTTS.

+ [Gradio](https://gradio.app) is a simple Python tool for building interactive web apps for machine learning or language-learning projects. It allows beginners to create apps with text boxes, buttons, audio, and images without needing complex web programming.
+ [Huggingface](https://huggingface.co/spaces) is an online platform where people can share, test, and publish AI models and web apps.  Students can use Hugging Face Spaces to upload and run Gradio apps on the web so that others can open and use them through a link. 🐥 [sample](FrameAI4687/Omni-Video-Factory)
+ [Streamlit]() is a simple Python framework for building interactive web apps, especially for data exploration, education, and demos. It helps beginners turn Python scripts into usable web pages with buttons, text input boxes, charts, and media display, without needing advanced web development skills.

#### 1. One-line guide
- **Gradio**: best for simple interactive apps, especially audio and AI demos  
- **Hugging Face Spaces**: best for putting an app online and sharing it  
- **Streamlit**: best for classroom tools, dashboards, and data-based apps
  
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
# Installation
!pip install gradio gTTS
```

```python
import gradio as gr          # for making a simple web app
from gtts import gTTS        # for converting text into speech
import tempfile              # for creating a temporary audio file
import os                    # for working with file paths

# language names and their code values
LANGUAGE_OPTIONS = {
    "English": "en",
    "Korean": "ko",
    "Japanese": "ja",
    "Spanish": "es"
}

# function to make speech audio from text
def make_tts(text, language_name):
    # check if the user entered any text
    if not text.strip():
        return None, "Please enter some text."

    # get the language code from the selected language
    lang_code = LANGUAGE_OPTIONS[language_name]

    try:
        # create TTS object with text and language
        tts = gTTS(text=text, lang=lang_code)

        # make a temporary mp3 file
        with tempfile.NamedTemporaryFile(delete=False, suffix=".mp3") as tmp_file:
            temp_path = tmp_file.name

        # save the speech audio into the file
        tts.save(temp_path)

        # return the audio file path and message
        return temp_path, "Audio created successfully."

    except Exception as e:
        # return error message if something goes wrong
        return None, f"Error: {e}"

# build the app screen
with gr.Blocks() as demo:
    # app title and simple instruction
    gr.Markdown("# Multi-language TTS App")
    gr.Markdown("Type a sentence, choose a language, and generate speech.")

    # text box for user input
    text_input = gr.Textbox(
        label="Text",
        placeholder="Type a short sentence here"
    )

    # dropdown menu for language choice
    language_input = gr.Dropdown(
        choices=list(LANGUAGE_OPTIONS.keys()),
        value="English",
        label="Language"
    )

    # button to start audio generation
    generate_btn = gr.Button("Generate Audio")

    # output area for audio and message
    audio_output = gr.Audio(label="Audio Output")
    message_output = gr.Textbox(label="Message")

    # connect button click to the TTS function
    generate_btn.click(
        fn=make_tts,
        inputs=[text_input, language_input],
        outputs=[audio_output, message_output]
    )

# run the app
demo.launch()
```
# DIY on colab

[Goto Colab](https://colab.research.google.com/)
