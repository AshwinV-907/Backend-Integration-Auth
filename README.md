# Task Manager Web Application

A full-stack Task Manager web application built with **Node.js**, **Express.js**, **EJS**, and the native **File System (`fs`)** module.

---

## 📋 Assignment Requirements & Implementation

| Requirement | Implementation Detail | Location |
| :--- | :--- | :--- |
| **Node.js & Express.js** | Server setup with Express middleware (`json`, `urlencoded`, `static`) | [`index.js`](./index.js) |
| **File System (`fs`) Module** | Flat-file persistent storage in `./files/` folder | [`files/`](./files) |
| **Create Task (Title & Description)** | HTML form accepting `title` and `details`/`description` | [`views/index.ejs`](./views/index.ejs) |
| **Save Task as `.txt` File** | Using `fs.writeFile` to save tasks formatted as `<title>.txt` | [`index.js` (POST /create)](./index.js#L35) |
| **Display Tasks as Cards** | Using `fs.readdir` to read all `.txt` files and render them as cards | [`index.js` (GET /)](./index.js#L24) |
| **"Read More" Option** | Link on each card navigating to full task view using `fs.readFile` | [`index.js` (GET /file/:filename)](./index.js#L51) & [`views/show.ejs`](./views/show.ejs) |
| **`req.body` Usage** | Extracts task title and details from form submission | `POST /create`, `POST /edit` |
| **`req.params` Usage** | Extracts target file name from route URL | `GET /file/:filename`, `POST /delete/:filename` |

---

## 🛠️ Key Routes & `fs` Methods

### 1. `GET /` &mdash; Display All Tasks
- **`fs` Method**: `fs.readdir(path, callback)`
- Reads all filenames in the `./files` directory and passes them to `views/index.ejs` to render cards.

### 2. `POST /create` &mdash; Create a New Task
- **`req.body`**: `req.body.title`, `req.body.details` (or `req.body.description`)
- **`fs` Method**: `fs.writeFile(path, content, callback)`
- Sanitizes spaces from the title, creates `<title>.txt`, and writes the description inside.

### 3. `GET /file/:filename` &mdash; Read More / Task Details
- **`req.params`**: `req.params.filename`
- **`fs` Method**: `fs.readFile(path, 'utf-8', callback)`
- Reads the task contents and renders `views/show.ejs` with complete task details and a "Go Back" button.

### 4. `POST /edit` &mdash; Rename Task
- **`fs` Method**: `fs.rename(oldPath, newPath, callback)`
- Allows renaming existing task files.

### 5. `POST /delete/:filename` &mdash; Delete Task
- **`fs` Method**: `fs.unlink(path, callback)`
- Removes the `.txt` file from the `./files` directory.

---

## 🚀 How to Run Locally

1. Install dependencies:
   ```bash
   npm install
   ```

2. Start the server:
   ```bash
   npm start
   ```

3. Open in your browser:
   **http://localhost:9000**

## Netlify deployment (college demo)

This project is adapted for Netlify Functions while preserving the existing Express + EJS architecture.

- `netlify/functions/server.js` wraps the existing Express app with `serverless-http`.
- `netlify.toml` routes application requests to the function and publishes the existing `public/` assets.
- The repository `files/` directory contains demo/seed task files.
- On Netlify, runtime task create/edit/delete operations use `/tmp/taskflow-files`. This storage is writable but temporary, which is acceptable for this college demonstration. A production application should use persistent database/storage services.

### Local run

```bash
npm start
```

Then open `http://localhost:9000`.

### Netlify settings

- Base directory: leave blank (project root)
- Build command: leave blank
- Publish directory: `public`
- Functions directory: `netlify/functions`
