# Copilot Instructions for FWF Site

## Project Architecture
- **Monorepo structure**: Contains both frontend (static HTML/CSS/JS) and backend (Node.js/Express, SQLite, and MongoDB).
- **Backend**: Located in `backend/`, uses Express and SQLite for user/wallet/project management. Key file: `backend/server.js`.
- **API Layer**: `api/` directory contains serverless-style handlers (e.g., `join.js`) using MongoDB via Mongoose. These are separate from the Express backend and use models in `models/`.
- **Frontend**: Static site in root directory, with HTML pages and shared assets in `assets/`.

## Data Flow & Integration
- **User registration**: Two flows:
  - `/api/pay/simulate-join` (Express/SQLite) for simulated payments and member creation.
  - `/api/join` (API/MongoDB) for actual member registration, sends email via `lib/mailer.js`.
- **Authentication**: JWT-based, cookies for session, see `backend/server.js`.
- **Wallets & Projects**: Managed in SQLite (`backend/server.js`), linked to users.
- **Email**: Uses nodemailer, see `lib/mailer.js`.

## Developer Workflows
- **Start backend server**: `cd backend && npm start` (serves static site and API on same port).
- **Environment variables**: Required for DBs and email. See `.env` and code for required keys (`MONGODB_URI`, `JWT_SECRET`, `MAIL_FROM`, etc.).
- **Database**: SQLite DB auto-creates in `backend/data/`. MongoDB required for API handlers.
- **No test scripts**: No automated tests detected; manual testing via HTTP requests recommended.

## Conventions & Patterns
- **Member IDs**: Format is `FWF-XXXXXX` (SQLite) or `FWF` + last 6 chars of Mongo `_id` (API).
- **Password handling**: Passwords are hashed with bcrypt. Initial password for members is auto-generated or set via email link.
- **Role-based access**: `member` and `admin` roles enforced in backend routes.
- **API error handling**: Consistent JSON error responses (`{ ok:false, error:... }`).
- **Static assets**: Served from `assets/` and root HTML files.

## Key Files & Directories
- `backend/server.js`: Main Express server, SQLite logic, authentication, admin/member APIs.
- `api/join.js`: Member registration via MongoDB, email notification.
- `lib/db.js`: MongoDB connection logic.
- `lib/mailer.js`: Email sending setup.
- `models/Member.js`: Mongoose schema for members.
- `assets/`: Static assets (CSS, JS, images).

## External Dependencies
- **Express, SQLite, Mongoose, Nodemailer, Bcrypt, JWT**
- **Environment variables**: Must be set for DB and email integration.

## Example Patterns
- To add a new API route, follow the structure in `api/` and use Mongoose models.
- For backend logic, use prepared statements and role checks as in `backend/server.js`.
- For frontend changes, update HTML/CSS in root or `assets/`.

---
_If any section is unclear or missing, please provide feedback to improve these instructions._
