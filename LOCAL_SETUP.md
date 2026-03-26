# 🛠️ Local Setup & Testing Guide

Before deploying to Render, test the project locally to ensure everything works.

---

## Prerequisites

- Python 3.11+ installed
- Node.js 18+ and npm installed
- Git installed
- Groq API key (get from https://console.groq.com/keys)

---

## Backend Setup

### 1. Navigate to Backend Directory
```bash
cd research-agent/backend
```

### 2. Create Virtual Environment
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables
Create a `.env` file in the backend directory:
```bash
GROQ_API_KEY=your_groq_api_key_here
CORS_ORIGINS=http://localhost:5173,http://localhost:3000
```

### 5. Start Backend Server
```bash
python main.py
```

Backend should start at: `http://localhost:8000`

### 6. Test Backend
Open browser and visit:
- Health check: http://localhost:8000/health
- API docs: http://localhost:8000/docs

---

## Frontend Setup

### 1. Navigate to Frontend Directory
Open a **new terminal** and run:
```bash
cd research-agent/frontend
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Configure Environment Variables
Create a `.env` file in the frontend directory:
```bash
VITE_API_URL=http://localhost:8000
```

### 4. Start Frontend Dev Server
```bash
npm run dev
```

Frontend should start at: `http://localhost:5173`

### 5. Test Frontend
Open browser and visit: http://localhost:5173

---

## Testing Checklist

### ✅ Backend Tests
- [ ] Backend starts without errors
- [ ] Health endpoint returns `{"status": "ok"}`
- [ ] API docs page loads at `/docs`
- [ ] No errors in terminal logs

### ✅ Frontend Tests
- [ ] Frontend loads without errors
- [ ] Login/Signup modal appears
- [ ] No console errors (F12 → Console)
- [ ] UI renders correctly

### ✅ Integration Tests
- [ ] **Sign Up**: Create a new account
  - Enter name, email, password
  - Should show success message
  - Check backend terminal for "New user registered" log
  
- [ ] **Login**: Sign in with created account
  - Enter email and password
  - Should redirect to main dashboard
  - Check backend terminal for "User logged in" log

- [ ] **Start Research**: Test the research pipeline
  - Enter topic: "Benefits of renewable energy"
  - Click "Start Research"
  - Watch pipeline progress in real-time
  - Verify all nodes complete: planner → search → extraction → synthesis → reflection → refinement
  - Check final report appears

- [ ] **Check Backend Logs**: 
  - Should see research events streaming
  - No error messages
  - All nodes completing successfully

### ✅ Error Handling Tests
- [ ] Try invalid login credentials → Should show error
- [ ] Try duplicate signup email → Should show error
- [ ] Try empty research topic → Should show validation error
- [ ] Stop backend while research running → Frontend should handle gracefully

---

## Common Issues & Solutions

### Backend Issues

**Problem: `ModuleNotFoundError`**
```bash
# Solution: Reinstall dependencies
pip install -r requirements.txt
```

**Problem: `GROQ_API_KEY not found`**
```bash
# Solution: Check .env file exists and has correct key
# Make sure .env is in backend directory
```

**Problem: Port 8000 already in use**
```bash
# Solution: Kill existing process or change port
# Windows: netstat -ano | findstr :8000
# Then: taskkill /PID <process_id> /F
```

### Frontend Issues

**Problem: `npm install` fails**
```bash
# Solution: Clear cache and retry
npm cache clean --force
rm -rf node_modules package-lock.json
npm install
```

**Problem: `VITE_API_URL is not defined`**
```bash
# Solution: Create .env file in frontend directory
# Add: VITE_API_URL=http://localhost:8000
```

**Problem: CORS errors in console**
```bash
# Solution: Check backend CORS_ORIGINS includes http://localhost:5173
# Restart backend after changing .env
```

---

## Verify Before Deployment

Before deploying to Render, ensure:

1. ✅ Both backend and frontend run locally without errors
2. ✅ All features work (signup, login, research)
3. ✅ No console errors in browser
4. ✅ Backend logs show no errors
5. ✅ Research pipeline completes successfully
6. ✅ `.env` files are NOT committed to git (check .gitignore)
7. ✅ All dependencies are in requirements.txt and package.json
8. ✅ Code is pushed to GitHub

---

## Quick Start Commands

### Terminal 1 (Backend):
```bash
cd research-agent/backend
python -m venv venv
venv\Scripts\activate  # Windows
pip install -r requirements.txt
python main.py
```

### Terminal 2 (Frontend):
```bash
cd research-agent/frontend
npm install
npm run dev
```

### Access:
- Frontend: http://localhost:5173
- Backend: http://localhost:8000
- API Docs: http://localhost:8000/docs

---

**Once everything works locally, proceed to RENDER_DEPLOYMENT_GUIDE.md for deployment! 🚀**
