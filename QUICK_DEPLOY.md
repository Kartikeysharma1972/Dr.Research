# 🚀 Quick Deployment Guide

## Step 1: Deploy Backend on Railway

### Via Railway Dashboard:

1. **Go to Railway**
   - Open https://railway.app
   - Click "Start a New Project" or "Login"
   - Sign in with GitHub

2. **Create New Project**
   - Click "New Project"
   - Select "Deploy from GitHub repo"
   - Authorize Railway to access GitHub if asked
   - Select your repository: `Kartikeysharma1972/Dr.Research`
   - Click "Deploy Now"

3. **Configure Root Directory**
   - After project is created, click on your service
   - Go to "Settings" tab
   - Scroll down to "Root Directory"
   - Click "Edit" and set it to: `backend`
   - Click "Save"

4. **Add Environment Variables**
   - Stay in "Settings" → Go to "Variables" tab
   - Click "New Variable"
   - Add these one by one:
   
   **Variable 1:**
   ```
   Name: GROQ_API_KEY
   Value: [Your Groq API Key from https://console.groq.com/keys]
   ```
   
   **Variable 2:**
   ```
   Name: CORS_ORIGINS
   Value: http://localhost:5173
   ```
   (We'll update this after frontend deployment)

5. **Get Your Backend URL**
   - Go to "Settings" → "Networking"
   - Under "Public Domain", click "Generate Domain"
   - Copy the URL (e.g., `https://dr-research-backend.railway.app`)
   - **SAVE THIS URL!** You'll need it for frontend

6. **Wait for Deployment**
   - Railway will automatically build and deploy
   - Check "Deployments" tab to see progress
   - Wait for "Deploy Successful" ✅

---

## Step 2: Deploy Frontend on Vercel

### Via Vercel Dashboard:

1. **Go to Vercel**
   - Open https://vercel.com
   - Click "Sign Up" or "Login"
   - Sign in with GitHub

2. **Import Project**
   - Click "Add New..." → "Project"
   - Click "Import" next to your repository: `Kartikeysharma1972/Dr.Research`
   - Click "Import"

3. **Configure Project**
   - **Framework Preset**: Should auto-detect "Vite" ✅
   - **Root Directory**: Click "Edit" and change from `/` to `frontend`
   - **Build Command**: `npm run build` (should be auto-filled)
   - **Output Directory**: `dist` (should be auto-filled)
   - **Install Command**: `npm install` (should be auto-filled)

4. **Add Environment Variable**
   - Before clicking "Deploy", scroll down to "Environment Variables"
   - Click "Add" button
   - Add:
     ```
     Name: VITE_API_URL
     Value: https://your-backend-url.railway.app
     ```
     (Replace with your actual Railway backend URL from Step 1)

5. **Deploy**
   - Click "Deploy" button
   - Wait for build to complete (2-3 minutes)
   - Once done, you'll see "Congratulations!"
   - Copy your Vercel URL (e.g., `https://dr-research.vercel.app`)
   - **SAVE THIS URL!**

---

## Step 3: Update CORS Origins

1. **Go back to Railway**
   - Open your backend service
   - Go to "Settings" → "Variables"
   - Find `CORS_ORIGINS` variable
   - Click "Edit" (pencil icon)
   - Update value to:
     ```
     https://your-frontend-url.vercel.app,http://localhost:5173
     ```
     (Replace with your actual Vercel URL)
   - Click "Save"
   - Railway will auto-redeploy

2. **Wait for Redeploy**
   - Check "Deployments" tab
   - Wait for new deployment to complete ✅

---

## Step 4: Test Your Deployment

1. **Open Your Frontend**
   - Visit your Vercel URL in browser
   - Example: `https://dr-research.vercel.app`

2. **Test Sign Up**
   - Click "Sign Up"
   - Create a test account
   - Should work! ✅

3. **Test Research**
   - Enter a research topic
   - Click "Start Research"
   - Should start processing! ✅

4. **Check for Errors**
   - Press F12 → Console tab
   - Look for any red errors
   - If you see CORS errors, make sure Step 3 is done correctly

---

## ✅ You're Done!

Your app is now live:
- **Frontend**: `https://your-app.vercel.app`
- **Backend**: `https://your-app.railway.app`

Share your frontend URL with others! 🎉

---

## 🐛 Troubleshooting

### Backend Issues:

**Problem**: Backend not starting
- Check Railway logs: Deployments → Latest → View Logs
- Make sure `GROQ_API_KEY` is set correctly
- Check if build completed successfully

**Problem**: CORS errors
- Make sure `CORS_ORIGINS` includes your Vercel URL
- No trailing slashes in URLs
- Redeploy backend after updating CORS

### Frontend Issues:

**Problem**: Can't connect to backend
- Check `VITE_API_URL` environment variable in Vercel
- Make sure it's `https://` not `http://`
- No trailing slash
- Redeploy frontend after updating

**Problem**: Build fails
- Check Vercel build logs
- Make sure all dependencies are in `package.json`
- Try building locally: `cd frontend && npm run build`

---

## 📝 Quick Reference

### Backend Environment Variables (Railway):
```
GROQ_API_KEY=your_groq_api_key
CORS_ORIGINS=https://your-frontend.vercel.app,http://localhost:5173
PORT=8000 (auto-set by Railway)
```

### Frontend Environment Variables (Vercel):
```
VITE_API_URL=https://your-backend.railway.app
```

---

## 💡 Tips

- Railway gives $5/month free credit (enough for small apps)
- Vercel is completely free for personal projects
- Both auto-deploy on git push (after initial setup)
- Check logs if something doesn't work

Good luck! 🚀
