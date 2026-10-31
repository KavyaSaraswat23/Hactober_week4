# 💀 Spiritus — Virtual Ofrenda Builder & Remembrance Generator

<div align="center">

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js](https://img.shields.io/badge/Node.js-v18%2B-green.svg)](https://nodejs.org/)
[![Ollama](https://img.shields.io/badge/Ollama-Local%20AI-blue.svg)](https://ollama.com/)
[![Gemma 3](https://img.shields.io/badge/Model-Gemma%203%20%2F%20Mistral%20%2F%20Llama-purple.svg)](https://deepmind.google/technologies/gemma/)
[![Hacktoberfest 2026](https://img.shields.io/badge/Hacktoberfest-Week%204%20Challenge-orange.svg)](https://dev.to/challenges/hacktoberfest-week4-2026-10-26)

*“The dead are never truly gone as long as they are remembered.”*

<br/>

![Spiritus Hero Image](./public/hero.jpg)

</div>

---

## 📖 About Spiritus

**Spiritus** is an open-source, privacy-first virtual *ofrenda* (altar) builder and remembrance writer inspired by the rich cultural tradition of **Día de los Muertos** (Day of the Dead) and built for the **Hacktoberfest 2026: Ghost in the Machine** challenge.

In a digital age where memories are routinely scraped, monetized, and trapped behind paid cloud subscriptions, **Spiritus** takes the opposite stance: **your most sacred memories belong to you alone**. Powered by open-weights local AI (Gemma, Mistral, LLaMA) via **Ollama**, all token generation happens directly on your machine. **Zero telemetry. Zero external API keys. Zero cloud fees.**

---

## ✨ Features

- 🕯️ **Interactive Digital Ofrenda**: Place marigold petals (*cempasúchil*), light candles, offer pan de muerto, copal incense, and favorite foods while seeing the live altar update in real-time.
- 🤖 **100% Local AI Generation**: Runs Google's **Gemma 3**, Mistral, or LLaMA 3.2 locally through [Ollama](https://ollama.com). No OpenAI key or external API calls required.
- 📡 **Real-Time Token Streaming**: Server-Sent Events (SSE) stream the AI's remembrance token-by-token with an atmospheric typewriter effect.
- 🪶 **Atmospheric Tones**: Customize the voice and emotional texture of the tribute:
  - 🌸 **Poetic** — Lyrical, nostalgic, and imagery-rich.
  - 🎉 **Celebratory** — Joyous, lively, celebrating a life fully lived (*¡Que viva el recuerdo!*).
  - 🕯️ **Solemn** — Reverent, dignified, quiet peace.
  - 📖 **Storyteller** — Anecdotal, warm, weaving shared moments into narrative.
- 🔊 **Recite Aloud (TTS)**: Built-in gentle Speech Synthesis integration to read the remembrance aloud.
- 💬 **Multi-Turn Remembrance Chat**: Talk with the model after the remembrance is drafted — request a poem, translate into Spanish, add a custom stanza, or refine a memory.
- 💾 **Local Persistence & Export**: Download the remembrance as a `.txt` file, export ofrenda configurations, and persist saved ofrendas to local JSON storage.
- ⚡ **Offline Demo Mode**: No Ollama installed yet? Seamless fallback demo mode lets anyone experience the full interactive workflow instantly.
- 🎨 **Rich Gothic-Warm Aesthetic**: Responsive, dark-mode design with glowing embers, floating gold petals, and traditional Mexican folkloric accents.

---

## 🛠️ Tech Stack

| Layer | Technology | Details |
|---|---|---|
| **Frontend** | Vanilla HTML5, CSS3, JavaScript | Zero build step, lightweight, blazing fast |
| **Backend** | Node.js & Express | Proxy layer for Ollama, SSE streaming, file persistence |
| **AI Runtime** | [Ollama](https://ollama.com) | Local model serving with low-latency inference |
| **Recommended Models** | `gemma3:4b`, `gemma3:1b`, `llama3.2:3b` | Open-weight foundation models |
| **Persistence** | File-system JSON (`data/ofrendas.json`) | Self-contained, portable, private |

---

## 📐 Architecture

```
                    ┌─────────────────────────────────────────┐
                    │               Web Browser               │
                    │   (Interactive Altar UI & Particle FX)  │
                    └────────────────────┬────────────────────┘
                                         │
                        HTTP / SSE (Stream & Chat)
                                         │
                                         ▼
                    ┌─────────────────────────────────────────┐
                    │          Express Backend Server         │
                    │               (server.js)               │
                    └──────────────┬──────────────────┬───────┘
                                   │                  │
                Persists to Local  │                  │  Local In-Memory
                 JSON (`/data`)    │                  │  Proxy & Formatting
                                   ▼                  ▼
                    ┌──────────────────┐    ┌─────────────────┐
                    │ ofrendas.json    │    │  Ollama Engine  │
                    │ (Saved Altars)   │    │  (127.0.0.1)    │
                    └──────────────────┘    └────────┬────────┘
                                                     │
                                            ┌────────▼────────┐
                                            │ Gemma / Mistral │
                                            │ (Open Weights)  │
                                            └─────────────────┘
```

---

## 🚀 Quick Start Guide

### Prerequisites
1. **Node.js**: v18.0.0 or higher ([Download Node.js](https://nodejs.org/))
2. **Git**: Installed on your system
3. **Ollama** *(Optional, recommended for AI features)*: ([Download Ollama](https://ollama.com/))

---

### Step 1: Clone the Repository

```bash
git clone https://github.com/KavyaSaraswat23/Hactober_week4.git
cd Hactober_week4
```

### Step 2: Install Dependencies

```bash
npm install
```

### Step 3: Run Ollama & Pull a Model *(Optional)*

Open a new terminal window to start Ollama and download the lightweight Gemma 3 model:

```bash
# Start the Ollama background daemon
ollama serve

# In a separate terminal, pull your preferred model:
ollama pull gemma3:4b
# Or for lower memory footprint:
ollama pull gemma3:1b
```

> 💡 **Tip:** If you do not have Ollama running, Spiritus will automatically provide a **Demo Mode** with pre-composed poetic remembrances so you can still explore all features!

### Step 4: Launch the Server

```bash
# Production mode:
npm start

# Or development mode with auto-reload:
npm run dev
```

The application will be live at:
👉 **[http://localhost:3000](http://localhost:3000)**

---

## 🔌 API Endpoints

The Express server exposes the following REST & Streaming routes:

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/health` | Healthcheck and server status |
| `GET` | `/api/ollama/status` | Tests connection to `localhost:11434` and lists models |
| `GET` | `/api/ollama/models` | Returns available downloaded Ollama models |
| `POST` | `/api/generate` | Server-Sent Events (SSE) token stream for remembrance creation |
| `POST` | `/api/chat` | Multi-turn chat SSE stream to converse about the remembrance |
| `POST` | `/api/demo` | Fallback canned remembrance generator (no Ollama required) |
| `POST` | `/api/ofrenda/save` | Persists an ofrenda entry to `data/ofrendas.json` |
| `GET` | `/api/ofrendas` | Retrieves all saved ofrenda records |

---

## ⚙️ Environment Variables

You can configure optional environment variables by setting them before starting:

```bash
PORT=3000                           # Default: 3000
OLLAMA_URL=http://localhost:11434   # Default: http://localhost:11434
```

---

## 🌟 Why Open Innovation & Local AI?

Remembrances written for deceased ancestors, mentors, friends, and family are intimate and sacred. 

* **Complete Privacy**: No third-party API logs your family stories or private grief.
* **Permanence**: Closed AI APIs deprecate endpoints, change terms, or shut down. Your local model file (`.gguf`) remains functional as long as your computer turns on.
* **Independence**: Accessible in remote locations without internet connectivity.
* **No Subscriptions**: Honoring your loved ones should never be contingent on a monthly subscription fee.

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!
Feel free to check the [issues page](https://github.com/KavyaSaraswat23/Hactober_week4/issues).

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📜 License

Distributed under the **MIT License**. See [`LICENSE`](./LICENSE) for more information.

---

<div align="center">
  <sub>Built with 💛 and marigold petals for Hacktoberfest 2026. ¡Que vivan los recuerdos!</sub>
</div>
