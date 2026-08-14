# MovieTime 🎬

A full-stack movie ticket booking platform built with the MERN stack. Browse now-playing movies, view showtimes, book seats, and manage bookings — with a dedicated admin dashboard for adding shows and tracking sales.

## Features

For users
- Sign up and log in via [Clerk](https://clerk.com), with multiple sign-up options: email, social login, and phone number
- Multi-session support — create and switch between multiple profiles on the site without signing out
- Browse now-playing and upcoming movies (powered by [TMDB](https://www.themoviedb.org))
- View movie details, cast, and showtimes
- Interactive seat selection when booking tickets
- Secure online payments via [Stripe](https://stripe.com)
- Automatic seat release: if a payment fails or is cancelled, the selected seats stay reserved for 10 minutes so the user can retry payment — if not completed in time, the seats are released back for others to book
- Maintain a list of favorite movies

Automated emails (via [Inngest](https://www.inngest.com) + [Brevo](https://www.brevo.com))
- Notification email to all users whenever a new movie is added
- Booking confirmation email after a successful ticket purchase
- Reminder email sent a few hours before the movie's showtime

For admins
- Dedicated admin dashboard
- Add new movies and set showtimes (pulled from TMDB)
- View and manage all bookings across the platform

Under the hood
- Background job processing and scheduling via [Inngest](https://www.inngest.com), including scheduled reminder emails and delayed seat-release logic

## Tech Stack

Frontend
- React (Vite)
- Tailwind CSS
- React Router
- Axios

Backend
- Node.js / Express
- MongoDB (Mongoose)
- Clerk (authentication & admin roles)
- Stripe (payments + webhooks)
- Inngest (event-driven background jobs)
- TMDB API (movie data)
- Brevo (transactional email)

## Project Structure

```
├── client/          # React frontend
│   ├── src/
│   └── .env
└── server/          # Express backend
    ├── controllers/
    ├── routes/
    ├── models/
    ├── middleware/
    ├── configs/
    └── .env
```

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/en/download/) installed
- Accounts/API keys for:
  - [MongoDB Atlas](https://www.mongodb.com/cloud/atlas/register)
  - [TMDB](https://www.themoviedb.org) (use the **API Read Access Token**, not the short API key)
  - [Clerk](https://clerk.com)
  - [Stripe](https://dashboard.stripe.com/register)
  - [Inngest](https://www.inngest.com)
  - [Brevo](https://www.brevo.com)

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
```

### 2. Set up the server

```bash
cd server
npm install
```

Create a `.env` file inside `server/` with the following:

```env
MONGODB_URI=your_mongodb_connection_uri
TMDB_API_KEY=your_tmdb_read_access_token
CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
CLERK_SECRET_KEY=your_clerk_secret_key
STRIPE_SECRET_KEY=your_stripe_secret_key
STRIPE_WEBHOOK_SECRET=your_stripe_webhook_secret
BREVO_API_KEY=your_brevo_api_key
INNGEST_EVENT_KEY=your_inngest_event_key
INNGEST_SIGNING_KEY=your_inngest_signing_key
```

Run the server:

```bash
npm run server
```

The server should start on `http://localhost:3000`.

### 3. Set up the client

Open a new terminal:

```bash
cd client
npm install
```

Create a `.env` file inside `client/` with the following:

```env
VITE_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
VITE_BASE_URL=http://localhost:3000
VITE_CURRENCY=$
VITE_TMDB_IMAGE_BASE_URL=https://image.tmdb.org/t/p/original
```

Run the client:

```bash
npm run dev
```

Visit `http://localhost:5174` (or whichever port Vite prints) in your browser.

### 4. Set up admin access

To access the admin dashboard:

1. Go to your [Clerk Dashboard](https://dashboard.clerk.com) → Users
2. Select your user account
3. Under Private Metadata, add:
   ```json
   { "role": "admin" }
   ```
4. Refresh your session and visit `/admin` on the site

From the admin dashboard, you can add shows (movies + showtimes), view listings, and track bookings.

### 5. Sync Inngest (for background jobs)

Run the Inngest dev server alongside your app for local event processing:

```bash
npx inngest-cli@latest dev
```

## Deployment

- Deploy the backend and frontend separately (e.g. both on [Vercel](https://vercel.com), or backend on Render/Railway and frontend on Vercel)
- Update `VITE_BASE_URL` in the client to point to your deployed backend URL
- Configure a live Stripe webhook endpoint pointing to `/api/stripe`
- Set up Clerk webhooks for user sync if applicable
- Sync your app on Inngest's dashboard for production event handling

## License

This project was built for learning and educational purposes. It is licensed under the [MIT License](./LICENSE).

MIT License

Copyright (c) 2026 Piyush Mandal

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

---

Note: This project was built for learning and educational purposes. It uses
third-party services (TMDB, Clerk, Stripe, Inngest, Brevo, MongoDB Atlas) —
usage of those services is subject to their own respective terms of service.
