🤖 Forget-AI — REAL AI IN ROBLOX!!!

«A Roblox AI Companion powered by a secure backend proxy and OpenAI.»

Forget-AI is an AI Companion project for Roblox.

The companion can clone the player's avatar, move around the game, follow the player, use animations, respond through chat and bubble chat, remember recent conversation context, and execute a small set of safe in-game actions.

The project is designed around a simple rule:

The Roblox client never receives the OpenAI API key.

---

✨ Features

🤖 AI Companion

- Uses the player's Roblox avatar appearance.
- Supports R6/R15 avatar appearance cloning.
- Creates an AI character near the player.
- Uses Roblox "Humanoid" / "Animator".
- Supports basic character animations.
- Bubble chat responses.
- "Thinking..." state while waiting for the AI.

🧠 AI System

- AI-generated responses.
- Short-term conversation memory.
- Game context sent to the backend.
- Player name and AI name awareness.
- Personality settings.
- Vietnamese / English language settings.
- Structured AI responses containing:
  - "reply"
  - "action"

🎮 Actions

The current client-side action system supports:

FOLLOW_PLAYER
STOP
JUMP
SIT
TELEPORT_NEAR
SAY
IDLE

Unsupported actions are rejected by the client.

🚶 Movement

- Follow player.
- Pathfinding.
- Automatic recovery when stuck.
- Teleport near the player when the distance becomes too large.
- Configurable follow distance.
- Configurable teleport distance.

💬 UI

- Roblox chat interface.
- Mobile-friendly layout.
- PC support.
- AI toggle button.
- Settings panel.
- AI name customization.
- Personality selection.
- Language selection.
- Feedback buttons.

---

🏗️ Architecture

┌──────────────────────────┐
│        Roblox Game       │
│                          │
│  CONFIG AI V3            │
│  LocalScript              │
└────────────┬─────────────┘
             │ HTTPS POST
             ▼
┌──────────────────────────┐
│     Render Backend       │
│                          │
│  Express + Node.js       │
│  /api/chat               │
│                          │
│  Rate limiting           │
│  Input validation        │
│  Request limits          │
└────────────┬─────────────┘
             │
             │ OPENAI_API_KEY
             ▼
┌──────────────────────────┐
│        OpenAI API        │
│                          │
│   Backend-selected model │
└──────────────────────────┘

The Roblox client only knows the backend URL.

The OpenAI API key stays on the backend.

---

📁 Project Structure

Forget-AI/
│
├── backend/
│   ├── package.json
│   └── server.js
│
├── roblox/
│   └── CONFIG AI V3
│
├── .env.example
├── .gitignore
├── Error
└── README.md

---

🔐 Security

Never put your OpenAI API key inside the Roblox script.

Do NOT:

- Hard-code the API key in Lua.
- Put the key in a public GitHub repository.
- Put the key inside JSON files.
- Put the key inside frontend/client code.
- Encode the key with Base64.
- Obfuscate the key.
- Split the key into multiple strings.
- Store the real key inside ".env.example".

The real key should only exist as an environment variable/secret on the backend.

For local development:

backend/.env

For Render:

Environment Variables

If a real API key is ever exposed publicly, revoke it and create a new one.

---

⚙️ Backend

The backend is located at:

backend/server.js

It uses:

- Node.js
- Express
- OpenAI SDK
- Helmet
- CORS
- Express Rate Limit
- dotenv

The backend does not allow the Roblox client to select the AI model.

The model is configured on the backend using:

OPENAI_MODEL

If it is not specified, the backend defaults to:

gpt-5.6-luna

---

🌐 API Endpoints

Health

GET /health

Example:

https://forget-ai.onrender.com/health

A successful response looks similar to:

{
  "ok": true,
  "service": "roblox-ai-proxy",
  "model": "gpt-5.6-luna"
}

Compatibility Health Check

GET /api/healthz

Chat

POST /api/chat

The Roblox client sends:

{
  "message": "Đi theo tôi",
  "playerId": "123456789",
  "context": "{\"playerName\":\"Player\",\"distance\":8}"
}

The backend returns:

{
  "ok": true,
  "reply": "{\"reply\":\"Được, tôi đi theo bạn!\",\"action\":\"FOLLOW_PLAYER\"}"
}

The Roblox client then parses the AI's structured response and executes the validated action.

---

🚀 Deploy with Render

Requirements

- GitHub repository
- Render account
- OpenAI API key
- Node.js 18+

Create a new Web Service on Render and connect this repository.

Use:

Root Directory:
backend

Build command:

npm install

Start command:

npm start

Environment Variables

