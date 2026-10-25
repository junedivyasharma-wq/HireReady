# HireReady

HireReady is an AI-powered interview preparation platform. Users can create an
account, upload a resume, provide a self-description and job description, and
receive a tailored interview report with a match score, technical and
behavioral questions, skill gaps, and a day-by-day preparation plan. The
application can also generate an ATS-friendly resume PDF from an interview
report.

## Features

- User registration, login, logout, and cookie-based session persistence
- Resume upload in PDF format
- AI-generated interview reports powered by Google Gemini
- Candidate/job match score
- Technical and behavioral interview questions with answer guidance
- Skill-gap analysis with severity levels
- Day-wise interview preparation plan
- Interview report history
- AI-generated resume PDF export

## Tech stack

### Frontend

- React 19
- React Router
- Vite
- Sass
- Axios

### Backend

- Node.js
- Express
- MongoDB with Mongoose
- JSON Web Tokens and cookies
- Multer for file uploads
- `pdf-parse` for reading uploaded resumes
- Google GenAI SDK for report and resume generation
- Puppeteer for PDF rendering

## Project structure

```text
HireReady/
├── Backend/
│   ├── server.js
│   ├── package.json
│   └── src/
│       ├── config/
│       ├── controllers/
│       ├── middlewares/
│       ├── models/
│       ├── routes/
│       └── services/
├── Frontend/
│   ├── package.json
│   └── src/
│       ├── features/auth/
│       └── features/interview/
└── README.md
```

## Prerequisites

- Node.js 18 or later
- npm
- A running MongoDB instance or MongoDB Atlas database
- A Google Gemini API key with access to the configured GenAI model

## Configuration

Create a `.env` file in `Backend/`:

```env
MONGO_URI=mongodb://127.0.0.1:27017/hireready
JWT_SECRET=replace-with-a-long-random-secret
GOOGLE_GENAI_API_KEY=your-google-gemini-api-key
```

Do not commit `.env` files or expose these values in the frontend.

## Installation

Install dependencies for both applications:

```bash
cd Backend
npm install

cd ../Frontend
npm install
```

## Running locally

Start the backend from `Backend/`:

```bash
npm run dev
```

The API runs on `http://localhost:3000`.

In a second terminal, start the frontend from `Frontend/`:

```bash
npm run dev
```

The Vite development server runs on `http://localhost:5173`. The frontend is
configured to call the backend at `http://localhost:3000` and to send
credentials with requests.

## Available scripts

### Backend

| Command | Description |
| --- | --- |
| `npm run dev` | Start the API with Nodemon |
| `npm test` | Placeholder test command |

### Frontend

| Command | Description |
| --- | --- |
| `npm run dev` | Start the Vite development server |
| `npm run build` | Create a production build |
| `npm run preview` | Preview the production build |
| `npm run lint` | Run ESLint |

## API overview

All API routes are served from `http://localhost:3000`.

### Authentication

| Method | Route | Description | Auth |
| --- | --- | --- | --- |
| `POST` | `/api/auth/register` | Register a user | Public |
| `POST` | `/api/auth/login` | Log in a user | Public |
| `GET` | `/api/auth/logout` | Log out and clear the session cookie | Public |
| `GET` | `/api/auth/get-me` | Get the current user | Required |

### Interviews and reports

| Method | Route | Description | Auth |
| --- | --- | --- | --- |
| `POST` | `/api/interview/` | Generate a report from a resume PDF, self-description, and job description | Required |
| `GET` | `/api/interview/` | List the current user's reports | Required |
| `GET` | `/api/interview/report/:interviewId` | Retrieve one report | Required |
| `POST` | `/api/interview/resume/pdf/:interviewReportId` | Generate and download a resume PDF | Required |

The report-generation request uses `multipart/form-data` with the following
fields:

- `resume`: the uploaded PDF file
- `selfDescription`: the candidate's self-description
- `jobDescription`: the target role's job description

## Notes

- The backend currently allows requests from `http://localhost:5173` through
  CORS.
- Resume and PDF generation use external AI and browser-rendering services, so
  generation time depends on the input and available resources.
- Puppeteer may download a compatible Chromium binary during dependency
  installation.
