---
name: frontend-dev
description: Start and develop the ISIC Project frontend (React 19 + Vite) with Chrome DevTools MCP for live browser inspection, debugging, and UI development. Use when starting the dev server, building UI components, debugging frontend issues, inspecting layout/styles, testing responsiveness, or doing any browser-based frontend work.
---

# Frontend Development with Chrome DevTools

## Quick Reference

**Start development:**
```bash
cd frontend && npm run dev
```
Dev server runs at `http://localhost:5173` with HMR.

**Routes:**
- `/` → redirects to `/login`
- `/login` → Login page
- `/dashboard` → Dashboard page

**Other commands:**
```bash
npm run build    # TypeScript check + production build
npm run lint     # ESLint
npm run preview  # Preview production build locally
```

## Startup Workflow

Follow these steps in order when starting frontend development:

### 1. Install dependencies (if needed)

```bash
cd frontend && npm install
```

### 2. Start the Vite dev server

```bash
cd frontend && npm run dev
```

Wait for the terminal output confirming the server is running (shows the local URL, typically `http://localhost:5173`).

### 3. Open the page in Chrome DevTools

Use Chrome DevTools MCP to navigate to the running app:

```
navigate_page → url: "http://localhost:5173"
```

If no browser page is available, create one first:

```
new_page → url: "http://localhost:5173"
```

### 4. Take a screenshot to verify

```
take_screenshot
```

Always take a screenshot after navigation to confirm the page loaded correctly.

**Note:** The app redirects `/` to `/login`. If you navigate to a route that requires auth and get redirected, re-navigate after taking the screenshot. After `resize_page`, you may also need to re-navigate if the app re-renders and triggers routing changes.

## Chrome DevTools MCP Tools Reference

### Visual Inspection

| Tool | Purpose |
|---|---|
| `take_screenshot` | Capture current page state — use after every UI change to verify |
| `take_snapshot` | Get full DOM snapshot for structural analysis |
| `get_console_message` | Read specific console messages (errors, warnings, logs) |
| `list_console_messages` | List all console output — check for React errors/warnings |

### Navigation and Interaction

| Tool | Purpose |
|---|---|
| `navigate_page` | Navigate to a URL (e.g., switch between `/login` and `/dashboard`) |
| `click` | Click elements by CSS selector |
| `fill` | Fill input fields by selector |
| `fill_form` | Fill multiple form fields at once |
| `type_text` | Type text character by character (for inputs with live validation) |
| `press_key` | Press keyboard keys (Enter, Tab, Escape, etc.) |
| `hover` | Hover over elements to trigger hover states |
| `drag` | Drag elements (for drag-and-drop UIs) |

### Responsive Design

| Tool | Purpose |
|---|---|
| `resize_page` | Resize viewport to test responsive layouts |
| `emulate` | Emulate specific devices (mobile, tablet) |

Common viewport sizes:
- Mobile: `resize_page → width: 375, height: 812` (iPhone)
- Tablet: `resize_page → width: 768, height: 1024` (iPad)
- Desktop: `resize_page → width: 1440, height: 900`

### Debugging

| Tool | Purpose |
|---|---|
| `evaluate_script` | Run JavaScript in the page context (inspect React state, DOM, etc.) |
| `list_network_requests` | List API calls — verify backend communication |
| `get_network_request` | Inspect specific request/response details |
| `list_console_messages` | Check for errors, warnings, React dev messages |

### Performance

| Tool | Purpose |
|---|---|
| `lighthouse_audit` | Run Lighthouse audit for accessibility, best practices, and SEO (**excludes performance**) |
| `performance_start_trace` | Start a performance trace (use this for performance measurement, not Lighthouse) |
| `performance_stop_trace` | Stop trace and get results |
| `performance_analyze_insight` | Analyze performance insights from a trace |
| `take_memory_snapshot` | Capture heap snapshot for memory analysis |

### Page Management

| Tool | Purpose |
|---|---|
| `list_pages` | List all open browser pages/tabs |
| `select_page` | Switch between pages |
| `new_page` | Open a new page |
| `close_page` | Close a page |

## Common Development Workflows

### Implementing a new page or component

1. Start dev server (`npm run dev`)
2. Open in Chrome DevTools (`navigate_page`)
3. Write the component code
4. Take a screenshot to verify the rendered output
5. Test responsiveness with `resize_page` at mobile/tablet/desktop widths
6. Check console for errors (`list_console_messages`)

### Debugging a UI issue

1. Take a screenshot to see current state
2. Use `take_snapshot` to inspect the DOM structure
3. Use `evaluate_script` to inspect element styles or React component state:
   ```js
   // Check computed styles
   getComputedStyle(document.querySelector('.my-element')).display

   // Check React fiber (dev mode)
   document.querySelector('#root')._reactRootContainer
   ```
4. Check console for errors (`list_console_messages`)
5. Check network requests if data-related (`list_network_requests`)

### Testing form interactions

1. Navigate to the page with the form
2. Use `fill` or `fill_form` to populate inputs
3. Use `click` to submit
4. Take a screenshot to verify the result
5. Check network requests to verify API calls were made

### Verifying responsive design

1. Take screenshots at each breakpoint:
   - `resize_page → width: 375, height: 812` (mobile)
   - `resize_page → width: 768, height: 1024` (tablet)
   - `resize_page → width: 1440, height: 900` (desktop)
2. Take a screenshot after each resize
3. Compare layouts for consistency

### Running accessibility checks

1. Run `lighthouse_audit` — covers accessibility, best practices, and SEO (not performance)
2. Review findings and fix issues
3. Use `evaluate_script` to check specific ARIA attributes or focus order

### Measuring performance

`lighthouse_audit` does **not** include performance scoring. Use the trace-based workflow instead:

1. `performance_start_trace` — begin recording
2. Perform the user action you want to measure (navigate, click, etc.)
3. `performance_stop_trace` — stop and get raw trace data
4. `performance_analyze_insight` — analyze the trace for actionable insights

## Tech Stack

- **React** 19.2.0 with **TypeScript** (strict mode)
- **Vite** 8 (beta) with React plugin — HMR enabled
- **React Router** 7.13.1 — `BrowserRouter` with `Routes`/`Route`
- **ESLint** 9 flat config with React Hooks and React Refresh plugins
- Entry point: `src/main.tsx` → `src/App.tsx` → `src/pages/`

## Backend Integration

The backend runs on port `8000` (via `docker-compose up` in `backend/`). When developing frontend features that call the API, ensure the backend is running. Check API connectivity with:

```
list_network_requests
```

If you see CORS or connection errors, verify the backend is up and the Vite proxy is configured (if needed in `vite.config.ts`).
