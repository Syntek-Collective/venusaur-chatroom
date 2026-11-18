# Venusaur Chatroom

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

A real-time chat application built by a three-person team in a 10-day sprint, now maintained as an open-source project by [Syntek Collective](https://github.com/Syntek-Collective).

Users create an account, sign in, and chat live in one of four rooms. Three are free; the Mewtwo room is a premium room unlocked with a one-time Stripe Checkout payment.

Contributions are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) to get started.

## Tech Stack

| Layer                  | Technology                                            |
| ---------------------- | ----------------------------------------------------- |
| Language               | TypeScript                                            |
| Frontend               | React + Vite                                          |
| Backend                | Express                                               |
| Real-time              | Socket.io                                             |
| Users and sessions     | MongoDB (PostgreSQL optional)                         |
| Rooms and chat history | MongoDB                                               |
| Auth                   | bcrypt + database-backed sessions (HTTP-only cookies) |
| Payments               | Stripe Checkout + webhooks (premium room)             |

See `DECISIONS.md` for why.

## Team

Venusaur Chatroom was originally built by:

| Name    | Owns                                                     |
| ------- | -------------------------------------------------------- |
| Eddie   | MongoDB and PostgreSQL authentication                    |
| Kanami  | MongoDB (rooms and chat history), Express, and Socket.io |
| Tanisha | Frontend                                                 |

## Project Structure

```text
venusaur-chatroom/
├── client/         React + Vite frontend
├── server/         Express backend
└── package.json    Root scripts for running both apps together
```

`client/` and `server/` are separate apps, each with its own `package.json` and dependencies.

## Getting Started

### Prerequisites

- Node.js 22.13+ or 24+
- npm

### Install

Dependencies are installed separately in the root, `client/`, and `server/`:

```bash
git clone https://github.com/Syntek-Collective/venusaur-chatroom.git
cd venusaur-chatroom
npm install
npm install --prefix client
npm install --prefix server
```

On npm 12 or newer, you may see `install-scripts` warnings about `esbuild` and `fsevents` being blocked. They are harmless, and the app still runs.

### Environment Variables

Copy `server/.env.example` to `server/.env` and configure the required variables.

```bash
cd server
cp .env.example .env
```

Configure the following variables:

- `AUTH_DB=mongodb` — Selects MongoDB for authentication (recommended).
- `MONGODB_URI` — MongoDB connection string for rooms, chat history, users and sessions.
- `POSTGRES_URI` — PostgreSQL connection string (only required when `AUTH_DB=postgres`).
- `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET`, `STRIPE_PRICE_MEWTWO`, `CLIENT_URL` — Only needed for the premium Mewtwo room. See [Stripe Setup](#stripe-setup-optional) below.

MongoDB is the primary database for the application. PostgreSQL authentication is also supported but requires additional setup.

**Production:** Set `NODE_ENV=production` and use HTTPS to enable secure session cookies.

Do not commit `.env` files containing credentials.

### MongoDB Setup

MongoDB stores rooms and chat history regardless of the authentication database.

1. Configure `MONGODB_URI` in `server/.env`.
2. Create the four chat rooms from the repository root:

```bash
npm run seed --prefix server
```

This only needs to run once per database. It is safe to run again: rooms that already exist are left alone.

When using MongoDB for authentication, set `AUTH_DB=mongodb` in `server/.env`. Mongoose automatically creates the `users` and `sessions` collections when documents are inserted. No additional authentication database setup is required.

### PostgreSQL Authentication Setup (Optional)

PostgreSQL is supported as an alternative authentication database but is not required for the standard application setup.

To use PostgreSQL for authentication:

1. Create a Supabase PostgreSQL project.
2. Open the SQL Editor.
3. Open `server/sql/001_create_users.sql`.
4. Replace the placeholder password with a secure password.
5. Execute the SQL to create the users table and restricted application user.
6. Execute `server/sql/002_create_sessions.sql` to create the sessions table.
7. Configure `POSTGRES_URI` in `server/.env` using the connection credentials for `chatroom_app_user`.
8. Set `AUTH_DB=postgres` in `server/.env`.

The application database user has `SELECT` and `INSERT` permissions on the `users` table, and `SELECT`, `INSERT`, and `DELETE` permissions on the `sessions` table.

MongoDB is still required for rooms and chat history when using PostgreSQL authentication.

Keep database credentials private and never commit your `.env` file.

### Stripe Setup (Optional)

The free rooms work without Stripe. To try the premium Mewtwo room locally, use Stripe **test mode** only:

1. In the Stripe Dashboard (test mode), create a product with a one-time price and copy its Price ID into `STRIPE_PRICE_MEWTWO`.
2. Copy your test secret key (`sk_test_...`) into `STRIPE_SECRET_KEY`.
3. Set `CLIENT_URL=http://localhost:5173` so Checkout returns to the Vite dev server.
4. Forward webhooks to your local server with the [Stripe CLI](https://docs.stripe.com/stripe-cli) and copy the `whsec_...` value it prints into `STRIPE_WEBHOOK_SECRET`:

```bash
stripe listen --forward-to localhost:3000/api/stripe/webhook
```

Pay with Stripe's test card `4242 4242 4242 4242`, any future expiry date, and any CVC.

### Run

From the repository root:

```bash
npm run dev
```

This starts both apps in one terminal:

| App    | URL                                                |
| ------ | -------------------------------------------------- |
| Client | http://localhost:5173                              |
| Server | http://localhost:3000/health returns `{"ok":true}` |

To run one app on its own, use `npm run dev` inside `client/` or `server/`.

### Before Opening a Pull Request

`npm run dev` does not type-check your code. Run these in each folder you changed:

| Folder    | Commands                           |
| --------- | ---------------------------------- |
| `client/` | `npm run build` and `npm run lint` |
| `server/` | `npm run build`                    |

Then, from the repository root, check formatting:

```bash
npm run format:check
```

If it reports files, run `npm run format` to fix them and commit the result.

## Documentation

| Document                      | Purpose                                                   |
| ----------------------------- | --------------------------------------------------------- |
| `README.md`                   | Project introduction, setup, and running the app          |
| `PROJECT_BRIEF.md`            | Goals, MVP scope, timeline, and stretch goals             |
| `DECISIONS.md`                | Technical decisions, their reasoning, and open questions  |
| `CONTRIBUTING.md`             | Git workflow, branches, commits, and pull requests        |
| `CODE_OF_CONDUCT.md`          | How we expect everyone in the project to behave           |
| `SECURITY.md`                 | How to report a security vulnerability privately          |
| `AGENTS.md`                   | How AI coding agents should behave in this repository     |
| `server/AUTH_API_CONTRACT.md` | Authentication endpoints, responses, and session handling |

## License

Released under the [MIT License](LICENSE).

Pokémon and Pokémon character names are trademarks of Nintendo, Creatures Inc., and GAME FREAK inc. This is an unofficial fan project built for learning, and it is not affiliated with or endorsed by them. The premium room is a demo of Stripe Checkout and is meant to run in Stripe test mode.
