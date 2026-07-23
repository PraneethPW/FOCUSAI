# FocusAI

AI-based context-aware smart notification system for student focus and distraction management.

FocusAI helps students protect deep work by learning their study goal, exam track, best study window, daily focus target, and biggest distraction. It uses that profile to generate a personalized focus plan, classify notifications, save focus sessions, and provide AI coaching.

## Features

- Secure signup, login, JWT authentication, and protected routes
- Student onboarding and profile personalization
- Personalized dashboard generated from user inputs
- Focus session creation and saved session history
- Notification analyzer that decides whether to allow, batch, summarize, or mute alerts
- Backend-powered insights from saved sessions and notification decisions
- OpenRouter-powered AI coach with user profile context
- Neon Postgres persistence
- Orange/black glowing responsive UI with motion, testimonials, and concept-specific visuals

## Tech Stack

Frontend:

- React
- Vite
- Tailwind CSS
- Framer Motion
- Lucide React
- React Router

Backend:

- Node.js
- Express
- PostgreSQL via Neon
- JWT authentication
- bcrypt password hashing
- OpenRouter API integration
- Helmet, CORS, Morgan, and rate limiting

## Project Structure

```text
FocusAI/
  frontend/
    src/
    package.json
    vercel.json
    .env.example
  backend/
    src/
      auth.js
      db.js
      openrouter.js
      planner.js
      server.js
    package.json
    railway.json
    .env.example
  README.md
  .gitignore
```

## Local Setup

Install frontend dependencies:

```bash
cd frontend
npm install
npm run dev
```

Install backend dependencies:

```bash
cd backend
npm install
npm run dev
```

Default local URLs:

- Frontend: `http://localhost:5174`
- Backend: `http://localhost:8001`
- Health check: `http://localhost:8001/api/health`

## Environment Variables

Create `frontend/.env`:

```env
VITE_API_URL=http://localhost:8001
```

For Vercel production, set:

```env
VITE_API_URL=https://focusai-production-31f2.up.railway.app
```

If `VITE_API_URL` is not set, the frontend automatically uses:

- `http://localhost:8001` on localhost
- `https://focusai-production-31f2.up.railway.app` when deployed

Create `backend/.env`:

```env
PORT=8001
DATABASE_URL=postgresql://username:password@host/database?sslmode=verify-full
OPENROUTER_API_KEY=your-openrouter-key
OPENROUTER_MODEL=openai/gpt-4o-mini
FRONTEND_URL=http://localhost:5174
JWT_SECRET=replace-with-a-long-random-secret
```

For Railway production, set:

```env
FRONTEND_URL=https://focusai-nine.vercel.app
```

Optional for preview deployments:

```env
FRONTEND_URLS=https://focusai-nine.vercel.app,https://your-preview.vercel.app
```

Never commit real `.env` files. Use `.env.example` for placeholders only.

## API Overview

Authentication:

- `POST /api/auth/signup`
- `POST /api/auth/login`
- `GET /api/auth/me`

Profile:

- `PATCH /api/profile`

Dashboard and focus:

- `GET /api/dashboard`
- `POST /api/sessions`
- `PATCH /api/sessions/:id/complete`

Notification intelligence:

- `POST /api/notifications/analyze`

AI coach:

- `POST /api/coach`

Protected endpoints require:

```http
Authorization: Bearer <token>
```

## Personalization Flow

1. Student creates an account with name, email, and password.
2. Student completes onboarding with:
   - study goal
   - exam or track
   - daily focus target
   - biggest distraction
   - best study window
3. Backend generates a personalized focus plan.
4. Dashboard and insights update from saved sessions and notification decisions.
5. AI coach receives profile context for more relevant responses.

## Deployment

Backend on Railway:

- Root directory: `backend`
- Build: Railway/Nixpacks auto-detects Node
- Start command: `npm start`
- Required environment variables:
  - `NODE_ENV=production`
  - `DATABASE_URL`
  - `OPENROUTER_API_KEY`
  - `OPENROUTER_MODEL=openai/gpt-4o-mini`
  - `JWT_SECRET`
  - `FRONTEND_URL=https://focusai-nine.vercel.app`
  - Do not manually set `PORT`; Railway provides it automatically.

Frontend on Vercel:

- Root directory: `frontend`
- Build command: `npm run build`
- Output directory: `dist`
- Required environment variable:
  - `VITE_API_URL=https://focusai-production-31f2.up.railway.app`

After Railway gives you the backend URL, add it as `VITE_API_URL` in Vercel. After Vercel gives you the frontend URL, add it as `FRONTEND_URL` in Railway so CORS allows the deployed frontend.

Neon:

- Use the Neon pooled connection string.
- Prefer `sslmode=verify-full` in the connection string.

## Validation

Frontend production build:

```bash
cd frontend
npm run build
```

Backend health check:

```bash
curl http://localhost:8001/api/health
```

## Security Notes

- Passwords are hashed with bcrypt.
- JWT tokens expire after 7 days.
- Auth routes are rate limited.
- Helmet is enabled for safer HTTP headers.
- Real secrets are excluded by `.gitignore`.
