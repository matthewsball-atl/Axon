# Axon v1.33

> *Your knowledge, alive.*

Axon is a local-first personal knowledge management app that runs entirely in your browser — no server, no cloud sync, no account required. It connects directly to a folder of markdown files on your computer and turns them into a searchable, AI-powered knowledge base.

---

## What Axon Does

Axon is built around four ideas:

**Your files stay yours.** Everything lives in a folder on your machine. Axon reads and writes markdown files directly using the browser's File System Access API. There is no database, no proprietary format, no lock-in. If you stop using Axon, your notes are still just markdown files.

**Structure emerges from content.** Axon scans your vault on connect, extracts titles, backlinks, headings, and excerpts, and builds a live index. The more content you add, the more useful the connections become.

**AI is a tool, not the product.** Axon uses an LLM API for specific jobs: answering questions about your vault, converting imported documents to markdown, generating daily education cards, and rewriting reminders into action items. You bring your own API key. Axon never sends your content anywhere except the configured LLM API when you explicitly trigger an AI feature.

**Capture first, organise later.** The `+` button and the reminders file are designed for zero-friction capture. Drop a thought in reminders.md from your phone or another device, and it shows up in your Today list when you open Axon.

---

## Version History

| Version | Notes |
|---|---|
| 1.33 | Stats page redesigned with visual charts: 4-KPI header row, horizontal bar charts for folders and most-referenced notes, SVG donut chart for task breakdown, longest notes list, word count distribution sparkline. |
| 1.32 | Token efficiency fixes: removed full file path dump from every system prompt, capped full content per file at 2000 words (~2700 tokens), capped fallback context tiers. Folder filter label shown in system prompt when active. |
| 1.31 | Removed icon column from import rows — was rendering confusing clipboard emoji next to filename. Type badge already identifies file type. |
| 1.30 | Fixed import checkboxes — FileRow was defined as an unstable component inside ImportView causing React to remount on every render, losing click handlers. Converted to an inline render function. |
| 1.29 | Connect to Brain auto-detects Axon root structure (Content/, Actions/, Chats/, Imports/, reminders.md). Supports both Axon root and direct vault folder selection. Toast confirms what was detected. |
| 1.33 | Stats page redesigned with visual charts: 4-KPI header row, horizontal bar charts for folders and most-referenced notes, SVG donut chart for task breakdown, longest notes list, word count distribution sparkline. |
| 1.32 | Token efficiency fixes: removed full file path dump from every system prompt, capped full content per file at 2000 words (~2700 tokens), capped fallback context tiers. Folder filter label shown in system prompt when active. |
| 1.31 | Removed icon column from import rows — was rendering confusing clipboard emoji next to filename. Type badge already identifies file type. |
| 1.30 | Fixed import checkboxes — FileRow was defined as an unstable component inside ImportView causing React to remount on every render, losing click handlers. Converted to an inline render function. |
| 1.29 | Connect to Brain auto-detects Axon directory structure. Picks Axon root → finds Content/ as vault, Chats/, Imports/, Actions/, reminders.md automatically. Falls back to scanning inside picked folder. Shows detection toast on connect. |
| 1.28 | Fixed stale closure bug causing rescan and import to not update sidebar or index. Fixed readVaultFiles skipping system folders (index/, chats/). Refresh button replaces icon with text, shows "Refreshing…" while active. |
| 1.27 | Capture + button moved to right of search bar. Floating action button (FAB) added to bottom-right on all pages except Settings. |
| 1.26 | Sidebar refresh button (↻) rescans vault and updates index for backdoor-added files. Spinner indicates active refresh. |
| 1.25 | Browse sidebar defaults to collapsed. Expand all / Collapse all toggle added to sidebar. |
| 1.24 | LLM provider selector in Settings (Anthropic active, OpenAI and Google Gemini coming soon). API key field renamed to LLM API Key. About section added to Settings showing current version. |
| 1.23 | Security hardening: DOMPurify for markdown rendering, safeFetch guard, sanitizeName for path traversal prevention, JSON.parse error handling, API key format validation. |
| 1.22 | README.md auto-generated on first-time setup. README included in Axon root scaffold. |
| 1.21 | Rich editor with split/write/preview modes, toolbar, Cmd+S save. Editor mode setting. New file and folder creation from sidebar. Root folder hidden from Browse sidebar. |
| 1.20 | First-time setup wizard — one-click scaffold of full Axon directory structure with auto-connect. |
| 1.19 | Reminders system — configurable reminders file, auto-import to Today on load, Claude rewrites to action titles. |
| 1.18 | Capture & Compose — + button modal with auto-classification (task / note / draft), voice-matched draft generation. |
| 1.17 | Two-layer index system (index/axon-index.json), background indexing on vault connect, incremental index on import, context folder selector in Chat, Save Research to Vault. |
| 1.16 | Browse tab always resets to landing page. |

