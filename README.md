# STUDY — VIP Academy Management System

A full-stack academy and coaching-centre management web application built with React, TypeScript, Vite, Express, and SQLite. It provides a role-based workspace for managing students, admissions, attendance, fees, assessments, timetables, reports, and family/student portals.

> **Project status:** This repository contains a working development/demo setup with seeded sample data. Review and harden security, backups, deployment configuration, and messaging credentials before using it with real student or payment information.

## Contents

- [Features](#features)
- [Technology stack](#technology-stack)
- [Requirements](#requirements)
- [Quick start](#quick-start)
- [Environment configuration](#environment-configuration)
- [Demo login accounts](#demo-login-accounts)
- [How to use the system](#how-to-use-the-system)
- [Public admission form](#public-admission-form)
- [WhatsApp notifications](#whatsapp-notifications)
- [Build and production notes](#build-and-production-notes)
- [Database and backups](#database-and-backups)
- [Project structure](#project-structure)
- [Scripts](#scripts)
- [Troubleshooting](#troubleshooting)
- [Security notes](#security-notes)
- [License](#license)

## Features

- **Dashboard:** overview of academy activity, attendance, admissions, fee collection, outstanding balances, upcoming assessments, and recent activity.
- **Student management:** student records, contact information, class assignment, guardian details, and student status.
- **Admissions:** review and manage admission applications submitted through the public form.
- **Attendance:** record daily attendance and review attendance information.
- **Fees:** fee plans, invoices, payment records, outstanding balances, and overdue-fee tracking.
- **Assessments:** schedule tests and maintain student marks, grades, and feedback.
- **Timetable:** manage class schedules and view timetable information.
- **Reports:** review academy performance and operational records.
- **Messaging and reminders:** notification queue and optional WhatsApp Business Cloud API integration.
- **Role-based portals:** separate experiences for academy owners/admins, teachers, students, and parents.
- **Parent portal:** view a child's attendance, assessment results, fee information, and send an inquiry to the academy.
- **Student portal:** view timetable, assessment results, and teacher feedback.
- **Sample data:** a new database is seeded with demonstration users and sample academy records.

Some messaging features run in simulated/development mode unless valid WhatsApp Business API credentials are configured.

## Technology stack

- React 19 and TypeScript
- Vite
- Express
- SQLite using `sql.js`
- Cookie-based session handling
- Tailwind CSS and Lucide icons

## Requirements

Install the following on your computer:

1. **Node.js** (a current LTS release is recommended).
2. **npm**, which is included with Node.js.
3. **Git** (optional, if you clone the repository with Git).

Check your installation:

```bash
node --version
npm --version
git --version
```

## Quick start

### 1. Get the project

Clone your GitHub repository:

```bash
git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
cd YOUR-REPOSITORY
```

Replace `YOUR-USERNAME` and `YOUR-REPOSITORY` with your actual GitHub username and repository name. Alternatively, download the repository ZIP from GitHub and extract it.

### 2. Install dependencies

From the project root—the folder containing `package.json`—run:

```bash
npm install
```

### 3. Create your environment file

Copy `.env.example` to `.env`.

On Windows Command Prompt:

```bat
copy .env.example .env
```

On macOS/Linux:

```bash
cp .env.example .env
```

Open `.env` and adjust the values as explained in [Environment configuration](#environment-configuration). Do not commit private credentials or production secrets to GitHub.

### 4. Start the development server

```bash
npm run dev
```

Open the local address shown in the terminal. By default, the application uses:

```text
http://localhost:3000
```

Keep the terminal running while you use the application. Stop the server with `Ctrl+C`.

### 5. Sign in

Use one of the demo accounts listed below. The demo password is `study123`. The owner account is configured to require a password change on first login; change it before continuing.

## Environment configuration

The project reads its server settings from environment variables. Use `.env.example` as the template.

| Variable | Purpose | Example/default |
|---|---|---|
| `APP_URL` | Public base URL used for deployment/configuration | `https://your-domain.com` |
| `PORT` | Port for the Express server | `3000` |
| `NODE_ENV` | Runtime mode | `development` |
| `SESSION_SECRET` | Secret intended for session/security configuration | Replace with a long, random secret for production |
| `DATABASE_PATH` | Example database path setting | `./data/study.sqlite` |
| `WHATSAPP_PHONE_NUMBER_ID` | WhatsApp Business phone-number ID | Optional |
| `WHATSAPP_ACCESS_TOKEN` | WhatsApp Cloud API access token | Optional; keep private |
| `WHATSAPP_BUSINESS_ACCOUNT_ID` | WhatsApp Business account ID | Optional |

**Important implementation note:** in the current code, the SQLite database path is set directly in `server/db.ts` to `data/study.sqlite`; changing `DATABASE_PATH` in `.env` may not change the actual database file unless you update the code. Also, before production use, verify that every security-related environment variable is actually wired into the relevant code.

Generate a strong secret rather than using the example value. Never publish real API tokens, passwords, or private credentials.

## Demo login accounts

The initial database seed defines these accounts:

| Role | Email | Password |
|---|---|---|
| Owner | `owner@studyacademy.com` | `study123` |
| Admin / Reception | `reception@studyacademy.com` | `study123` |
| Teacher | `teacher@studyacademy.com` | `study123` |
| Additional teacher | `physics@studyacademy.com` | `study123` |
| Student | `student@studyacademy.com` | `study123` |
| Parent | `parent@studyacademy.com` | `study123` |

These are **demo credentials only**. Do not use them on a public production deployment. Change or remove all seeded accounts before real-world use. The owner account is seeded with a first-login password-change requirement.

## How to use the system

The exact navigation options depend on the role used to sign in.

### Owner / Admin

1. Sign in with the owner or admin demo account.
2. Start at the dashboard to review academy activity and summary figures.
3. Open **Students** to add or maintain student profiles and class/guardian details.
4. Open **Admissions** to review applications and update their status.
5. Open **Attendance** to select the relevant class/session and record attendance.
6. Open **Fees** to review fee information, create/manage invoices, and record payments where available.
7. Open **Assessments** to schedule tests and enter marks or feedback.
8. Open **Timetable** to manage or review class schedules.
9. Open **Reports** to review available operational and academic summaries.
10. Use the relevant communication or inquiry areas to review messages and notification activity.

For real student records, verify the information before saving and establish a regular backup process.

### Teacher

1. Sign in with a teacher account.
2. Open the available teacher workspace.
3. Review the relevant class and timetable information.
4. Use the attendance area to record attendance for assigned sessions, where permitted.
5. Enter or review assessment marks and feedback if those controls are available to the account.

Teacher permissions are restricted by the role checks implemented in the API.

### Student

1. Sign in with the student account.
2. Open the student portal.
3. Review the weekly timetable.
4. Check test results, grades, and teacher feedback when available.

### Parent / Guardian

1. Sign in with the parent account.
2. Open the parent portal.
3. Review the linked child's attendance, assessments, and fee invoices/payment information.
4. Use the inquiry form to send a question to the academy.

The parent portal only displays records linked to the signed-in parent account.

## Public admission form

The application includes a public admission page. The app checks paths beginning with `/apply/` or `/admissions/` and loads the public admission interface.

The route expects an academy code/slug in the URL. Use the route format implemented by the app and verify the academy code/slug from the seeded academy record or database. After submitting a test application, sign in as an owner/admin and review it in Admissions.

## WhatsApp notifications

WhatsApp integration is optional. Without valid WhatsApp Business Cloud API credentials, the application is intended to operate in a simulated/development mode with manual-send or web-fallback behavior.

To configure live delivery:

1. Create and configure a WhatsApp Business Platform app through Meta.
2. Obtain the correct phone number ID, access token, and business account ID.
3. Set `WHATSAPP_PHONE_NUMBER_ID`, `WHATSAPP_ACCESS_TOKEN`, and `WHATSAPP_BUSINESS_ACCOUNT_ID` in the server environment.
4. Restart the server and test with an authorized recipient.
5. Check the server logs and Meta configuration if delivery fails.

Do not commit API tokens to the repository. Production messaging may require approved templates, recipient consent, and compliance with Meta's policies. Verify those requirements before sending real messages.

## Build and production notes

Create a frontend production build:

```bash
npm run build
```

Run the server in production mode after building:

```bash
NODE_ENV=production npm start
```

On Windows PowerShell, set the variable for the current terminal session first:

```powershell
$env:NODE_ENV = "production"
npm start
```

The Express server serves the `dist` directory when `NODE_ENV=production`. The host must support a persistent Node.js process and writable storage for the SQLite database. A static-only hosting service cannot run this Express backend by itself.

Before deploying publicly:

- Configure HTTPS and secure deployment secrets.
- Use a persistent disk/volume for the database.
- Replace demo credentials and sample data.
- Review authentication, session persistence, cookie settings, CORS, rate limiting, and authorization.
- Test the production build and all role-based flows.
- Set up backups, monitoring, and a recovery procedure.
- Confirm that your host supports the Node.js server process and database writes.

**Deployment caveat:** sessions are stored in memory in the current implementation. Users may be logged out after a server restart, and sessions are not shared across multiple server instances. Configure a durable session store before scaling the application.

## Database and backups

The current database file is `data/study.sqlite`. If it does not exist, the application initializes a database and seeds demonstration records.

To back up the database:

1. Stop the application so that the file is not being written.
2. Copy `data/study.sqlite` to a secure backup location.
3. Restart the application.
4. Test your backup restoration process periodically.

Do not commit a database containing real student, parent, contact, or financial information. Before publishing the repository, ensure `data/study.sqlite` contains only safe demo data or remove it if you want the application to initialize a fresh database.

## Project structure

```text
.
├── index.html
├── package.json
├── package-lock.json / bun.lock
├── vite.config.ts
├── tsconfig.json
├── .env.example
├── server.ts              # Express server and Vite integration
├── server/
│   ├── api.ts              # API routes
│   ├── auth.ts             # Authentication and role checks
│   ├── db.ts               # SQLite setup, schema, and seed data
│   └── worker.ts           # Notification/reminder worker
├── src/
│   ├── App.tsx
│   ├── main.tsx
│   ├── index.css
│   ├── components/         # Shared UI components
│   ├── context/            # Authentication and theme contexts
│   ├── pages/              # Dashboard and role-specific pages
│   └── types/
├── data/
│   └── study.sqlite        # Local SQLite database (may contain demo data)
└── tests/
    └── test_study.ts
```

## Scripts

| Command | What it does |
|---|---|
| `npm install` | Installs project dependencies |
| `npm run dev` | Starts the development server |
| `npm start` | Starts the server; set production environment variables for deployment |
| `npm run build` | Builds the frontend into `dist/` |
| `npm run preview` | Starts Vite's preview server for the built frontend (not a replacement for the Express API server) |
| `npm run lint` | Runs the TypeScript compiler without emitting files |
| `npm run clean` | Removes the `dist` directory on Unix-like environments |

Note: the `clean` script uses `rm -rf`, which does not run as-is in standard Windows Command Prompt.

## Troubleshooting

### `npm` or `node` is not recognized

Install Node.js, close and reopen your terminal, then run `node --version` and `npm --version`.

### Dependencies fail to install

- Confirm that you are in the folder containing `package.json`.
- Use a supported Node.js LTS release.
- Retry `npm install` with a stable internet connection.
- Read the first error shown in the terminal; it usually identifies the missing package or incompatible runtime.

### Port 3000 is already in use

Set another port before starting the app.

macOS/Linux:

```bash
PORT=3001 npm run dev
```

Windows PowerShell:

```powershell
$env:PORT = "3001"
npm run dev
```

Then open `http://localhost:3001`.

### Login does not work

- Confirm that you are using one of the demo emails exactly as listed.
- The demo password is `study123`.
- Check the server terminal for API/database errors.
- If you are using an old database, it may not contain the expected seeded accounts. Back up any data you need before changing or recreating the database.

### The app starts but data is missing

The application reads from `data/study.sqlite`. Check that the server has write permission to the `data` directory. Back up the file before replacing or removing it. The seed data is inserted when the database is first initialized and the academies table is empty; an existing database is not automatically reset.

### WhatsApp messages are not delivered

Confirm that the WhatsApp credentials are valid, the recipient is eligible to receive messages, required templates are approved, and the server logs show no API errors. Without live credentials, use the development/manual fallback.

### Production shows a blank page or API errors

Run `npm run build`, confirm `dist/index.html` exists, set `NODE_ENV=production`, and start the Express server with `npm start`. Deploy the Node server and frontend together; do not deploy only the static `dist` folder if you need the API.

## Security notes

This project is provided as a starting point/demo, not as a certified production student-information system. Before using real personal or financial data, conduct a security review. In particular:

- Replace demo passwords and use strong, unique credentials.
- Keep `.env` and all tokens out of version control.
- Do not publish real student/parent data or database files.
- Review session storage, session-secret usage, cookie flags, HTTPS, CORS, and rate limiting.
- Confirm authorization for every API route and validate incoming data.
- Use database backups and test restoring them.
- Follow applicable privacy, education-record, and data-protection laws.

## License

No license file was included with this project. Unless you add a license, others should not assume the repository grants permission to reuse, modify, or redistribute the code. Add a `LICENSE` file if you want to specify reuse terms.
