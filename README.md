# ClipForge AI

A local-first starter web app for automatic video formatting and speech subtitles, with a UI for prompt-based video-to-video animation.

## Current features
- Upload a video or provide a **direct HTTPS video-file URL** (MP4/MOV/M4V/WEBM).
- Generate speech subtitles with Faster-Whisper.
- Burn subtitles into the output.
- Export vertical 9:16, landscape 16:9, or square 1:1 MP4.
- Download the SRT subtitle file separately.
- Animation prompt UI and backend integration point.

## Important limitations
This is an MVP starter, not a finished commercial AI editor. It does **not yet** perform semantic highlight selection, silence removal, automatic music selection, sound effects, or real video-to-video generation. The animation endpoint intentionally reports that a provider-specific adapter is required rather than returning a fake result. Processing under ten minutes is not guaranteed; it depends on source duration, hardware, and model size.

Social-media page URLs are not direct video URLs. Upload a video file you own or have permission to edit, or use a direct HTTPS URL to a video file. Respect each platform's terms and copyright.

## Run locally

Requirements: Python 3.11+, FFmpeg installed and available on PATH.

```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
uvicorn app:app --reload
```

Open http://127.0.0.1:8000. The first Whisper run downloads the selected speech model. On CPU, try `WHISPER_MODEL=base` for faster processing or `small` for a better accuracy/speed balance.

## Docker

```bash
docker build -t clipforge-ai .
docker run --rm -p 8000:8000 -v "$(pwd)/work:/app/work" clipforge-ai
```

## Add real video-to-video animation

Choose a provider that explicitly supports video-to-video generation and review its current API documentation, pricing, file limits, and content rules. Add credentials to `.env`, then implement the provider-specific request, upload, polling, and result-download flow inside `/api/animate`. API shapes differ between providers, so there is deliberately no invented generic request.

Never expose provider API keys in the browser. Keep them on the server.

## GitHub

Create a new public or private repository named `clipforge-ai`, then upload/push this project. Do not commit `.env`, source videos, or generated output.
