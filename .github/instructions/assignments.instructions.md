# Assignment Contributor Guidelines

This document explains how to add or update assignments in the `week2-labno-2` educational portal repository.

## 🗂 Folder Structure
- `assignments/` — each assignment has its own subdirectory identified by a unique `id`.
- `templates/assignment-template.md` — markdown template to copy when creating a new assignment.
- `config.json` — the only source of truth that lists all assignments and their metadata.

## ➕ Adding a New Assignment
1. **Create a subfolder** under `assignments/` with a unique name (e.g. `new-topic`).
2. **Copy the template**:
   ```sh
   cp templates/assignment-template.md assignments/new-topic/README.md
   ```
3. **Edit the README** to replace placeholders with a title, objective, tasks, and any starter files or attachments as needed.
4. **Update `config.json`**:
   * Add a new object to the `assignments` array with keys:
     - `id`: same as the folder name
     - `title`: human‑readable title
     - `description`: short description used on the homepage
     - `path`: relative path to the subdirectory (e.g. `assignments/new-topic`)
     - `dueDate`: ISO string (optional)
     - `attachments`: array of `{name,file,type}` objects (optional)
5. **Link any static assets** (images, code files) with correct relative paths in the markdown.

## ✏️ Editing an Existing Assignment
- Modify the `README.md` in its folder; keep the structure of headings and emoji icons intact.
- If you rename files or change paths, update `config.json` accordingly.

## 🚫 Common Pitfalls
- Do **not** open `index.html` or assignment pages using `file://`—use a simple HTTP server.
- Ensure `config.json` remains valid JSON; run `jq . config.json` to verify.
- Avoid adding build tooling or runtime dependencies; the site is static.

## ✅ Testing Changes Locally
Start a server from the workspace root:
```sh
cd /workspaces/week2-labno-2 && python3 -m http.server 8000
```
Visit `http://localhost:8000` and navigate through assignments to confirm formatting and links.

---
Thanks for contributing! Please keep the portal lightweight and student‑friendly.