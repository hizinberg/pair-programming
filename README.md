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
- Executes Python using exec ( WARNING this is unsafe )
- Displays output/errors in real time to the frontend

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

