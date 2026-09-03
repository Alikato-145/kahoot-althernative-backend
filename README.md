# Camp Quiz Backend

This repository contains only the API, Socket.IO realtime game server, Redis live state, MySQL migrations, and media storage. There are no player/Host pages here; deploy it as a long-running Node service on Dokploy.

Required environment variables:

```env
DATABASE_URL=mysql://...
REDIS_URL=redis://...
MEDIA_ROOT=/data/media
PUBLIC_BASE_URL=https://api.quiz.nxsmnt.me
FRONTEND_URL=https://quiz.nxsmnt.me
PORT=3000
```

Camp Quiz is a Thai, Kahoot-inspired game for a projected Host screen and players joining from their phones. A Host creates a quiz with exactly four answers per question, optional question/reveal images, and an explanation. Live rooms use Socket.IO and Redis; MySQL keeps quiz content and final results.

## Quick start

```sh
cp .env.example .env
docker compose up -d
npm ci
npm run db:migrate
npm run dev
```

Open `http://localhost:3000/host` to make a quiz and start a room. Players open `http://localhost:3000/join` (or scan the Host QR code), enter the six-digit PIN, then choose a nickname.

Run checks with:

```sh
npm run test
npm run build
npm run e2e
```

For public HTTPS/WSS setup, database backups, and the five-phone rehearsal checklist, read [the operations guide](docs/operations.md).

For step-by-step Host and player usage, quiz duplication, image uploads, and deployment overview, read [the user guide](docs/user-guide.md).

## Architecture

- Next.js + Socket.IO: UI, REST routes, and real-time game protocol
- Redis: active PINs, players, answer state, timers, and scores
- MySQL: authored quizzes and durable final rankings
- Nginx: HTTPS, WSS upgrade proxy, and `/media/` image delivery

Docker Compose is for local development only. Production runs Node under systemd with local MySQL/Redis and Nginx; it does not require Docker.

## License

[MIT](LICENSE)
