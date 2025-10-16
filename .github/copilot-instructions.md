
# Copilot Instructions for FWF Site

## Architecture Overview
- **Monorepo**: Combines static frontend (HTML/CSS/JS), Node.js/Express backend (SQLite), and serverless-style API handlers (MongoDB/Mongoose).
- **Backend (`backend/`)**: Express server (`server.js`) manages SQLite-based user, wallet, and project logic. Serves static site and API endpoints on the same port.
- **API Layer (`api/`)**: Contains handlers (e.g., `join.js`, `contact.js`) using MongoDB via Mongoose models in `models/`. These are independent of the Express backend.
- **Frontend**: Static HTML pages in root, assets in `assets/` (CSS, JS, images).

## Data Flow & Integration
- **User Registration**:
  - Simulated: `/api/pay/simulate-join` (Express/SQLite)
  - Actual: `/api/join` (API/MongoDB, triggers email via `lib/mailer.js`)
- **Authentication**: JWT-based, session cookies. See `backend/server.js` for logic.
- **Wallets/Projects**: Managed in SQLite, linked to users (see `backend/server.js`).
- **Email**: Sent via nodemailer (`lib/mailer.js`).

## Developer Workflows
- **Start server**: `cd backend && npm start` (serves static and API routes)
- **Environment setup**: `.env` required for DB and email (`MONGODB_URI`, `JWT_SECRET`, `MAIL_FROM`, etc.)
- **Database**: SQLite auto-creates in `backend/data/`; MongoDB required for API handlers
- **Testing**: No automated tests; use manual HTTP requests for validation

## Project-Specific Conventions
- **Member IDs**: `FWF-XXXXXX` (SQLite) or `FWF` + last 6 chars of Mongo `_id` (API)
- **Password Handling**: Bcrypt-hashed; initial password auto-generated or set via email link
- **Roles**: `member` and `admin` enforced in backend routes
- **Error Responses**: Consistent JSON: `{ ok: false, error: ... }`
- **Static Assets**: Served from `assets/` and root HTML files

## Key Files & Directories
- `backend/server.js`: Express server, SQLite logic, authentication, admin/member APIs
- `api/join.js`: Member registration (MongoDB), email notification
- `lib/db.js`: MongoDB connection
- `lib/mailer.js`: Email setup
- `models/Member.js`: Mongoose member schema
- `assets/`: Static assets

## Integration Points & Dependencies
- **Express, SQLite, Mongoose, Nodemailer, Bcrypt, JWT**
- **Environment variables**: Must be set for DB/email integration

## MongoDB Credential Example
To connect to MongoDB Atlas, set your environment variables as follows:

```
MONGODB_URI=mongodb+srv://beenabeena909_db_user:q75H997QJtJrBCzG@<your-cluster-url>/test?retryWrites=true&w=majority
```
Replace `<your-cluster-url>` with your actual cluster hostname from MongoDB Atlas.

**Username:** beenabeena909_db_user
**Password:** q75H997QJtJrBCzG

Store these securely in your `.env` file and never commit credentials to version control.

## Example Patterns
- Add API routes in `api/` using Mongoose models
- Backend logic: use prepared statements and role checks (`backend/server.js`)
- Frontend: update HTML/CSS in root or `assets/`

---
_If any section is unclear or missing, please provide feedback to improve these instructions._
