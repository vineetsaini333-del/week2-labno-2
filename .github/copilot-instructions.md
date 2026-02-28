# Copilot Instructions for `week2-labno-2`

This repository powers an **educational website for sharing homework
assignments and coding exercises with students**.  Students navigate the
portal to browse, view and download assignments; each assignment lives in its
own folder and the entire site is configured via a simple JSON file.

Because the site is static, there is no build step.  All logic runs in the
browser, and the `assignments/` subdirectories contain the actual exercise
material.  These instructions collect the essential knowledge an AI agent
needs to be immediately productive in the codebase.

---

## 📁 High‑level structure

* `index.html` – the course homepage.  It loads `assets/js/script.js` which
  fetches `config.json` and populates the list of assignments.
* `assets/` – static assets (CSS, images, JavaScript, and the assignment page
  template).  JavaScript is written in ES‑6 classes; the two entry points are
  `AssignmentPortal` (for the home page) and `AssignmentPage` (for
  `assets/pages/assignment.html`).
* `config.json` – single source of truth for the course metadata.  The
  `assignments` array contains objects with keys like `id`, `title`,
  `description`, `path`, `dueDate`, and optional `attachments`.
* `assignments/` – each assignment has its own folder with a `README.md` (the
  markdown rendered on the detail page) and any starter files (Python code,
  datasets, etc.).  The `path` value in `config.json` must match a
  subdirectory here.
* `assets/pages/assignment.html` – generic detail page; it uses [marked.js]
  (included via CDN) to convert the assignment `README.md` to HTML.

There is no dependency management or tooling – open `index.html` in a browser
or serve the folder with a simple HTTP server (e.g. `python -m http.server`)
so that `fetch` works correctly.

---

## 🔧 Developer workflows

1. **View changes locally** – because of `fetch`, do **not** open files over
   the `file://` protocol.  Use a lightweight server such as:
   ```sh
   cd /workspaces/week2-labno-2 && python3 -m http.server 8000
   # then visit http://localhost:8000 in a browser
   ```

2. **Adding or editing an assignment**
   * Update `config.json` – add or modify the object in `assignments`.  The
     `id` becomes the query parameter used by `assignment.html`; it must be
     unique.
   * Ensure the `path` points to a subdirectory under `assignments/`.  That
     folder must contain a `README.md` and any downloadable files referenced by
     `attachments` (each attachment entry is `{name,file,type}`; `type` affects
     the emoji icon shown).
   * `dueDate` is an ISO string; the front‑end treats missing dates as
     "active forever".  The portal automatically calculates status (`active`/
     `overdue`) and styles rows accordingly.  Sorting is done in
     `renderAllAssignments`.

3. **Styling and visual changes** – `assets/css/styles.css` uses custom
   properties and simple utility classes; the layout is mobile‑responsive but
   very light.  New components should follow the naming conventions already in
   use (`.assignment-row`, `.next-due-card`, etc.).

4. **JavaScript conventions**
   * Classes are used to encapsulate page logic and are instantiated on
     `DOMContentLoaded`.
   * `script.js` and `assignment.js` both fetch `config.json` (relative paths
     differ; note `../..` on the assignment page).
   * Template strings build chunks of HTML; keep them readable and escape
     interpolated values when modifying them.
   * Error handling is minimal and logs to the console – new error messages
     should call `showError()` on the appropriate page.

---

## 🚧 Project‑specific notes

* There is **no build step or package manager**.  Do not introduce dependencies
  that require `npm`, `pip`, etc.; if you need a library, vendor it or use a
  CDN. Marked.js is already loaded via CDN for Markdown rendering.
* A markdown template for new assignments lives at `templates/assignment-template.md`.  New agents can copy it when creating a fresh `README.md`.
* `config.json` is the only place assignments are enumerated.  Look there to
  understand what the JavaScript expects when it iterates over the list.
* Local assets must be referenced with relative paths.  The assignment page is
  two levels deep from the root, so paths often begin with `../../`.
* Tests and Python scripts in `assignments/` are educational, not executed by
  the site – the portal simply offers download links.  You can ignore their
  contents unless asked to modify an assignment.

---

## ✅ When writing or modifying code for this repo, Copilot should

1. Prefer editing the existing classes rather than creating new global
   functions.
2. Update `config.json` when new assignments or course changes are required.
3. Respect the file‑path conventions (`assignments/<id>`, relative URLs).
4. Keep the UI lightweight; avoid introducing frameworks.  Small vanilla JS
   utilities or CSS styles are fine.
5. Add inline comments where business logic lives (due‑date sorting,
   status calculation) so future agents understand why numbers like `1000 *
   60 * 60 * 24` appear.
6. Assume there is no test harness; any manual changes should be verified by
   running a local HTTP server and opening the site.

---

*Feel free to ask if any part of the portal or workflow is unclear.*
