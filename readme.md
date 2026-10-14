# TaskCast (Event Planner) - Team Project (Lehigh University, Spring 2026)

TaskCast is a full-stack event planner that enriches each event with live data, such as weather, holidays, locations, sports scores, and movie suggestions, so users can plan in one place instead of checking several apps.

Built by a team of 3 for CSE 264 (Web Systems Programming).

## Team Members

- **Truc Linh Ho** – Database & API Integration (tth227@lehigh.edu)
- **Denny Li** – Authorization & Cards
- **Neirah Ho** – Frontend & UI

## Functionality

- **Accounts:** register, log in, and stay signed in, with **free** and **premium** user tiers
- **Events:** create, view, edit, and delete events with a title, date, time, location, category, and notes
- **Insight cards:** live data from external APIs is converted into one standard card format and attached to events
  - Free users: weather, holidays, and venue/location cards
  - Premium users: also unlock sports scores and movie suggestions
- **Export:** download events as an `.ics` calendar file or copy event details to the clipboard

## Tech Stack

React, TypeScript, Vite, Tailwind CSS, Node.js, Express, PostgreSQL

**External APIs:** Open-Meteo, weather.gov, OpenStreetMap (Nominatim), Nager.Date, ESPN, IMDb / JustWatch

## Project Structure

- `client/` – React + TypeScript frontend
- `server/` – Node.js / Express REST API and PostgreSQL data layer

## Instructions

### Server

1. Create `server/.env`:

   ```
   POSTGRES_HOST=your-database-host
   POSTGRES_PORT=5432
   POSTGRES_DBNAME=your-database-name
   POSTGRES_USERNAME=your-username
   POSTGRES_PASSWORD=your-password
   PORT=3000
   USER_TABLE=your-user-table
   EVENT_TABLE=your-event-table
   CONFIG_TABLE=your-config-table
   ```

2. Install and run:

   ```bash
   cd server
   npm install
   npm start
   ```

### Client

1. Create `client/.env`:

   ```
   VITE_API_URL=http://localhost:3000
   ```

2. Install and run:

   ```bash
   cd client
   npm install
   npm run dev
   ```

No third-party API keys are needed; all external integrations use public APIs.

## API Endpoints

| Method | Endpoint | Purpose |
|--------|----------|---------|
| POST | `/users` | Register a new user |
| POST | `/users/login` | Log in |
| PUT | `/users` | Change password |
| GET | `/events?userId=...` | Get a user's events |
| GET | `/events/:eventId` | Get one event |
| POST | `/events` | Create an event |
| PUT | `/events/:eventId` | Update an event |
| DELETE | `/events/:eventId` | Delete an event |
| GET | `/config?userId=...` | Load user settings (tier, integrations) |
| PUT | `/config?userId=...` | Save user settings |

## Database Design

- The project adapts **3 existing PostgreSQL tables** into separate stores for **users**, **events**, and **per-user settings**.
- Extra event details are stored as **JSON**, so new fields and card types can be added without changing the database schema.
- Each user's settings (tier and enabled integrations) are stored in **one JSON row**, so upgrading to premium doesn't modify the user record.
- Passwords are hashed with **scrypt** and per-user salts, and all queries are **parameterized** to prevent SQL injection.

