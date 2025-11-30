# Pair Programming Full-Stack Prototype  
Parent Repository  
(Contains links + documentation for both frontend and backend implementations)

---

## 📌 Overview

This project is a **simplified real-time pair-programming platform** that allows two users to:

- Join the same room  
- Edit code collaboratively  
- View live updates instantly via WebSockets  
- Receive mock autocomplete suggestions  
- Execute code directly in the browser  
- Leverage an AST-based suggestion engine (custom implementation)

This parent repository links to the two separate codebases:

- **Frontend (React + JavaScript)**  
  https://github.com/hizinberg/pair-programming-frontend.git

- **Backend (FastAPI + Python)**  
  https://github.com/hizinberg/pair-programming-backend.git

Each repository includes its own setup instructions and independent READMEs.

---

## 🚀 Features Implemented

### ✔ Real-Time Collaborative Coding
- Implemented via **WebSockets**
- Lightweight syncing model — "last write wins"
- Rooms isolate code sessions so users only collaborate inside their own room

### ✔ Room Creation & Joining
- Auto-generated room IDs
- Join via URL format:  
  `/room/<roomId>`

### ✔ Autocomplete System (Enhanced)
- POST `/autocomplete`
- Accepts `{ code, cursorPosition, language }`
- **AST-powered rule-based suggestion engine**
- Not a real AI model, but more advanced than the minimum requirements

### ✔ Code Execution Engine
- Executes Python code in a safe sandboxed backend environment
- Displays output/errors in real time to the frontend

### ✔ Web-Friendly UI
- Editor built using **Monaco Editor**
- Written in **JavaScript**, not TypeScript
- Uses **React Router** for navigation
- **No Redux** — state is handled using Context + internal hooks

---

## 📁 Repository Architecture

This parent repository acts as a meta-directory containing documentation.  
All actual code lives in the following repos:

### 🔹 Frontend Repository
**React, JavaScript, React Router, WebSockets**

https://github.com/hizinberg/pair-programming-frontend.git

Contains:

- Live code editor  
- WebSocket client handler  
- Autocomplete UI  
- Code execution output panel  
- Room creation/join screens  
- No Redux — implementation uses custom state handling  

### 🔹 Backend Repository  
**FastAPI, Python, WebSockets, AST utilities**

https://github.com/hizinberg/pair-programming-backend.git

Contains:

- REST endpoints: room creation, autocomplete, execution
- WebSocket server for broadcasting changes
- In-memory + Postgres-backed room state
- Code execution sandbox
- AST suggestion module

---

## 📄 Additional Documentation in This Repository

You may include these two files in this parent repo:

### `frontend-logic.md`
Detailed explanation of:

- WebSocket events and listeners  
- Internal editor state flow  
- Debouncing model for autocomplete  
- Code execution request flow  
- Routing structure  
- Why Redux was not used  

### `backend-logic.md`
Detailed explanation of:

- How WebSocket rooms are managed  
- Room registry + Postgres persistence  
- Code execution safety model  
- AST-based autocomplete engine  
- Message broadcasting architecture  
- Project folder structure (routers/services/models/utils)  

These files serve as deep-dive technical documentation.

---

