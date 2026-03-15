# 🚀 Quick Deployment Guide

## Deploy in 3 Steps

### Step 1: Deploy Backend on Railway

1. Go to https://railway.app and sign up
2. Click "New Project" → "Deploy from GitHub repo"
3. Select your repo and set root directory to `backend`
4. Add environment variables:
   - `GROQ_API_KEY` = your Groq API key
   - `CORS_ORIGINS` = `http://localhost:5173` (update after frontend deploy)
5. Copy your Railway URL (e.g., `https://your-app.railway.app`)

### Step 2: Deploy Frontend on Vercel

1. Go to https://vercel.com and sign up
2. Click "Add New Project" → Import your GitHub repo
3. Configure:
   - Root Directory: `frontend`
   - Framework: Vite
   - Build Command: `npm run build`
   - Output Directory: `dist`
4. Add environment variable:
   - `VITE_API_URL` = your Railway backend URL
5. Deploy!

### Step 3: Update CORS

1. Go back to Railway
2. Update `CORS_ORIGINS` with your Vercel URL:
   ```
   https://your-app.vercel.app,http://localhost:5173
   ```
3. Redeploy backend

**Done!** 🎉 Your app is live!

## Quick Commands

### Railway CLI
```bash
npm i -g @railway/cli
railway login
cd backend
railway init
railway up
```

### Vercel CLI
```bash
npm i -g vercel
cd frontend
vercel
vercel --prod
```

## Need Help?

Check `DEPLOYMENT.md` for detailed instructions.
