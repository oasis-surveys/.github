<p align="center">
  <img src="https://oasis-surveys.github.io/oasis-logo.png" alt="OASIS Logo" width="200"/>
</p>

<h1 align="center">OASIS</h1>
<h3 align="center">Open Agentic Survey Interview System</h3>

<p align="center">
  A self-hosted platform for conducting AI-powered conversational interviews at scale.
  <br/>
  Built for survey researchers, qualitative methodologists, and anyone who needs structured or open-ended data collection through voice or text.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white" alt="Python"/>
  <img src="https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white" alt="FastAPI"/>
  <img src="https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=black" alt="React"/>
  <img src="https://img.shields.io/badge/Tailwind_CSS-38B2AC?logo=tailwindcss&logoColor=white" alt="Tailwind"/>
  <img src="https://img.shields.io/badge/PostgreSQL-16-4169E1?logo=postgresql&logoColor=white" alt="PostgreSQL"/>
  <img src="https://img.shields.io/badge/Redis-7-DC382D?logo=redis&logoColor=white" alt="Redis"/>
  <img src="https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white" alt="Docker"/>
  <img src="https://img.shields.io/badge/Pipecat-AI_Pipeline-black" alt="Pipecat"/>
  <img src="https://img.shields.io/badge/LiteLLM-Multi_Provider-orange" alt="LiteLLM"/>
</p>

<p align="center">
  <a href="https://oasis-surveys.github.io">Website</a> · <a href="https://oasis-surveys.github.io/docs">Docs</a> · <a href="https://oasis-surveys.github.io/about">About</a> · <a href="https://oasis-surveys.github.io/license">License</a>
</p>

---

## What is OASIS?

OASIS is an open-source research platform that lets you run conversational AI interviews from your own infrastructure. You define the study, configure an agent with a system prompt and model of your choice, and share a link (or phone number) with participants. Transcripts are diarized and stored in your database. You stay in control of the data, the models, and the entire pipeline.

It was built out of a simple frustration: existing commercial tools for conversational AI are powerful, but they are not designed with research methodology in mind. Things like follow-up probing, semi-structured interview guides, participant identifiers, and study-level organization tend to be afterthoughts. OASIS puts those features front and center.

## Demo

<p align="center">
  <strong>1. Setup a Study</strong><br/><br/>
</p>

https://github.com/user-attachments/assets/25633a6b-26c5-4030-89d9-40638b67aff7

<p align="center">
  <strong>2. Create and Configure an Agent</strong><br/><br/>
</p>

https://github.com/user-attachments/assets/4ac63d6e-fdbd-44da-818d-3a4e53417ddc

<p align="center">
  <strong>3. Collect and Manage Data</strong><br/><br/>
</p>

https://github.com/user-attachments/assets/bbf5a613-a28a-4dd0-a962-9250ac1f05a1

## Features

**Two interview modalities**
- Voice interviews (real-time speech with STT, LLM, and TTS)
- Text chat interviews (clean chat UI with customizable avatars)

**Flexible pipeline architecture**
- Modular pipeline: chain any STT, LLM, and TTS provider independently
- Voice-to-voice pipeline: stream audio directly to multimodal models (OpenAI Realtime, Gemini Live)

**Semi-structured interview mode**
- Define a question guide with preset questions, follow-up probes, and transition logic
- The agent follows a structured backbone while still allowing natural conversation
- Upload interview guides via CSV or JSON, or configure them in the dashboard

**Multi-provider model support**
- OpenAI (GPT-4o, GPT-4.1, GPT-5, Realtime models)
- Google Gemini (including native audio preview)
- Scaleway (European-hosted, OpenAI-compatible)
- Azure OpenAI and GCP Vertex AI (self-hosted endpoints)
- Any LiteLLM-compatible provider, plus a custom model identifier option

**STT and TTS providers**
- Deepgram, Scaleway Whisper (STT)
- ElevenLabs, Cartesia, Scaleway (TTS)

**Research-first design**
- Study-level organization with multiple agents per study
- Participant identifiers: random, predefined lists, or self-reported
- Diarized transcripts with timestamps
- Session analytics and data export
- Knowledge base (RAG) with pgvector for document-grounded conversations

**Telephony support (beta)**
- Twilio Media Streams integration for phone-based interviews
- Currently supports incoming calls

**Self-hosted and private**
- Everything runs in Docker on your own machine or server
- No data leaves your infrastructure unless you explicitly configure external API calls
- Optional basic authentication for the admin dashboard
- API keys configurable via `.env` or the dashboard settings page

## Architecture

OASIS runs as five Docker containers on a single internal network:

| Container | Stack | Role |
|-----------|-------|------|
| **Caddy** | Caddy 2 | Reverse proxy, automatic HTTPS |
| **Frontend** | React, Tailwind CSS, Nginx | Admin dashboard and participant interview widget |
| **Backend** | FastAPI, Pipecat, LiteLLM | WebSocket audio/text transport, AI pipeline orchestration, REST API |
| **PostgreSQL** | pgvector/pg16 | Agent configs, transcripts, participant data, vector embeddings |
| **Redis** | Redis 7 | Session tracking, real-time transcript pub/sub, API key overrides |

```
┌──────────────────────────────────────────────────────────┐
│                     Caddy (ports 80/443)                 │
│                    ┌──────────┬──────────┐               │
│                    │ Frontend │ Backend  │               │
│                    │ (React)  │ (FastAPI)│               │
│                    └────┬─────┴────┬─────┘               │
│                         │          │                     │
│                    ┌────┴────┐ ┌───┴───┐                 │
│                    │PostgreSQL│ │ Redis │                 │
│                    └─────────┘ └───────┘                 │
└──────────────────────────────────────────────────────────┘
```

## Goals

OASIS exists to fill a gap. Commercial conversational AI platforms are built for customer support, sales, and marketing. They are good at what they do, but research has different requirements:

- **Methodological control.** Researchers need semi-structured guides, probing logic, and participant tracking built into the tool, not bolted on.
- **Transparency.** When you publish a study, reviewers and readers should be able to understand exactly what system was used, what models were called, and where data was stored.
- **Affordability.** Academic budgets are not enterprise budgets. Self-hosting with pay-as-you-go API keys is often the only realistic option.
- **Data sovereignty.** Especially in Europe, running your own infrastructure is not a nice-to-have. It is a compliance requirement.

The long-term goal is for OASIS to become a standard open-source tool that survey and qualitative researchers can rely on, the same way they rely on tools like Qualtrics for traditional surveys but with conversational AI as the medium.

## Contributing

Contributions are very welcome. If you are interested in contributing, improving the platform, or building on it for your own research, please reach out first. Open an issue or send an email to [max.lang@stx.ox.ac.uk](mailto:max.lang@stx.ox.ac.uk).

For feature requests and bug reports, please use [GitHub Issues](https://github.com/oasis-surveys/oasis-platform/issues).

## License

OASIS is released under the **Open Non-Commercial Research License (ONCRL) v1.0**.

You are free to use, modify, and share OASIS for non-commercial research purposes. If you want to use it in a funded project, a commercial product, or any context that goes beyond personal or academic exploration, you need written approval from the copyright holder.

See the full [LICENSE](LICENSE) file for details.

