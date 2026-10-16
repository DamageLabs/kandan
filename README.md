# Kandan Core

Modern revival of [Kandan](https://github.com/kandanapp/kandan) — a self-hosted open-source team chat app. Built on Angular 21 + Express/Node/TypeScript with real-time messaging via Socket.io.

> **Status:** Early development. Backend and Angular authentication scaffolded; channel and message UI are still planned.

## Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Angular 21 + TailwindCSS |
| Backend | Express 5 + TypeScript |
| Database | SQLite via Drizzle ORM |
| Real-time | Socket.io |
| Auth | JWT + bcrypt |

## Features (planned)

- Real-time messaging via WebSockets
- Multiple channels
- User authentication (register/login/JWT)
- Admin controls (user approval, suspension, channel management)
- File attachments
- `/me` emote commands
- Typing indicators
- Message history with pagination
- Dark/light mode

## Getting Started

```bash
git clone https://github.com/DamageLabs/kandan.git
cd kandan
npm install

# Server
cd server
cp ../.env.example .env   # edit as needed
mkdir -p data             # parent directory for the example DATABASE_URL
npx drizzle-kit push      # initialize the local development database from the schema
npm run dev               # http://localhost:3200
```

`drizzle-kit` and the server both read `server/.env`. If you change `DATABASE_URL`,
create its parent directory before initializing the database. The `push` command above
is for a new local development database, not a production migration procedure.

In a second terminal, from the repository root:

```bash
cd client
npm start -- --port 4300 --proxy-config proxy.conf.json
```

Open `http://localhost:4300/register` to create the first user (an administrator),
or `/login` to sign in. The current client routes to a channel placeholder after
sign-in; channel and message operations are available through the API and Socket.io below.

## API

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/v1/auth/register` | POST | Register new user |
| `/api/v1/auth/login` | POST | Login, returns JWT |
| `/api/v1/auth/me` | GET | Current user |
| `/api/v1/channels` | GET | List channels |
| `/api/v1/channels/:id` | GET | Channel + recent messages |
| `/api/v1/channels` | POST | Create channel (admin) |
| `/api/v1/messages/channel/:id` | GET | Paginated message history |
| `/api/v1/users` | GET | List users (admin) |

## Socket.io Events

| Event | Direction | Description |
|-------|-----------|-------------|
| `channel:join` | client → server | Join a channel room |
| `channel:leave` | client → server | Leave a channel room |
| `message:send` | client → server | Send a message |
| `message:new` | server → client | New message broadcast |
| `typing:start` | client → server | User started typing |
| `typing:stop` | client → server | User stopped typing |
| `user:connected` | server → client | User joined channel |
| `user:disconnected` | server → client | User left channel |

## License

MIT
