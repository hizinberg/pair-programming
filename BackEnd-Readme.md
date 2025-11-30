# Backend File Responsibilities  
(FastAPI + WebSockets + Code Execution + Autocomplete)

This document explains the responsibilities of each important file in the `pair-programming-backend` project.

---

# 📌 API Overview

A simple backend for a real-time collaborative code editor with live execution and autocomplete.

---

## 👥 User Endpoints (Public)

### **POST /rooms**
Create a new collaborative coding room.  
Returns `{ roomId }`.

### **GET /rooms/{room_id}**
Fetch information about a specific room.  
Used by frontend to confirm room existence + get initial code state.

### **POST /autocomplete**
Returns code suggestions based on:
```
{ code, cursorPosition, language }
```
Uses a custom AST-based suggestion generator.

### **POST /execute**
Executes the submitted code and returns:
```
{ output }
```
Execution is sandboxed.

---

## 🔐 Admin Endpoints

### **GET /admin/rooms**
View all rooms currently stored in memory or database.

---

# 📁 Directory Structure & File Responsibilities

```
pair-programming-backend/
│── app/
│   ├── __init__.py
│   ├── routers/
│   ├── services/
│   ├── db.py
│   ├── logger.py
│   ├── main.py
│   ├── models.py
│   └── schemas.py
│── requirements.txt
└── README.md
```

---

# 🧠 app/main.py  
**The central entry point of the FastAPI application.**

Responsible for:

- Creating the FastAPI app instance  
- Mounting all routers (`rooms`, `admin`, `autocomplete`, `execute`)  
- Initializing WebSocket endpoint at:  
  `/ws/{room_id}/{client_id}`
- Setting up lifespan events (startup/shutdown if used)
- Wiring services into the routing layer

This is the file that `uvicorn` runs:
```
uvicorn app.main:app --reload
```

---

# 🗂 app/routers/

Contains all API endpoint definitions.  
Each router corresponds to a logical module.

### **rooms.py**
- `POST /rooms` → create a room  
- `GET /rooms/{room_id}` → fetch room data  
- Handles interaction with the room store or database  
- Calls service functions from `services/rooms_service.py`

### **autocomplete.py**
- `POST /autocomplete`  
- Parses request data and calls the AST-based suggestion engine  
- Returns a list of suggestions  
- Does not perform execution or WebSocket operations

### **execute.py**
- `POST /execute`  
- Runs the incoming code through the execution service  
- Returns execution output (stdout, errors, etc.)

### **admin.py**
- `GET /admin/rooms`  
- Used to inspect all active rooms  
- Intended for debugging or admin inspection

### **ws_router.py** 
- Defines the WebSocket route  
- Manages:
  - Client connection/disconnection  
  - Broadcasting `code_update` messages  
  - Broadcasting `execution_lock` and `execution_result`

---

# 🧩 app/services/

Contains all business logic.  
Routers call these files for real processing.

### **rooms_service.py**
Handles room creation, retrieval, storage.

Responsibilities:
- Generate room IDs  
- Maintain per-room state (code content, active connections)  
- Store state in memory or Postgres (depending on configuration)  
- Provide helpers for WebSocket broadcasting

### **ast_name.py**
- Implements AST-based suggestion engine  
- Parses code using `ast` module  
- Returns "globals", "functions", "classes", "imports", "params", "locals"


### **connection_manager.py**
- Manages active WebSocket connections  
- Stores per-room clients  
- Handles:
  - Broadcasting messages  
  - Ignoring sender when appropriate  
  - Sending code updates  
  - Sending lock/unlock signals  
  - Clean disconnections

---

---

# 🧿 app/db.py  
Handles database layer.

Responsible for:

- Connecting to Postgres (if used)  
- Providing room persistence  
- Acting as a boundary between services and storage  

If running purely in-memory, this file may contain the in-memory store.

---

# 📝 app/models.py  
Database or internal model definitions.

Contains:

- Room model  
- State model  
- Internal WebSocket connection models  
- Objects representing stored values

These are not Pydantic schemas; they are Python-level logic objects.

---

# 📄 app/schemas.py  
Pydantic request/response models.

Examples include:

### `AutocompleteRequest`
```
code: str
cursorPosition: int
language: str
```

### `ExecuteResponse`
```
output: str
```

### `RoomCreateResponse`
```
roomId: str
```

Schemas provide validation and typed responses.

---

# 📰 app/logger.py  
Central logger configuration.

Responsibilities:

- Configures formatting  
- Adds timestamped logs  
- Used by services & routers  
- Helps trace WebSocket activity, execution, and room creation events

---

# 📦 requirements.txt  
Lists backend dependencies:

Includes:
- fastapi  
- uvicorn  
- websockets  
- python-dotenv  
- async libraries  
- AST utilities  
- Optional DB libraries (psycopg2, sqlalchemy)

---

# 📘 README.md  
Backend-specific instructions:

- How to run FastAPI  
- Environment setup  
- Explanation of execution engine  
- Explanation of autocomplete system  
- WebSocket protocol description  
- Room model specification  
- Improvements + limitations

---

# Summary Table

| File | Purpose |
|------|---------|
| **main.py** | App entry, mounts routers, websocket endpoint |
| **routers/** | Defines REST & WS endpoints |
| **services/** | Core business logic (rooms, autocomplete, execution, ws) |
| **db.py** | Database/in-memory room store |
| **models.py** | Internal data models |
| **schemas.py** | Pydantic schemas |
| **logger.py** | Centralized logging |
| **requirements.txt** | Backend dependencies |
| **README.md** | Backend documentation |

---

If you want, I can also produce:

✔ A **backend-logic.md** deep-dive  
✔ A **sequence diagram** of WebSocket & execution flow  
✔ A **combined architecture diagram** for FE + BE  

Just tell me.
