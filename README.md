# TaskFlow — Full-stack learning project

A working task manager built with React, Vite, Express, and SQLite.

## Requirements
- Node.js 20.19+ or 22.12+ (an LTS release is recommended)
- npm (included with Node.js)
- A code editor such as VS Code

## 1. Install and run

Open a terminal in this project directory and run:

```bash
npm install
npm run dev
```

Open the frontend at **http://localhost:5173**. The API runs at **http://localhost:4000**.

The first run creates `server/taskflow.db` automatically. Tasks remain saved across restarts.

## 2. Try it
1. Add a task with a title, optional notes, and priority.
2. Mark a task complete with its checkbox.
3. Edit a task with the pencil button.
4. Delete a task with the trash button (confirm when asked).
5. Use search and the All / In progress / Completed filters.
6. Refresh the browser. Tasks should still be there.

## 3. How the project is organized

```text
taskflow-fullstack/
├── server/
│   └── index.js       # Express API + SQLite schema and queries
├── src/
│   ├── App.jsx        # React UI and client-side state
│   ├── main.jsx       # React entry point
│   └── styles.css     # Responsive visual design
├── index.html
├── vite.config.js     # Dev server + /api proxy
├── package.json
└── README.md
```

## 4. API endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/health` | Check server health |
| GET | `/api/tasks` | List tasks |
| POST | `/api/tasks` | Create a task |
| PUT | `/api/tasks/:id` | Edit title, notes, priority |
| PATCH | `/api/tasks/:id/complete` | Mark complete/incomplete |
| DELETE | `/api/tasks/:id` | Delete a task |

Example create request:

```bash
curl -X POST http://localhost:4000/api/tasks \
  -H "Content-Type: application/json" \
  -d '{"title":"Learn REST APIs","details":"Try the POST endpoint","priority":"high"}'
```

## 5. Learn the request flow

When you submit the form:
1. `handleSubmit` in `src/App.jsx` sends JSON with `fetch`.
2. Vite forwards `/api` requests to Express on port 4000.
3. `server/index.js` validates the input.
4. A prepared SQLite statement inserts the row.
5. Express returns the saved task as JSON.
6. React reloads the list and updates the dashboard.

Prepared SQL statements keep user input separate from SQL code.

## 6. Production build (local)

Build the React app:

```bash
npm run build
```

Then serve the built frontend and API from Express:

**macOS/Linux**
```bash
NODE_ENV=production npm start
```

**Windows PowerShell**
```powershell
$env:NODE_ENV="production"
npm start
```

Visit **http://localhost:4000**. This demo is intended for learning and local use. Before exposing it publicly, add authentication, rate limiting, secure deployment configuration, backups, and appropriate security headers.

## Troubleshooting

- **`node` or `npm` not found:** install Node.js LTS, close and reopen the terminal, then run `node -v` and `npm -v`.
- **Port already in use:** stop the other server process, or change `PORT` for the API and configure Vite's proxy to match.
- **SQLite native dependency install error:** use a supported Node.js LTS version and rerun `npm install`.
- **Frontend says backend is unreachable:** make sure `npm run dev` is running and check the terminal for API errors.
