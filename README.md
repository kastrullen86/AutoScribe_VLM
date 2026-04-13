# AutoScribe VLM

An adaptive media-to-markdown pipeline designed for **NotebookLM**. It automatically transcribes audio and performs visual frame analysis (VLM) for silent or visual-only content.

## 🚀 Key Features

- **Smart Environment (Resilient Setup):** Block 1 now detects existing Google Drive mounts and pre-installed dependencies. If the kernel restarts, re-running the setup takes seconds instead of minutes.
- **Adaptive AI Engine:** Automatically scales the transcription model based on file duration.
  - **Files < 4 hours:** Uses `Whisper large-v3` for maximum precision.
  - **Files > 4 hours:** Switches to `Whisper medium` and activates `VAD (Voice Activity Detection)` filters to prevent memory overflows (OOM) on massive datasets.
- **Intelligent Caching:** Block 2 checks the local NVMe storage before downloading. If a file (YouTube or Drive) is already cached from a previous run, it skips the ingestion phase.
- **YouTube Bot-Bypass:** Integrated Android client spoofing to ensure stable downloads even from Google Colab's data center IP ranges.
- **Local-First I/O:** All processing occurs on local NVMe for maximum throughput, with a final batch export to Google Drive.

## 🔑 Prerequisites

1. **Hugging Face Token:** - Create a **Read** token at [huggingface.co](https://huggingface.co/).
   - Add it to Colab **Secrets** (🔑 icon) as `HF_TOKEN` and enable "Notebook access".
2. **Hardware:** Use a **GPU Runtime** (`Runtime > Change runtime type` -> T4 or better).

## 🛠️ Usage Workflow

1. **Block 1:** Synchronize the environment. Smart detection will skip redundant installations.
2. **Block 2:** Configure your source (URL or Drive). Caching logic ensures no redundant downloads.
3. **Block 3:** Execute AI processing. The engine scales the model dynamically to ensure stability for long-form content.
4. **Block 4:** Finalize, export debug logs, and disconnect to preserve Compute Units.

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
