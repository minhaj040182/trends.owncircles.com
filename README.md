# Trending Tube — Deployment Guide

## Requirements
- Node.js 18 or newer
- A **YouTube Data API v3** key ([get one here](https://console.cloud.google.com/))

## Quick start

```bash
# 1. Copy environment file and fill in your keys
cp .env.example .env
nano .env          # set YOUTUBE_API_KEY and SESSION_SECRET

# 2. Start the server
PORT=3000 YOUTUBE_API_KEY=... SESSION_SECRET=... node dist/index.mjs
```

The server will:
- Serve the React frontend from `public/` at `/`
- Expose the YouTube proxy API at `/api/*`

## Environment variables

| Variable           | Required | Description                        |
|--------------------|----------|------------------------------------|
| `YOUTUBE_API_KEY`  | ✅ Yes   | YouTube Data API v3 key            |
| `SESSION_SECRET`   | ✅ Yes   | Session signing secret             |
| `PORT`             | ✅ Yes   | Port to listen on (e.g. `3000`)    |

## cPanel / Shared Hosting (Node.js app)

1. Upload all files to your hosting directory.
2. In cPanel → **Setup Node.js App**: set the startup file to `dist/index.mjs`.
3. Add the environment variables in the cPanel Node.js app interface.
4. Click **Start**.

## Reverse proxy (Nginx example)

```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

## File structure

```
trending-tube/
├── dist/          ← compiled Node.js backend (self-contained)
│   ├── index.mjs
│   ├── pino-worker.mjs
│   └── ...
├── public/        ← built React frontend (served as static files)
│   ├── index.html
│   └── assets/
├── package.json
├── .env.example
└── README.md
```
