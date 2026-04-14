# AutoScribe VLM (v2.0.8 - The Enterprise Stability Update)

AutoScribe VLM is an automated, fault-tolerant AI ingestion and processing pipeline. Designed to run in Google Colab, it dynamically routes media (from YouTube or Google Drive) through either Whisper (for audio transcription) or Moondream2 (for visual frame analysis), producing clean, NotebookLM-ready Markdown files.

## 🚀 Key Features

* **Smart Routing Engine:** Automatically analyzes media streams via `ffprobe` to determine if a file contains audio. Audio files are routed to Whisper, while silent or visual-only files are routed to Moondream2 for frame-by-frame analysis.
* **Multi-Stage Adaptive Scaling:** Prevents VRAM exhaustion on T4 GPUs by dynamically shifting Whisper model sizes based on media duration:
  * `< 2 hours:` `large-v3` (Maximum Precision)
  * `2 - 5 hours:` `medium` (Balanced Performance)
  * `5 - 8 hours:` `small` (Enhanced Stability)
  * `> 8 hours:` `tiny` (Secure / Resource Optimized)
* **Bulletproof Fault Tolerance:** Engineered for large batch processing. If a single video in a playlist fails to download or process, the system logs the error and gracefully continues to the next item without terminating the session.
* **Collision-Proof File Naming:** Output files automatically append unique source IDs (e.g., YouTube IDs) to prevent data overwriting when processing media with similar titles.
* **Mise en Place Data Structure:** Generates a pristine workspace for RAG systems (like NotebookLM). Outputs are saved in dynamically named session folders based on the playlist/file name, with debug logs securely isolated in a dedicated `logs` subdirectory.
* **Enterprise Dependency Pinning:** Utilizes a strict dependency matrix (`transformers==4.44.2`, Moondream revision pinning) to completely eliminate upstream compatibility crashes ("Dependency Hell").

## 📂 Architecture & Workflow

The pipeline is divided into four distinct operational blocks:

### Block 1: Smart Environment Setup

Mounts your Google Drive, establishes the local NVMe workspace (`/content/AutoScribe_Local`), and enforces the strict library dependency matrix required for stable AI inference.

### Block 2: Ingestion & Routing

Downloads media (via `yt-dlp` with IPv4 routing to bypass network blocks) or copies files from Drive. Uses `ffprobe` to verify audio streams, caches files locally, and queues them for the appropriate AI model.

### Block 3: Adaptive AI Processing

The core inference engine. Loads either Faster-Whisper (FP16 precision with VAD filtering) or Moondream2 (extracting frames at 1/30 FPS) based on the routing queue. Aggressively manages VRAM (`gc.collect`, `torch.cuda.empty_cache`) between tasks to ensure continuous uptime.

### Block 4: Finalize & Disconnect

Performs systematic cleanup. Exports the operational debug log to your Drive (`outputs/<Session_Name>/logs/`), wipes the temporary NVMe storage to prevent data leaks, and safely disconnects the Colab runtime to preserve Compute Units.

## 🛠️ Usage Instructions

1. Open the `.ipynb` notebook in Google Colab.
2. Select a **T4 GPU** runtime.
3. Execute **Block 1** to initialize the environment and connect Google Drive.
4. Input your source (YouTube Playlist URL or Google Drive path) in **Block 2** and execute.
5. Execute **Block 3** and monitor the output as the AI processes your queue.
6. Run **Block 4** to export logs and cleanly terminate the session.

## 📝 Output Example

Your Google Drive will be populated with a structured directory perfectly formatted for immediate import into knowledge bases:

```text
AutoScribe_Workspace/
└── outputs/
    └── <Session_Name>/
        ├── Video_Title_Part_1_abc123.md
        ├── Video_Title_Part_2_xyz789.md
        └── logs/
            └── <Session_Name>_debug.log
```

## ⚠️ Requirements

* Google Colab (Free or Pro) with a GPU runtime assigned.
* Access to Google Drive for output synchronization.
