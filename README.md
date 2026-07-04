# SkyVora

An AI-powered weather forecasting web application built on **TanStack Start** (React 19 + Vite), **Lovable Cloud** (Postgres + Auth + Server Functions), and **Lovable AI Gateway** (Gemini).

## Features

### Weather
- Search weather by city name (or auto-detect via geolocation)
- Current conditions: temperature, feels-like, weather description
- Hourly forecast (next 24h, 3h steps)
- 7-day daily forecast
- Air Quality Index (AQI)
- Wind speed & direction, humidity, pressure, visibility
- Sunrise & sunset times
- Dynamic weather-aware backgrounds (clear, cloudy, rainy, stormy, day/night)

### User
- Email/password sign-up and sign-in (JWT via Lovable Cloud)
- Save & remove favorite cities
- Persisted chat history
- Celsius / Fahrenheit toggle (persisted in `localStorage`)
- Dark / Light theme toggle with smooth animation (persisted in `localStorage`)

### AI Assistant
- Floating chatbot at the bottom-right of every page
- Streams responses from Lovable AI Gateway (Gemini)
- Real-time weather context passed with every question:
  - "Should I carry an umbrella today?"
  - "What should I wear?"
  - "Is today safe for travel?"
  - Storm / heatwave warnings, AQI explanations, hydration reminders

## Architecture

```
Frontend (React + TanStack Start)
  └── /api/chat  ─── Streaming AI (Lovable AI Gateway)
  └── Server functions (createServerFn) ─── Lovable Cloud (Postgres, Auth)
       └── OpenWeatherMap API (server-side fetch, keys never in browser)
```

- **Database schema** (`profiles`, `favorite_cities`, `chat_messages`) with Row-Level Security scoped to `auth.uid()`.
- **Auth**: JWT sessions managed by Lovable Cloud; bearer token auto-attached to server function calls.
- **Weather API keys**: `OPENWEATHER_API_KEY` stored as a Lovable secret, read server-side only.
- **AI key**: `LOVABLE_API_KEY` auto-provisioned by Lovable Cloud, server-side only.
- **Fetch API** used throughout — no Axios.

## Local development

```
bun install
bun run dev
```

## Environment variables

Managed via Lovable Cloud secrets — no `.env` file needed. See `.env.example` for reference.

## Deploy

Publish directly from the Lovable editor (Publish button). Frontend and server functions deploy together to a Cloudflare Worker edge runtime.