---

## Directory Structure

When you run first-time setup, Axon creates the following structure:

```
Axon/
├── README.md             ← This file
├── Content/              ← Your markdown vault (connect this as Main Vault)
│   ├── Actions/
│   │   └── items.md      ← Task storage (Today / This Week / Someday / Done)
│   ├── reminders.md      ← Drop thoughts here; they load into Today on launch
│   └── index/
│       └── axon-index.json  ← Auto-generated semantic index (do not edit)
├── Chats/                ← Saved AI conversations (JSON)
└── Imports/              ← Drop files here to convert to markdown
```

You can add any folders and markdown files you like under `Content/`. Axon will discover them automatically on the next vault connect or rescan.

---

## Getting Started

### First-time setup

1. Open `axon.html` in Google Chrome (Chrome is required — Firefox and Safari do not fully support the File System Access API)
2. Click **"Set up Axon for first-time users →"** on the welcome screen
3. Choose a location on your computer for the Axon folder
4. Click **"◈ Create My Axon Folder"** — Axon creates the full directory structure and connects automatically

### Connecting an existing vault

If you already have a folder of markdown files:

1. Open `axon.html` in Chrome
2. Click **"◈ Connect to Brain"**
3. Select your **Axon root folder** (the folder containing Content/, Chats/, Imports/) — Axon will auto-detect all subdirectories
4. Or select any markdown folder directly — Axon will scan inside it for Actions/, Chats/, Imports/, and reminders.md
5. A confirmation toast shows everything that was auto-detected

### API key

Axon requires an API key for AI features (Chat, Import conversion, Daily Education, Reminders rewriting). Without a key, Browse, Search, and Actions work fully with no API calls.

