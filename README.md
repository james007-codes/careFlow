# CareFlow — Smart Hospital Queue & Resource Management

CareFlow is a healthcare operations project for hospital queue and resource management. It includes a React/Vite frontend, a Node.js/Express API with MongoDB, and a FastAPI-based AI service that supports AI chat and medical-document workflows.

> **Important:** Use fictional/sample patient data only. CareFlow is a project prototype, not a medical device, and does not replace professional medical judgment.

## Project structure

```text
careFlow/
├── frontend/       # React + Vite + Tailwind CSS
├── backend/        # Node.js + Express + MongoDB
└── ai-service/     # Python + FastAPI + LangChain/LangGraph + retrieval
```

## Prerequisites

Install the following before starting:

- Git
- Node.js and npm
- Python 3.11 or newer
- uv (Python package/environment manager)
- MongoDB (local instance or MongoDB Atlas connection string)
- API keys required by the AI service (configure them in its environment file)

## 1. Clone the repository

Open a terminal and run:

```bash
git clone https://github.com/james007-codes/careFlow.git
cd careFlow
```

## 2. Configure and start MongoDB

Start your local MongoDB service, or create a MongoDB Atlas database and copy its connection string. Keep the connection string private; do not commit credentials to GitHub.

## 3. Configure and start the backend API

Open a terminal in the repository root:

```bash
cd backend
npm install
```

Create a file named `.env` inside the `backend/` directory:

```env
PORT=5000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=replace_with_a_long_random_secret
AI_SERVICE_URL=http://localhost:8000
NODE_ENV=development
```

Replace the example values with your own configuration. Then start the API:

```bash
npm run dev
```

The API should start on `http://localhost:5000`. Keep this terminal open. The backend needs a reachable MongoDB instance.

For a non-development start, use `npm start` from the `backend/` directory.

## 4. Configure and start the AI service

Open a **second terminal** from the repository root:

```bash
cd ai-service
uv sync
```

Create an `.env` file in `ai-service/` and add the API credentials required by the model/embedding configuration used in the service. Never commit real API keys. The service uses FastAPI and its dependencies are declared in `pyproject.toml`.

Start the development server:

```bash
uv run uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

Check `http://localhost:8000/` to confirm the service is running. Interactive API documentation is normally available at `http://localhost:8000/docs`.

If the retrieval service requires a local knowledge index, follow the ingestion setup in `ai-service/app/retrieval/` before testing retrieval-based questions.

## 5. Install and start the frontend

Open a **third terminal** from the repository root:

```bash
cd frontend
npm install
npm run dev
```

Vite prints the local URL in the terminal; it is usually `http://localhost:5173`. Open that URL in your browser.

## 6. Run and test the application

1. Confirm MongoDB is running and reachable.
2. Confirm the backend is running on port `5000`.
3. Confirm the AI service is running on port `8000`.
4. Confirm the frontend is running on the Vite URL shown in its terminal.
5. Open the frontend and try the available hospital queue/resource-management screens.
6. Test login and AI chat/document workflows only after the backend environment and required model API keys are configured.

Keep all three application terminals running while using the full local setup.

## Common issues

- **MongoDB connection error:** Check that MongoDB is running and that `MONGO_URI` is correct.
- **Authentication/JWT errors:** Ensure `JWT_SECRET` is set in `backend/.env`, then restart the backend.
- **AI requests fail:** Check that the AI service is running, that `AI_SERVICE_URL` points to it, and that required provider API keys are configured.
- **Frontend cannot reach an API:** Check the browser console, backend logs, service URLs, and CORS configuration.
- **Port already in use:** Stop the process using the port or configure a free port and update the corresponding service URL.
- **Python dependency issues:** Run `uv sync` from `ai-service/` using Python 3.11 or newer.

## Production deployments

The frontend and API may also be deployed separately. For local development, use the service URLs and environment variables described above. Do not publish secrets, database credentials, JWT secrets, or private patient information in source control.

## Technology stack

- **Frontend:** React, Vite, Tailwind CSS, Recharts
- **Backend:** Node.js, Express, MongoDB/Mongoose, JWT
- **AI service:** Python, FastAPI, LangChain, LangGraph, Google/Groq integrations, FAISS-based retrieval

## Disclaimer

CareFlow is intended for demonstration and educational use. It provides operational estimates and queue-management assistance; it does not diagnose patients, prescribe treatment, or replace clinicians. Use synthetic data for development and demos.
