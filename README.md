# Generator CMS

Generator CMS is a full-stack AI app for:
- AI content generation (rewrite, expand, shorten, article, SEO)
- AI image generation with history
- User authentication and protected routes

## Tech Stack

- Frontend: React, Vite, Tailwind CSS, React Router, Axios, React Hook Form, Zod
- Backend: Node.js, Express, MongoDB (Mongoose), Redis (optional cache)
- AI services: Hugging Face (image), Gemini (content)
- Media storage: Cloudinary

## Project Structure

```
generator-cms/
  backend/
  frontend/
```

## Prerequisites

- Node.js 18+ (Node 20+ recommended)
- npm
- MongoDB connection string
- Cloudinary account
- Hugging Face API key
- Gemini API key
- (Optional) Redis

## Environment Variables

Create `backend/.env`:

```env
PORT=8000
MONGO_URL=your_mongodb_connection_string
SECRET_KEY=your_jwt_secret
HUGGING_FACE_API_KEY=your_huggingface_key
CLOUDINARY_API_KEY=your_cloudinary_key
CLOUDINARY_API_SECRET=your_cloudinary_secret
CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
GEMINI_API_KEY=your_gemini_key

# Optional Redis
REDIS_HOST=127.0.0.1
REDIS_PORT=6379
REDIS_USERNAME=default
REDIS_PASSWORD=your_password
```

Create `frontend/.env`:

```env
VITE_API_BASE_URL=http://localhost:8000
```

## Installation

Install backend dependencies:

```bash
cd backend
npm install
```

Install frontend dependencies:

```bash
cd ../frontend
npm install
```

## Run in Development

Start backend:

```bash
cd backend
npm run dev
```

Start frontend (new terminal):

```bash
cd frontend
npm run dev
```

Frontend URL: `http://localhost:5173`  
Backend URL: `http://localhost:8000`

## Build Frontend

```bash
cd frontend
npm run build
```

## API Overview

Base: `/v1`

- Auth
  - `POST /auth/sign-up`
  - `POST /auth/sign-in`
- Content (protected)
  - `POST /content/:action`
  - `GET /content/history`
  - `GET /content/search`
  - `GET /content/:id`
- Image (protected)
  - `POST /image/generate`
  - `GET /image/history`

## Notes on Image Generation

- Image generation can be slow depending on provider load.
- Backend includes timeout handling for AI generation and upload.
- Frontend request timeout is configured to avoid indefinite loading.
- Redis is optional. If Redis is unavailable, the app continues without caching.

## Troubleshooting

### Image not generating

1. Confirm backend is running on `http://localhost:8000`.
2. Confirm frontend `VITE_API_BASE_URL` points to backend.
3. Login again (token may be expired or missing).
4. Check backend terminal for:
   - auth errors (`No token provided`, `Error in authenticating user`)
   - provider timeout (`Image provider timed out`)
   - upload timeout (`Image upload timed out`)
5. Try a shorter prompt and smaller resolution.

### Redis warnings

If Redis is not configured, warnings may appear once at startup.  
The app is designed to continue without Redis cache.

## Security

- Never commit real `.env` files.
- Rotate any secret keys that were accidentally exposed.
- Use separate keys for development and production.