1. Get an Anthropic API key at [console.anthropic.com](https://console.anthropic.com)
2. Open **Settings → AI** in Axon
3. Select **Anthropic Claude** as your LLM Provider
4. Paste your key — it is stored only in your browser's localStorage, never transmitted anywhere except the Anthropic API

---

## LLM Provider Support

| Provider | Status | Notes |
|---|---|---|
| Anthropic Claude | **Active** | Default provider. Uses claude-sonnet-4. |
| OpenAI | Coming soon | GPT-4o support planned. |
| Google Gemini | Coming soon | Gemini 1.5 Pro support planned. |

Axon is designed to be provider-agnostic. The LLM provider selector in Settings → AI shows available and planned providers. Only Anthropic Claude is active in this version.

---

## Features

### Browse
The home view when your vault is connected. Shows open actions, a daily education carousel generated from your topic list, most-referenced notes, and recently added content. Click any note to read it. Click **✎ Edit** to open it in the rich editor.

### Chat
Ask questions about your vault. Axon uses a two-layer retrieval system: a compact index manifest of all notes is always in context, and the most relevant full files are pulled in based on your query. Use the context folder selector in the sidebar to narrow the scope and reduce token cost.

After a productive conversation, click **"◈ Save research to vault"** to synthesise the entire thread into a clean markdown document and save it to any folder.

### Actions
A three-column task board: Today, This Week, Someday. Tasks persist to `items.md` in your Actions folder. Click any task text to edit it inline. Use the **Capture** button (`+` in the topbar) from any screen to add tasks instantly.

### Import
Drop files into your Imports folder and convert them to markdown. Markdown files are copied directly with no API call. PDFs, DOCX, PPTX, and XLSX files are converted via the LLM API. Converted files are added to the vault index immediately.

### Capture & Compose
The `+` button in the topbar opens a quick-capture modal from any screen. Type anything:
- **Short imperative phrases** → classified as tasks, routed to Actions
- **Longer text** → classified as notes, saved to a vault folder you choose
- **"Write…" / "Draft…"** → Claude generates a draft in your voice using your vault as context, which you can save or copy

### Daily Education
A carousel on the Browse landing page. Each card covers one of your configured topics with a 2–3 sentence insight and an **"Explore in Chat →"** button that opens a pre-seeded conversation. Cards are generated once per day (refreshing after 6am). Configure topics in **Settings → Education**.

### Reminders
Point Axon at a `reminders.md` file (Settings → Directories → Reminders File). Add anything to that file — bullets, checkboxes, numbered lists, or freeform lines. When Axon loads or you open the Actions tab, items are rewritten as concise action titles by Claude and added to your Today list. The reminders file is then cleared, ready for your next batch.

### Rich Editor
Click **✎ Edit** on any note, or use the `+` button on any sidebar folder to create a new file. The editor has a markdown toolbar (bold, italic, headings, lists, code blocks, blockquotes) and supports three layout modes: **split** (write and preview side by side), **write** (editor only), **preview** (rendered only). Set your preferred mode in Settings → Appearance. Save with `Cmd+S` or the Save button.

---

## Settings Reference

| Setting | Description |
|---|---|
| LLM Provider | Select your AI provider. Anthropic Claude is active; others coming soon. |
| LLM API Key | Your API key for the selected provider. Stored only in localStorage. |
| Theme | Dark (alien green) or light (army matte green). |
| Editor Layout | Split, write, or preview mode for the rich editor. |
| Education Topics | One topic per line. Changing this clears the daily cache. |
| Main Vault | The Content/ folder (or any markdown directory). |
| Actions | The folder containing items.md. |
| Chats | Where chat conversations are saved as JSON files. |
| Imports | Source folder for the Import tab. |
| Reminders File | A specific .md or .txt file for overnight capture. |
| About | Current version number. |

---

## Technical Notes

**Browser requirement:** Google Chrome (or Chromium-based browsers). The File System Access API (`showDirectoryPicker`, `createWritable`) is not supported in Firefox or Safari.

**Data storage:** All persistent data lives in your file system. The only things stored in the browser's `localStorage` are your API key, theme preference, editor layout preference, education topics, and the daily education card cache.

**Index:** `index/axon-index.json` is auto-generated and auto-maintained. It stores a semantic summary, title, folder, backlinks, and word count for every markdown file. Updated incrementally after each import and rebuilt fully when Claude Code compiles your wiki. Do not edit it manually.

**Security:** Axon sanitizes all markdown output with DOMPurify before rendering. File and folder names are sanitized to prevent path traversal. All API calls are routed through a fetch guard that enforces the configured provider's API domain. Your API key is validated for format before storage.

**Token costs:** The two-layer chat retrieval system keeps token costs roughly constant as your vault grows. The index manifest (titles and summaries) is ~3–5K tokens regardless of vault size. Full file content is only loaded for the top 6 most relevant matches per query.

---

## Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Cmd+S` | Save current file in editor |
| `Esc` | Close Capture modal / close editor |
| `Enter` | Submit task in Capture modal |
| `Shift+Enter` | New line in Chat input |

---

## Roadmap

- Morning Brief — daily AI-generated summary of open actions, recent notes, and calendar events
- Relationship & Commitment Tracker — lightweight per-person context cards built from vault mentions
- Outlook calendar integration via a local Node.js server
- OpenAI and Google Gemini LLM provider support

---

## License

Axon is a personal productivity tool. All content in your vault remains entirely yours.