Add:

OPENAI_API_KEY=your_real_api_key
OPENAI_MODEL=gpt-5.6-luna

Optional:

RATE_LIMIT_WINDOW_MS=60000
RATE_LIMIT_MAX=20
ALLOWED_ORIGINS=*

Render provides the "PORT" environment variable automatically.

Do not commit your real environment variables to GitHub.

---

🎮 Roblox Installation

Open:

Roblox Studio
→ StarterPlayer
→ StarterPlayerScripts

Create a:

LocalScript

Then copy the contents of:

roblox/CONFIG AI V3

into the LocalScript.

---

🌐 Backend URL

The current Forget-AI backend is:

https://forget-ai.onrender.com

The Roblox script currently uses:

BACKEND_URL = "https://forget-ai.onrender.com/api/chat"

You can replace this with your own backend URL if you deploy your own instance.

---

⚠️ Roblox HTTP Requests

Your Roblox experience must allow HTTP requests.

Go to:

Game Settings
→ Security
→ Allow HTTP Requests

Enable it.

Without HTTP requests, the Roblox client cannot communicate with the backend.

---

🧠 Personalization

The companion supports:

Personality

Vui vẻ
Nghiêm túc
Hài hước
Hỗ trợ
Trung lập

Language

Tiếng Việt
English

AI Name

You can customize the displayed AI name from the settings UI.

The current script stores personalization locally through a Roblox Player Attribute.

This is not a server database.

If persistent cross-session storage is required, the project should later use a server-side DataStore system.

---

🧩 Current AI Action System

The AI is instructed to return a JSON object:

{
  "reply": "Được, tôi đi theo bạn!",
  "action": "FOLLOW_PLAYER"
}

The client validates the action against its allowed action list before executing it.

Example:

"Đi theo tôi"
        ↓
FOLLOW_PLAYER
        ↓
AI starts following the player

Another example:

"Nhảy đi"
        ↓
JUMP
        ↓
AI jumps

---

🛡️ Backend Protection

The backend currently includes:

- API-key protection through environment variables.
- Rate limiting.
- Request body size limits.
- Message length limits.
- Context length limits.
- Player ID length limits.
- OpenAI request timeout.
- Input validation.
- Helmet security headers.
- Error handling.
- No API key logging.
- No player message logging.
- No AI response logging.

The backend also prevents the client from selecting a different model.

---

⚠️ Important Limitations

The current companion is primarily client-side.

That means:

AI Character
Pathfinding
Animation
UI
Actions

are handled by the Roblox client.

For a fully synchronized multiplayer AI visible and controllable by everyone, the architecture should eventually move the AI character and authoritative actions to the server using:

ServerScript
+
RemoteEvent

The current version is mainly intended as a client-side AI Companion prototype.

---

🧪 Troubleshooting

AI does not respond

Check:

1. HTTP Requests are enabled.
2. "BACKEND_URL" is correct.
3. Render service is running.
4. "/health" works.
5. OpenAI API key is configured on Render.
6. Roblox Output for HTTP errors.

Test:

https://forget-ai.onrender.com/health

---

"Endpoint not found"

Opening:

https://forget-ai.onrender.com/

is expected to return:

{
  "ok": false,
  "error": "Endpoint not found."
}

The root endpoint is not the API.

Use:

/health

for health checks.

Use:

/api/chat

for AI requests.

---

💳 API Costs

OpenAI API usage may incur charges depending on the account, billing configuration, model, and usage.

Do not assume that having access to ChatGPT means the API is free.

If you make the backend public, use appropriate rate limits and monitor API usage.

---

📜 Credit

The Roblox companion script is intentionally not obfuscated.

If you use the project or its ideas, please give appropriate credit.

Original author:

bebeomatnick / Ng'ĐzBthg
TikTok: Ng'ĐzBthg

Please do not remove the original credit from the script.

---

📄 License

This project currently does not include a dedicated license file.

If you want other people to freely reuse, modify, and redistribute the project, consider adding an explicit open-source license such as MIT.

Until a license is added, GitHub visibility does not automatically grant permission to reuse the code.

---

❤️ About

Forget-AI is an experiment to bring a real AI companion into Roblox.

The goal is to make an AI character that does more than simply return text:

Talk
  ↓
Understand
  ↓
Observe game context
  ↓
Choose an action
  ↓
Move / follow / jump / sit
  ↓
Respond naturally

This project is still evolving.

REAL AI IN ROBLOX!!! 🤖🔥

---

Repository

https://github.com/bebeomatnick11/Forget-AI

Backend:

https://forget-ai.onrender.com
