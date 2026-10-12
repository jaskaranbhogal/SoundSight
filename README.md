# SoundSight

**Making everyday sounds more accessible.**

SoundSight is an accessibility-focused web application designed to help Deaf and hard-of-hearing individuals better understand their surroundings and communicate more easily. By combining real-time speech transcription, environmental sound recognition, and text-to-speech technology, SoundSight helps users stay connected to the world around them.

Built with Next.js, TypeScript, Python, and FastAPI, SoundSight brings together several AI-powered audio processing technologies in a mobile-friendly interface.

**[View SoundSight on Devpost](https://devpost.com/software/soundsight-qlvn5y)**

---

## Features

### Live Speech Captions
- Transcribes spoken conversations in real time using Deepgram.
- Displays live captions in an accessible interface.
- Organizes detected speech into conversation sessions.
- Maintains temporary conversation history during the browser session.

### Environmental Sound Recognition
- Uses Google's YAMNet model to identify environmental sounds.
- Recognizes selected sound categories, including:
  - Emergency sirens
  - Doorbells
  - Dog barking
  - Car horns
- Displays recognized sounds with labels and severity indicators.
- Includes sound detection history and sample alerts for demonstrations.

### Type to Speak
- Converts typed messages into spoken audio.
- Integrates ElevenLabs for AI-powered text-to-speech.
- Falls back to the browser's built-in speech synthesis when necessary.
- Includes frequently used phrases for quick communication.
- Supports voice selection, playback speed adjustments, and replay functionality.

### Accessibility-Focused Interface
- Mobile-friendly, responsive design.
- Simple navigation between application features.
- Customizable application preferences.
- Clear visual indicators for listening status and alerts.
- Shared microphone session across application screens.

---

## Tech Stack

| Category | Technologies |
|----------|--------------|
| Frontend | Next.js, React, TypeScript |
| Styling | Tailwind CSS |
| Backend | Python, FastAPI |
| Real-Time Communication | WebSockets |
| Speech Transcription | Deepgram |
| Environmental Sound Detection | TensorFlow, YAMNet |
| Text-to-Speech | ElevenLabs, Web Speech API |
| Audio Processing | Picovoice Web Voice Processor |
| Testing | pytest, Node.js Test Runner, ESLint |

---

## How SoundSight Works

SoundSight combines multiple audio processing technologies to provide real-time accessibility features.

1. **Audio Capture:** The browser captures microphone audio with the user's permission.

2. **Audio Streaming:** The frontend processes the microphone input and streams audio to the FastAPI backend using WebSockets.

3. **Environmental Sound Detection:** When configured, YAMNet analyzes incoming audio to identify selected environmental sounds.

4. **Live Speech Transcription:** Deepgram processes microphone audio and returns speech transcription results to the frontend.

5. **Sound Alerts:** Recognized environmental sounds are displayed as visual alerts within the application.

6. **Text-to-Speech:** Users can type messages that are converted into spoken audio using ElevenLabs or the browser's speech synthesis engine.

The application is designed around a shared listening session, allowing users to navigate between screens without repeatedly activating their microphone.

---

## Getting Started

Follow the instructions below to run SoundSight locally.

### Prerequisites

Make sure the following are installed:

- Python 3.13
- Node.js 22.18+ or 24+
- npm
- Git
- A modern web browser with microphone access

Optional API credentials:

- Deepgram API key for live transcription
- ElevenLabs API key for AI-generated speech

Environmental sound detection requires downloading the YAMNet model.

### 1. Clone the Repository

```bash
git clone https://github.com/jaskaranbhogal/SoundSight.git
cd SoundSight
```

### 2. Backend Setup

Navigate to the backend directory:

```bash
cd backend
```

Create a Python virtual environment:

```bash
python3.13 -m venv .venv
```

Activate the virtual environment:

**macOS/Linux:**

```bash
source .venv/bin/activate
```

**Windows:**

```powershell
.venv\Scripts\Activate.ps1
```

Install the required dependencies:

```bash
python -m pip install -r requirements.txt
```

Create the environment configuration file:

```bash
cp .env.example .env
```

Configure the necessary environment variables inside `backend/.env`.

Example:

```env
DEEPGRAM_API_KEY=your_deepgram_api_key
ELEVENLABS_API_KEY=your_elevenlabs_api_key

FRONTEND_ORIGIN=http://localhost:3000
SOUNDSIGHT_ALLOWED_ORIGINS=http://localhost:3000,http://localhost:3001,http://127.0.0.1:3000
```

Additional settings, including the ElevenLabs voice configuration and YAMNet model path, are documented in `backend/.env.example`.

**Important:** Never commit actual API keys or `.env` files to GitHub.

#### Optional: Enable YAMNet Sound Detection

Download the YAMNet model:

```bash
python scripts/download_yamnet.py
```

Add the model path to `backend/.env`:

```env
YAMNET_MODEL_DIR=.models/yamnet
```

#### Start the Backend

Run:

```bash
python -m uvicorn app.main:app --reload
```

The backend runs at:

http://localhost:8000

Verify that the backend is working:

http://localhost:8000/health

FastAPI documentation is available at:

http://localhost:8000/docs

### 3. Frontend Setup

Open a second terminal.

From the repository root, navigate to the frontend:

```bash
cd frontend
```

Install dependencies:

```bash
npm install
```

Create the frontend environment file:

```bash
cp .env.example .env.local
```

The default configuration connects to the local FastAPI backend.

Example:

```env
NEXT_PUBLIC_BACKEND_WS_URL=ws://localhost:8000/ws/listen
NEXT_PUBLIC_BACKEND_HTTP_URL=http://localhost:8000
```

Start the development server:

```bash
npm run dev
```

Open the application at:

http://localhost:3000

**Note:** Microphone access requires HTTPS or localhost. Accessing the application through an unsecured HTTP connection on another device may prevent microphone functionality.

---

## Using SoundSight

### Start Listening

1. Open SoundSight in your browser.
2. Select **Start Listening**.
3. Allow microphone permissions when prompted.
4. The application begins processing microphone audio.
5. Navigate between the available features while the listening session remains active.

### Live Captions

1. Configure a valid Deepgram API key in the backend.
2. Start listening.
3. Open the Captions screen.
4. Speak near the microphone.
5. View incoming transcription results and conversation history.

### Environmental Sound Alerts

1. Download and configure the YAMNet model.
2. Start listening.
3. Open the Alerts screen.
4. View recognized environmental sounds as they are detected.
5. Explore the separate sample activity section to demonstrate the interface.

### Type to Speak

1. Navigate to the Type to Speak screen.
2. Enter a message or select a quick phrase.
3. Configure the available voice preferences.
4. Select **Speak** to play the message aloud.
5. Stop or replay the message as needed.

---

## Testing

SoundSight includes automated tests for portions of the frontend and backend functionality.

### Backend Tests

Navigate to the backend directory and activate the Python virtual environment.

Run:

```bash
python -m pytest
```

### Frontend Tests

Navigate to the frontend directory.

Run:

```bash
node --test tests/*.test.mjs
```

Run the frontend linter:

```bash
npm run lint
```

### Manual Testing

In addition to automated testing, important functionality should be verified manually.

Recommended checks include:

- Microphone permission handling.
- Starting and stopping microphone capture.
- Audio streaming to the backend.
- Live speech transcription.
- Environmental sound recognition.
- Navigation between screens during an active listening session.
- Text-to-speech playback.
- Error handling when API services are unavailable.
- Mobile browser compatibility.

For additional technical details, see:

- [Frontend Documentation](frontend/README.md)
- [Backend Documentation](backend/README.md)

---

## Current Development Status

SoundSight is an actively developed prototype.

### Implemented Functionality

- Responsive frontend interface.
- Microphone capture and WebSocket audio streaming.
- FastAPI backend integration.
- Deepgram live transcription integration.
- YAMNet environmental sound classification integration.
- In-app recognized sound events.
- Type-to-speech interface with ElevenLabs integration.
- Browser text-to-speech fallback.
- Shared microphone state across application screens.
- Frontend and backend automated tests.

Some functionality requires API credentials or additional local configuration before it can operate.

### Current Limitations

- Live transcription requires Deepgram credentials and an active network connection.
- Environmental sound detection requires a configured YAMNet model.
- Conversation and sound history are stored temporarily in browser memory.
- Data is not currently persisted to a database.
- Browser push notifications and lock-screen alerts are not implemented.
- Background listening is not supported.
- The current application is designed for an active browser tab.
- Full offline PWA functionality has not been implemented.

Environmental sound recognition is probabilistic and should not be relied upon as a safety-critical emergency detection system.

---

## Project Structure

```text
SoundSight/
├── backend/
│   ├── app/
│   │   ├── audio/
│   │   ├── events/
│   │   ├── config.py
│   │   ├── deepgram_auth.py
│   │   ├── main.py
│   │   └── tts.py
│   ├── scripts/
│   ├── tests/
│   ├── requirements.txt
│   └── README.md
│
├── frontend/
│   ├── app/
│   │   ├── alerts/
│   │   ├── captions/
│   │   ├── settings/
│   │   ├── speak/
│   │   └── page.tsx
│   ├── components/
│   ├── lib/
│   ├── public/
│   ├── tests/
│   ├── package.json
│   └── README.md
│
├── CHANGELOG.md
├── CLAUDE.md
└── README.md
```

---

## Contributors

SoundSight was developed as a collaborative team project focused on improving accessibility through modern web technologies and AI-powered audio processing.

This repository is a fork of the original SoundSight project.

**Original Repository:** [DilverSingh23/soundsight](https://github.com/DilverSingh23/soundsight)

**Forked Repository:** [jaskaranbhogal/SoundSight](https://github.com/jaskaranbhogal/SoundSight)

For additional information about the project and the team, visit the Devpost submission.

---

## Devpost

Learn more about SoundSight, its development, and the team behind the project:

**[SoundSight — Devpost Project Submission](https://devpost.com/software/soundsight-qlvn5y)**

---

## Acknowledgments

SoundSight uses technologies and services developed by:

- [Deepgram](https://deepgram.com/)
- [ElevenLabs](https://elevenlabs.io/)
- [Google YAMNet](https://www.tensorflow.org/hub/tutorials/yamnet)
- [Next.js](https://nextjs.org/)
- [FastAPI](https://fastapi.tiangolo.com/)
- [Picovoice](https://picovoice.ai/)

SoundSight was built with the goal of making communication and environmental awareness more accessible through technology.
