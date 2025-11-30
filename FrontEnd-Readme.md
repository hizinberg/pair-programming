# Frontend File Responsibilities  
(React + JavaScript + WebSockets)

This document explains the **purpose of each important file** in the `pair-programming-frontend` project.  
All irrelevant or auto-generated files are intentionally ignored.

---

## 📁 `src/`

### **App.jsx**
- Main application entry for routing.
- Defines two routes:
  - `/` → Landing page (room creation + join UI)
  - `/room/:roomId` → Code editor + collaboration environment
- Wraps everything inside `BrowserRouter`.

---

### **Rooms.jsx** (Main Collaborative Editor Component)
This is the **heart of the frontend application**.

Responsible for:

#### 🔹 WebSocket Runtime
- Connects to backend WebSocket at  
  `ws://localhost:8000/ws/<roomId>/<clientId>`
- Receives:
  - `code_update`
  - `execution_lock`
  - `execution_result`
- Sends:
  - Code updates
  - Execution lock notifications
  - Execution results

#### 🔹 Code Editor Logic
- Maintains editor state (`codeContent`)
- Tracks cursor position
- Broadcasts changes to other collaborators
- Disables editing when locked during execution

#### 🔹 Autocomplete System
- Debounced call to `POST /autocomplete`
- Displays suggestions via `SuggestionDisplay.jsx`
- Supports:
  - Arrow-key navigation
  - Tab / Enter to apply suggestion
  - Ctrl + Space to trigger suggestions manually

#### 🔹 Code Execution Workflow
- Sends code to `POST /execute`
- Locks editor for all connected users
- Shows terminal output in the `Terminal` component

#### 🔹 UI Structure
- Header (Room info + Run button)
- Editor area (textarea)
- Suggestion dropdown
- Terminal output panel
- Status bar

This file coordinates **all real-time collaboration**, **autocomplete**, **execution**, and **editor controls**.

---

### **SuggestionDisplay.jsx**
Responsible for:

#### 🔹 Suggestion List Rendering
- Displays autocomplete suggestions under the cursor in a floating panel
- Styled to resemble VS Code
- Highlights currently selected suggestion

#### 🔹 Positioning Engine
- Computes pixel-accurate position of the cursor inside a textarea
- Uses computed styles + hidden DOM clone to measure cursor offset
- Ensures suggestion box appears at the correct location even with wrapping text

#### 🔹 Interaction
- Mouse click to choose a suggestion
- Prevents editor blur via `onMouseDown` override

This component is crucial for making autocomplete suggestions appear correctly relative to the user’s text.

---

### **Terminal.jsx**
Responsible for:

#### 🔹 Displaying Code Execution Output
- Shows stdout/stderr results returned from `/execute`


# Summary

| File | Purpose |
|------|---------|
| **App.jsx** | Routes + global structure |
| **Rooms.jsx** | Full collaborative editor logic, websockets, autocomplete, execution |
| **SuggestionDisplay.jsx** | Suggestion popup UI + cursor positioning |
| **Terminal.jsx** | Code execution output UI |

---
