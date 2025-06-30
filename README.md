
# 🎥 One-to-Many WebRTC Video Conference (Self-Hosted)

A fully self-hosted one-to-many video conferencing web app using **WebRTC**, **Socket.IO**, and **Node.js** with no reliance on third-party services like Twilio, Agora, or Firebase.

---

## ✅ Features

- 🎤 **One Host → Multiple Viewers**
- 🔒 **No external services**
- 🖥️ **Runs in browser (Web App)**
- 🔁 **Realtime via WebRTC**
- 📡 **Custom signaling server with Socket.IO**
- 🌐 **Self-hosted TURN/STUN support via coturn**

---

## 🧱 Architecture Overview

```plaintext
[Host Camera+Mic]
       |
       | <-- WebRTC Media Stream -->
       |
[Signaling Server (Node.js + WebSocket)]
       |
       | <-- Signaling Messages -->
       |
[Multiple Viewers]
```

---

## 📦 Tech Stack

| Purpose            | Tech              |
|--------------------|-------------------|
| Signaling Server   | Node.js + Socket.IO |
| Media Transport    | WebRTC             |
| TURN/STUN Server   | coturn (self-hosted) |
| Frontend           | HTML + JS (or React) |
| Optional Backend   | Express (for serving HTML) |

---

## 🚀 Getting Started

### 1. Clone the Repo

```bash
git clone https://github.com/yourusername/webrtc-one-to-many.git
cd webrtc-one-to-many
```

### 2. Install Dependencies

```bash
npm install
```

---

## 🧠 How It Works

### Host:
- Captures camera & microphone.
- Joins a “room” via signaling server.
- Sends media to each connected viewer using WebRTC peer connections.

### Viewer:
- Connects to the host via Socket.IO.
- Receives an offer → sends back an answer.
- Receives host's media via peer connection.

---

## 🔧 coturn Setup (Self-Hosted TURN/STUN)

To ensure connectivity across NAT/firewalls:

### Install coturn

```bash
sudo apt update
sudo apt install coturn
```

### Configure

Edit `/etc/turnserver.conf`:

```ini
listening-port=3478
fingerprint
lt-cred-mech
realm=yourdomain.com
user=webrtcuser:password
```

### Start coturn

```bash
sudo turnserver -v
```

Make sure port 3478 is open in your firewall.

---

## 💻 Running the App

### 1. Start the Signaling Server

```bash
node server.js
```

The server will listen on `http://localhost:3000`.

### 2. Serve HTML File

You can use any static file server (e.g., `serve`, `http-server`, or Express):

```bash
npx serve .
```

Or add to `server.js`:
```js
app.use(express.static(__dirname));
```

Then access `http://localhost:3000/index.html` in browser.

---

## 📁 File Structure

```
.
├── server.js             # Signaling server (Node.js + Socket.IO)
├── index.html            # Frontend for host & viewers
├── README.md             # You're here!
```

---

## 🌐 TURN/STUN Config in WebRTC

Replace this block in `index.html`:

```js
iceServers: [
  { urls: "stun:yourdomain.com" },
  {
    urls: "turn:yourdomain.com",
    username: "webrtcuser",
    credential: "password"
  }
]
```

---

## ✅ TODO / Future Enhancements

- [ ] Add Viewer UI to view host stream.
- [ ] Add chat feature via Socket.IO.
- [ ] Implement authentication or room codes.
- [ ] Add screen sharing support.
- [ ] Use React or Vue for a richer UI.
- [ ] Use `MediaSoup` or `Janus` for improved scalability.

---

## 🛡️ Notes

- WebRTC requires **HTTPS** in production.
- Make sure coturn and signaling ports are exposed if deploying on cloud.
- If behind NAT, a TURN server is **required** for peer connections.

---

## 🧪 Testing

Open two tabs:
- One as **Host** (initiates stream).
- Others as **Viewers** (receive stream).

Use browser console to debug connection and ICE candidate issues.

---

## 📜 License

MIT License

---

## ✍️ File Struct

stream-app/
├── client/                     # React frontend
│   ├── src/
│   │   ├── components/
│   │   │   ├── HostStream.js   # Host camera + send stream
│   │   │   ├── ViewerPlayer.js # Play .m3u8 + chat
│   │   │   └── ChatBox.js
│   │   └── App.js
│   └── public/
│       └── index.html
│
├── server/                     # Backend
│   ├── stream-server.js        # WebSocket for video chunks
│   ├── chat-server.js          # WebSocket for chat per stream
│   └── public/hls/             # FFmpeg output HLS folder
│
├── nginx.conf                  # RTMP+HLS setup
├── package.json
└── README.md

