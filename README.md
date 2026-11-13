# ◈ Codeweave

**A collaborative, local-first code editor that runs entirely in your browser.**

Codeweave gives you a VS Code-style editor (powered by Monaco) where everyone in a room edits the same files in real time. It can run JavaScript, TypeScript, Python, SQL, HTML, CSS, Markdown, JSON and SVG right in the page. There is no backend, no account and no build step: it is a single `index.html` file.

---

## ✨ Features

### Real-time collaboration
- **Rooms**: pick any secret word (or hit 🎲 for a random one) and share the invite link. Anyone who opens it joins your room.
- **Live cursors and presence**: see who is online, with coloured cursors and selections.
- **Conflict-free editing**: text is synced with an **RGA text CRDT**, so edits merge correctly regardless of order, and work offline.
- **Peer-to-peer**: the first person in a room becomes the host and others connect to them. [PeerJS](https://peerjs.com/) is used only for signalling, and your code is not stored on a server.
- **Offline-friendly**: edits are saved locally and sync again when you reconnect.

### Run code in the browser
| Language | How it runs |
| --- | --- |
| HTML / CSS | Live preview in a sandboxed iframe, with linked `style.css` and `script.js` inlined automatically |
| JavaScript | Executed in a sandboxed iframe, with `console` output captured |
| TypeScript | Compiled by Monaco's TS worker, then executed |
| Python | [Pyodide](https://pyodide.org/) (WebAssembly), with about a 10 MB download on first run |
| SQL | [sql.js](https://github.com/sql-js/sql.js) (SQLite in WebAssembly), with results shown as tables |
| Markdown | Rendered with [marked](https://marked.js.org/) |
| JSON / SVG | Pretty-printed or rendered |

### Editor experience
- Monaco editor (the engine behind VS Code), with IntelliSense, multi-cursor, find and replace, and format document
- Menu bar, command palette (`F1`), quick open (`Ctrl+P`) and search across files (`Ctrl+Shift+F`)
- **Problems**, **Output** and **Preview** panels, with live-reload for HTML, CSS, Markdown and SVG
- Responsive preview: switch between full, tablet and mobile widths
- Dark, light and high-contrast themes
- Minimap and word wrap toggles

### Files and storage
- **Open Folder** (Chrome / Edge) to read and write straight to a folder on your disk, with optional **Auto Save**
- Open individual files, Save, Save As, and **Download Workspace (.zip)**
- Workspace state is kept in `localStorage` per room

---

## ⌨️ Keyboard shortcuts

| Shortcut | Action |
| --- | --- |
| `Ctrl/Cmd + Enter` | Run current file |
| `Ctrl/Cmd + S` | Save |
| `Ctrl/Cmd + Shift + S` | Save As |
| `Ctrl/Cmd + P` | Go to file |
| `Ctrl/Cmd + Shift + F` | Search in files |
| `Ctrl/Cmd + B` | Toggle explorer |
| `F1` | Command palette |

---

## 🚀 Getting started

No installation required.

```bash
git clone https://github.com/<your-username>/codeweave.git
cd codeweave
```

Open `index.html` in a modern browser, or serve it locally:

```bash
python3 -m http.server 8000
# visit http://localhost:8000
```

Then:

1. Enter your name and a **room code**.
2. Click **Start / Join room**.
3. Click the 🔗 icon in the sidebar to copy the invite link and send it to a friend.

You can also deploy it on **GitHub Pages**: push the repo and enable **Settings → Pages**.

> **Note:** an internet connection is needed on load, because Monaco, PeerJS and the language runtimes are fetched from a CDN. Collaboration also relies on PeerJS's public signalling server.

---

## 🧰 Tech stack

- **Vanilla JavaScript**, with a single HTML file and no framework or bundler
- [Monaco Editor](https://microsoft.github.io/monaco-editor/) 0.52
- [PeerJS](https://peerjs.com/) 1.5 (WebRTC data channels)
- [Pyodide](https://pyodide.org/) 0.26, [sql.js](https://github.com/sql-js/sql.js) 1.10, [marked](https://marked.js.org/) 12 and [JSZip](https://stuk.github.io/jszip/) 3.10, all loaded from jsDelivr
- Custom **RGA CRDT** for conflict-free text sync

### How collaboration works

1. The first peer to claim the room ID becomes the **host**; later peers connect to it (star topology).
2. New peers receive the full document state, then exchange small insert and delete operations.
3. Every character has a unique ID, so operations are idempotent and order-independent, and can safely merge after being offline.

---

## 🔒 Privacy and security

- Code runs inside a **sandboxed iframe** (`sandbox="allow-scripts"`), isolated from the editor page.
- Anyone who knows the room code can join and edit, so use a hard-to-guess code for private sessions.
- Do not run code from people you do not trust.

---

## 🌐 Browser support

Works in modern Chrome, Edge, Firefox and Safari. **Open Folder** and **Save As** use the File System Access API, which currently needs Chrome or Edge. Other browsers fall back to file download.

---

## 📄 License

Add your preferred license here (for example [MIT](https://choosealicense.com/licenses/mit/)).

---

## 👤 Author

Built by **Vikrant Basak**.
