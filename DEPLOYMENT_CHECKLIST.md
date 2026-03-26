# ✅ Deployment Checklist for Render

Use this checklist to ensure smooth deployment.

---

## Pre-Deployment Checklist

### 1. Code Preparation
- [ ] All code is committed to GitHub
- [ ] Latest changes are pushed to main branch
- [ ] `.env` files are in `.gitignore` (NOT committed)
- [ ] `requirements.txt` has all backend dependencies
- [ ] `package.json` has all frontend dependencies

### 2. API Keys & Credentials
- [ ] Groq API key obtained from https://console.groq.com/keys
- [ ] API key tested and working locally
- [ ] API key saved securely (NOT in code)

### 3. Local Testing Complete
- [ ] Backend runs locally without errors
- [ ] Frontend runs locally without errors
- [ ] Signup/Login works
- [ ] Research pipeline completes successfully
- [ ] No console errors in browser
- [ ] All features tested and working

### 4. GitHub Repository
- [ ] Repository is public or Render has access
- [ ] Repository URL: `https://github.com/Kartikeysharma1972/Dr.Research`
- [ ] Main branch is up to date
- [ ] All files are pushed

---

## Backend Deployment (Render Web Service)

### Step 1: Create Service
- [ ] Logged into Render dashboard
- [ ] Created new Web Service
- [ ] Connected GitHub repository
- [ ] Selected correct repository

### Step 2: Configure Service
- [ ] Name: `research-agent-backend`
- [ ] Region: Selected (e.g., Oregon)
- [ ] Branch: `main`
- [ ] Root Directory: `research-agent/backend`
- [ ] Runtime: `Python 3`
- [ ] Build Command: `pip install -r requirements.txt`
- [ ] Start Command: `uvicorn main:app --host 0.0.0.0 --port $PORT`

### Step 3: Environment Variables
- [ ] `GROQ_API_KEY` = `your_actual_key`
- [ ] `CORS_ORIGINS` = `http://localhost:5173`
- [ ] `PYTHON_VERSION` = `3.11.0` (optional)

### Step 4: Deploy & Verify
- [ ] Clicked "Create Web Service"
- [ ] Deployment completed successfully
- [ ] Status shows "Live" ✅
- [ ] Backend URL copied and saved: `___________________________`
- [ ] Tested health endpoint: `https://your-backend.onrender.com/health`
- [ ] API docs accessible: `https://your-backend.onrender.com/docs`

---

## Frontend Deployment (Render Static Site)

### Step 1: Create Static Site
- [ ] Created new Static Site in Render
- [ ] Connected same GitHub repository
- [ ] Selected correct repository

### Step 2: Configure Site
- [ ] Name: `research-agent-frontend`
- [ ] Branch: `main`
- [ ] Root Directory: `research-agent/frontend`
- [ ] Build Command: `npm install && npm run build`
- [ ] Publish Directory: `dist`

### Step 3: Environment Variables
- [ ] `VITE_API_URL` = `https://your-backend-url.onrender.com`
- [ ] Backend URL is correct (no trailing slash)
- [ ] Using `https://` not `http://`

### Step 4: Deploy & Verify
- [ ] Clicked "Create Static Site"
- [ ] Build completed successfully
- [ ] Status shows "Live" ✅
- [ ] Frontend URL copied and saved: `___________________________`
- [ ] Site loads in browser
- [ ] No console errors (F12 → Console)

---

## Post-Deployment Configuration

### Update CORS Settings
- [ ] Went back to backend service in Render
- [ ] Clicked "Environment" tab
- [ ] Updated `CORS_ORIGINS` to include frontend URL
- [ ] New value: `https://your-frontend.onrender.com,http://localhost:5173`
- [ ] Saved changes
- [ ] Backend redeployed automatically
- [ ] Redeployment completed successfully

---

## Final Testing

### Test Authentication
- [ ] Opened frontend URL in browser
- [ ] Clicked "Sign Up"
- [ ] Created test account successfully
- [ ] Logged in successfully
- [ ] No errors in browser console

### Test Research Pipeline
- [ ] Entered research topic
- [ ] Clicked "Start Research"
- [ ] Pipeline started successfully
- [ ] All nodes progressing (planner → search → extraction → synthesis → reflection → refinement)
- [ ] Final report generated
- [ ] Report displays correctly

### Test Error Handling
- [ ] Tried invalid login → Shows error message
- [ ] Tried duplicate signup → Shows error message
- [ ] All error messages display correctly

### Performance Check
- [ ] First load time acceptable (cold start may be slow on free tier)
- [ ] Subsequent requests faster
- [ ] No timeout errors
- [ ] Research completes within reasonable time

---

## Troubleshooting Completed

If you encountered issues, mark what you fixed:

- [ ] Fixed CORS errors by updating backend CORS_ORIGINS
- [ ] Fixed API connection by correcting VITE_API_URL
- [ ] Fixed build errors by updating dependencies
- [ ] Fixed runtime errors by checking logs
- [ ] Other: ___________________________

---

## URLs for Reference

**Production URLs:**
- Frontend: `___________________________`
- Backend: `___________________________`
- API Docs: `___________________________/docs`

**Local URLs:**
- Frontend: `http://localhost:5173`
- Backend: `http://localhost:8000`
- API Docs: `http://localhost:8000/docs`

---

## Next Steps After Deployment

- [ ] Share frontend URL with users
- [ ] Monitor Render dashboard for errors
- [ ] Check usage and bandwidth
- [ ] Set up monitoring/alerts (optional)
- [ ] Consider upgrading to paid plan for:
  - Persistent storage (users.csv currently resets)
  - No cold starts
  - Better performance
- [ ] Add custom domain (optional)
- [ ] Implement proper database (PostgreSQL) for production
- [ ] Add JWT authentication for better security
- [ ] Set up error tracking (Sentry)

---

## Support Resources

- **Render Docs**: https://render.com/docs
- **Render Community**: https://community.render.com
- **Check Logs**: Render Dashboard → Your Service → Logs
- **Status Page**: https://status.render.com

---

## Deployment Complete! 🎉

Once all items are checked, your app is live and ready to use!

**Share your app:** `https://your-frontend.onrender.com`

---

**Date Deployed:** ___________________________  
**Deployed By:** ___________________________  
**Notes:** ___________________________
