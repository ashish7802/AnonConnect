# AnonConnect

AnonConnect is a production-ready anonymous full-stack app with:

- Real-time room-based chat (`/chat` namespace)
- WebRTC voice/video calls with Socket.io signaling (`/call` namespace)
- In-memory room, user, and call state (no database)
- Vanilla HTML/CSS/JS frontend

## Features

- Anonymous random usernames and room switching
- Live online count + active room list with user counts
- Optional private rooms with in-memory password protection
- Typing indicators, system join/leave events, emoji picker
- Room message history (last 50 messages) synced on join
- Message reactions with live updates
- Base64 image sharing for files under 2MB
- Optional disappearing messages via room TTL (5m / 1h / 24h)
- Room auto-cleanup after 5 minutes of inactivity (0 users)
- In-memory per-socket message rate limiting (30 messages/minute)
- HTTP rate limiting + secure headers with `helmet`
- Multi-peer call room support (up to 4 remote peers)
- Mute/camera/screen-share controls and call timer
- In-call side panel chat and participants status

## Project Structure

```txt
AnonConnect/
├── server.js
├── package.json
├── .env.example
├── public/
│   ├── index.html
│   ├── chat.html
│   ├── call.html
│   ├── css/
│   │   ├── main.css
│   │   ├── chat.css
│   │   ├── call.css
│   │   └── components.css
│   ├── js/
│   │   ├── socket-client.js
│   │   ├── chat.js
│   │   ├── webrtc.js
│   │   ├── call.js
│   │   └── utils.js
│   └── assets/icons/
```

## Setup

1. Install dependencies
   ```bash
   npm install
   ```
2. Copy environment template
   ```bash
   cp .env.example .env
   ```
3. Start server
   ```bash
   npm start
   ```
4. Open `http://localhost:3000`

## Development

```bash
npm run dev
```

## Environment Variables

- `PORT`: server port (default `3000`)
- `NODE_ENV`: runtime mode
- `CORS_ORIGIN`: allowed origin (`*` by default)

## Socket Events

### `/chat`
- `welcome`
- `join-room`
- `join-room-error`
- `send-message`
- `send-file`
- `add-reaction`
- `reaction-updated`
- `message-deleted`
- `typing-start`
- `typing-stop`
- `get-rooms`
- `create-room`
- `online-count`
- `rate-limit-hit`

### `/call`
- `join-call-room`
- `webrtc-offer`
- `webrtc-answer`
- `webrtc-ice-candidate`
- `call-request`
- `call-accepted`
- `call-rejected`
- `call-ended`
- `mute-toggle`
- `video-toggle`

## Notes

- In-memory state is intentionally volatile; restarting server resets rooms/users/calls.
- For production at scale, add Redis adapter for Socket.io and persistent backing services.
- WebRTC P2P works best over HTTPS in production and may need TURN servers for strict NATs.
