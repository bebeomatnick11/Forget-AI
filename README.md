⚠️ Important Security & Billing Notice

This project uses a server-side AI API. The Roblox client must never contain an OpenAI API key.

Required architecture:

Roblox LocalScript
        │
        │ HTTPS
        ▼
Secure Backend Proxy
        │
        │ OPENAI_API_KEY (Environment Variable / Secret)
        ▼
OpenAI API

🔐 API Key Security

- "OPENAI_API_KEY" must exist only on the backend/server.
- Never hard-code the key into Lua, JavaScript, JSON, configuration files, or frontend code.
- Never put the key in the Roblox LocalScript.
- Never use Base64, string splitting, obfuscation, or client-side encryption as a substitute for proper secret storage.
- Never commit ".env" or any file containing the real API key to GitHub.
- ".env.example" must contain only an empty placeholder.
- If an API key has ever been exposed publicly, revoke it and create a new one.

💳 API Usage & Cost

The OpenAI API is a paid API service and API requests may incur charges according to the OpenAI account, billing configuration, model, and usage.

Using ChatGPT/GPT through an OpenAI consumer product is not the same thing as having free API credits.

Before deploying this project publicly, make sure you understand the API pricing and configure appropriate usage limits.

🛡️ Backend Abuse Protection

The backend is responsible for protecting the API account from unauthorized or excessive usage.

At minimum, production deployments should use:

- Rate limiting
- Request/payload size limits
- Input validation
- Request timeouts
- Token/output limits
- Error handling
- No API-key logging
- Appropriate authentication or per-user/session restrictions where possible

Important: Hiding the OpenAI API key does not automatically make the backend secure. If the backend endpoint is publicly accessible without authentication or sufficient rate limiting, other people may be able to abuse it and consume the owner's API quota or billing.

🌐 Public GitHub Repository

This repository is designed so that the source code can be public without exposing the OpenAI API key.

The public repository should contain:

.env.example       ← safe placeholder
.gitignore         ← prevents secrets from being committed
backend/           ← backend source code
roblox/            ← Roblox LocalScript
README.md

The real secret should exist only as:

backend/.env

during local development, or as a Secret / Environment Variable provided by the deployment platform in production.

Do not publish "backend/.env".
# Roblox AI Companion

AI Character đồng hành trong Roblox, sử dụng avatar của người chơi, có animation, pathfinding, follow, bubble chat và chat UI.

**Kiến trúc bảo mật:** API key của xAI **không bao giờ** nằm trong Roblox script hay source code public. Tất cả request đi qua Secure Backend Proxy.

## Tính năng chính

- Clone appearance (avatar) của người chơi (R6/R15)
- Animation đầy đủ: Idle, Walk, Run, Jump, Fall, Climb, Sit...
- Pathfinding + Follow player thông minh
- Tự động teleport khi khoảng cách > 60 studs
- Phát hiện bị kẹt và tự phục hồi
- Bubble Chat + trạng thái "Đang suy nghĩ..."
- Chat UI responsive (PC + Mobile)
- Action system an toàn (FOLLOW, STOP, JUMP, SIT...)
- Memory hội thoại ngắn hạn
- Backend bảo mật với rate limit + validation

## Kiến trúc
Roblox LocalScript
│
│  HTTPS (chỉ biết BACKEND_URL)
▼
Secure Backend Proxy
│
│  XAI_API_KEY (Environment Variable / Secret)
▼
xAI API (https://api.x.ai/v1)
API key chỉ tồn tại ở phía server.
Không hard-code, không Base64, không obfuscate, không lưu trong client.

Cấu trúc thư mục

roblox-ai-companion/
├── .gitignore
├── .env.example
├── README.md
├── backend/
│   ├── package.json
│   └── server.js
└── roblox/
└── AICompanion.client.lua

1. Triển khai Backend (Bắt buộc)

Yêu cầu

Node.js 18+

Tài khoản Render, Railway hoặc tương tự


Cách làm nhanh với Render

1. Fork / clone repo này lên GitHub của bạn.


2. Vào Render.com → New → Web Service.


3. Connect GitHub repository.


4. Cấu hình:

Root Directory: backend

Build Command: npm install

Start Command: npm start



5. Thêm Environment Variables (Secrets):



Key	Value	Ghi chú

XAI_API_KEY	your_new_xai_api_key	Key mới từ console.x.ai
PORT	10000	Render tự set
RATE_LIMIT_MAX	15	Request / phút


6. Deploy → copy URL (ví dụ: https://your-app.onrender.com)



Chạy local (để test)

cd backend  
cp ../.env.example .env  
# Sửa .env và điền XAI_API_KEY  
npm install  
npm start  
2. Cài đặt trong Roblox  
Mở Roblox Studio.  
Vào StarterPlayer → StarterPlayerScripts.  
Tạo LocalScript tên AICompanion.  
Copy toàn bộ nội dung file roblox/AICompanion.client.lua vào.  
Sửa dòng cấu hình:  
BACKEND_URL = "https://your-app.onrender.com/api/chat", -- ← URL backend của bạn  
Bật HTTP Requests:  
Game Settings → Security → Allow HTTP Requests = ✅  
Play để test.  
Lưu ý: Bạn cũng có thể dùng cho exexutor của bạn
3. Bảo mật quan trọng  
Không bao giờ commit file .env.  
File .env.example chỉ chứa placeholder.  
Nếu bạn từng đưa API key vào code trước đây → Revoke key cũ ngay tại console.x.ai và tạo key mới.  
Backend có:  
Rate limiting  
Payload size limit  
Input validation  
Timeout  
Không log API key  
4. Cách sử dụng trong game  
Vào game → AI Character sẽ spawn gần bạn (dùng avatar của bạn).  
Bấm nút 🤖 góc phải dưới để mở chat.  
Ví dụ lệnh:  
Đi theo tôi → AI follow  
Dừng lại → dừng  
Nhảy đi → nhảy  
Ngồi xuống → ngồi  
Hỏi bất kỳ câu gì → AI trả lời + bubble chat  
5. Lưu ý kỹ thuật  
Script hiện tại là LocalScript (client-side).  
Pathfinding + animation chạy trên client.  
Với game multiplayer lớn, nên chuyển logic AI Character sang ServerScript + RemoteEvent để đồng bộ tốt hơn.  
Model grok-4.5 hoặc grok-4.6 (tùy xAI hỗ trợ tại thời điểm bạn dùng).  
License  
MIT (hoặc license bạn muốn)  
Cảnh báo cuối:  
API key thật chỉ được đặt trong Environment Variables / Secrets của nền tảng deploy.  
Nếu lộ key → chi phí API có thể bị lạm dụng.  
---  
  
### Tóm tắt nhanh bạn cần làm:  
  
1. Tạo file `README.md` ở root repo.  
2. Paste nội dung trên vào.  
3. Sửa các chỗ `your-app.onrender.com` thành URL thật của bạn sau khi deploy.  
4. Commit & push.

   Lưu ý: có thể dùng cả API KEY của OpenAi chứ không chỉ riêng xAi
do khá lười nên tôi nhờ hai AI (GPT và Grok) viết ReadMe. Mong bạn thông cảm;)
