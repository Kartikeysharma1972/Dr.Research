# Deployment Guide

This guide will help you deploy the ResearchAgent application to production.

## Prerequisites

- GitHub account
- Groq API key (get it from https://console.groq.com/keys)
- Railway account (free tier available) - https://railway.app
- Vercel account (free tier available) - https://vercel.com

## Step 1: Deploy Backend (Railway)

### Option A: Deploy via Railway Dashboard

1. **Create Railway Account**
   - Go to https://railway.app
   - Sign up with GitHub

2. **Create New Project**
   - Click "New Project"
   - Select "Deploy from GitHub repo"
   - Connect your GitHub account
   - Select your repository
   - Select the `backend` folder as the root directory

3. **Configure Environment Variables**
   - Go to your service → Variables
   - Add these variables:
     ```
     GROQ_API_KEY=your_actual_groq_api_key
     CORS_ORIGINS=https://your-frontend-url.vercel.app,http://localhost:5173
     PORT=8000
     ```

4. **Deploy**
   - Railway will automatically detect Python and deploy
   - Wait for deployment to complete
   - Copy your Railway URL (e.g., `https://your-app.railway.app`)

### Option B: Deploy via Railway CLI

```bash
# Install Railway CLI
npm i -g @railway/cli

# Login
railway login

# Initialize project
cd backend
railway init

# Set environment variables
railway variables set GROQ_API_KEY=your_actual_groq_api_key
railway variables set CORS_ORIGINS=https://your-frontend-url.vercel.app

# Deploy
railway up
```

## Step 2: Deploy Frontend (Vercel)

### Option A: Deploy via Vercel Dashboard

1. **Create Vercel Account**
   - Go to https://vercel.com
   - Sign up with GitHub

2. **Import Project**
   - Click "Add New Project"
   - Import your GitHub repository
   - Configure:
     - **Framework Preset**: Vite
     - **Root Directory**: `frontend`
     - **Build Command**: `npm run build`
     - **Output Directory**: `dist`
     - **Install Command**: `npm install`

3. **Set Environment Variables**
   - Go to Project Settings → Environment Variables
   - Add:
     ```
     VITE_API_URL=https://your-backend-url.railway.app
     ```

4. **Update vercel.json**
   - Update the `vercel.json` file with your actual backend URL:
     ```json
     {
       "rewrites": [
         {
           "source": "/api/:path*",
           "destination": "https://your-backend-url.railway.app/:path*"
         }
       ]
     }
     ```

5. **Deploy**
   - Click "Deploy"
   - Wait for deployment
   - Copy your Vercel URL

### Option B: Deploy via Vercel CLI

```bash
# Install Vercel CLI
npm i -g vercel

# Login
vercel login

# Deploy
cd frontend
vercel

# Set environment variable
vercel env add VITE_API_URL
# Enter: https://your-backend-url.railway.app

# Redeploy with env vars
vercel --prod
```

## Step 3: Update CORS Origins

After deploying frontend, update backend CORS origins:

1. Go to Railway dashboard
2. Your backend service → Variables
3. Update `CORS_ORIGINS`:
   ```
   https://your-frontend-url.vercel.app,http://localhost:5173
   ```
4. Redeploy backend

## Step 4: Test Deployment

1. Visit your Vercel frontend URL
2. Try signing up/login
3. Test research functionality
4. Check browser console for any errors

## Alternative: Deploy Both on Railway

If you prefer to use Railway for both:

1. **Backend**: Follow Step 1 above
2. **Frontend**: 
   - Create new service in same Railway project
   - Select `frontend` folder
   - Set build command: `npm run build`
   - Set start command: `npx serve -s dist -p $PORT`
   - Add environment variable: `VITE_API_URL=https://your-backend-url.railway.app`
   - Deploy

## Troubleshooting

### Backend Issues

- **Port Error**: Make sure `PORT` environment variable is set (Railway provides this automatically)
- **CORS Error**: Update `CORS_ORIGINS` with your frontend URL
- **API Key Error**: Verify `GROQ_API_KEY` is set correctly

### Frontend Issues

- **API Connection Error**: Check `VITE_API_URL` environment variable
- **Build Errors**: Run `npm run build` locally first to check for errors
- **404 Errors**: Verify `vercel.json` rewrites are configured correctly

## Environment Variables Summary

### Backend (Railway)
```
GROQ_API_KEY=your_groq_api_key
CORS_ORIGINS=https://your-frontend.vercel.app,http://localhost:5173
PORT=8000 (auto-set by Railway)
```

### Frontend (Vercel)
```
VITE_API_URL=https://your-backend.railway.app
```

## Cost Estimate

- **Railway**: Free tier includes $5/month credit (enough for small apps)
- **Vercel**: Free tier includes unlimited deployments
- **Total**: $0/month for small projects

## Support

If you encounter issues:
1. Check Railway logs: Railway Dashboard → Your Service → Deployments → Logs
2. Check Vercel logs: Vercel Dashboard → Your Project → Deployments → Logs
3. Check browser console for frontend errors
