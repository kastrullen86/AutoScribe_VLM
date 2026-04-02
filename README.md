# AutoScribe VLM

An adaptive media-to-markdown pipeline designed to generate high-quality sources for **NotebookLM**. It automatically handles transcription for spoken audio and switches to visual analysis (VLM) for silent or visual-only videos.

## 🚀 Features

- **Multi-Source Support:** Works with YouTube (videos/playlists), Google Drive files, or local uploads via `yt-dlp`.
- **Adaptive Processing:** - **Whisper (Large-v3):** High-speed, high-accuracy transcription for audio.
  - **Moondream2 VLM:** Visual frame analysis for silent videos or demos.
- **Persistent Storage:** Caches Python dependencies on Google Drive to reduce startup time.
- **Fail-Safe Architecture:** Built-in state validators, RAM management, and detailed error logging (`autoscribe_debug.log`).

## 🔑 Prerequisites (Important!)

To bypass Google Colab IP rate-limits and prevent the AI models from freezing during download, you must provide a Hugging Face Token:

1. Create a free account at [Hugging Face](https://huggingface.co/).
2. Go to Settings > Access Tokens and generate a **Read** token.
3. In Google Colab, click the **🔑 Secrets** icon (left sidebar).
4. Add a new secret named `HF_TOKEN` and paste your token. Toggle "Notebook access" to ON.

## 🛠️ Installation & Setup

1. **GitHub:** Clone this repository to your local machine.
2. **Google Colab:** Upload the `src/notebooks/AutoScribe_Master.ipynb` to [Google Colab](https://colab.research.google.com/).
3. **Hardware:** Ensure you select a **GPU Runtime** (T4, V100, or A100) in Colab via `Runtime > Change runtime type`.

## 📖 Usage

1. Run **Block 1** to mount Google Drive, setup logging, and install dependencies.
2. Configure your source (URL or File) in **Block 2**.
3. Execute **Block 3** for AI processing.
4. Run **Block 4** to export the Markdown files to your Drive and shut down the session.

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
