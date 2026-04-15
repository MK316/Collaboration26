# Multi-language TTS App

This is a simple multi-language text-to-speech app made with Gradio and gTTS.

+ [Gradio](https://gradio.co)
+ [Huggingface](https://huggingface.co)

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
