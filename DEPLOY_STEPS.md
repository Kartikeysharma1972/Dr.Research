# 📋 Step-by-Step Deployment Checklist

## ✅ Pre-Deployment Checklist

- [ ] GitHub repository created and code pushed
- [ ] Groq API key ready (get from https://console.groq.com/keys)
- [ ] Railway account created (https://railway.app)
- [ ] Vercel account created (https://vercel.com)

---

## 🔵 Step 1: Deploy Backend (Railway)

### Via Dashboard:

1. **Login to Railway**
   - Go to https://railway.app
   - Sign in with GitHub

2. **Create New Project**
   - Click "New Project" button
   - Select "Deploy from GitHub repo"
   - Authorize Railway to access your GitHub
   - Select your repository
   - ⚠️ **Important**: Click "Configure" and set Root Directory to `backend`

3. **Set Environment Variables**
   - Click on your service
   - Go to "Variables" tab
   - Click "New Variable" and add:
     ```
     Name: GROQ_API_KEY
     Value: your_actual_groq_api_key_here
     ```
   - Add another:
     ```
     Name: CORS_ORIGINS
     Value: http://localhost:5173
     ```
     (We'll update this after frontend deployment)

4. **Deploy**
   - Railway will auto-detect Python and start deploying
   - Wait for "Deploy Successful" message
   - Click on your service → Settings → Copy the "Public Domain" URL
   - **Save this URL!** (e.g., `https://research-agent-backend.railway.app`)

### Via CLI:

```bash
# Install Railway CLI
npm install -g @railway/cli

# Login
railway login

# Navigate to backend
cd backend

# Initialize project
railway init

# Link to existing project (if you created one in dashboard)
# railway link

# Set environment variables
railway variables set GROQ_API_KEY=your_actual_groq_api_key
railway variables set CORS_ORIGINS=http://localhost:5173

# Deploy
railway up

# Get your URL
railway domain
```

---

## 🟢 Step 2: Deploy Frontend (Vercel)

### Via Dashboard:

1. **Login to Vercel**
   - Go to https://vercel.com
   - Sign in with GitHub

2. **Import Project**
   - Click "Add New..." → "Project"
   - Import your GitHub repository
   - Configure project:
     - **Framework Preset**: Vite
     - **Root Directory**: `frontend` (click "Edit" and change from `/` to `/frontend`)
     - **Build Command**: `npm run build` (should auto-detect)
     - **Output Directory**: `dist` (should auto-detect)
     - **Install Command**: `npm install` (should auto-detect)

3. **Set Environment Variables**
   - Before deploying, click "Environment Variables"
   - Add new variable:
     ```
     Name: VITE_API_URL
     Value: https://your-backend-url.railway.app
     ```
     (Use the Railway URL you saved in Step 1)

4. **Deploy**
   - Click "Deploy"
   - Wait for build to complete
   - Copy your Vercel URL (e.g., `https://research-agent.vercel.app`)
   - **Save this URL!**

### Via CLI:

```bash
# Install Vercel CLI
npm install -g vercel

# Login
vercel login

# Navigate to frontend
cd frontend

# Deploy (first time)
vercel

# Set environment variable
vercel env add VITE_API_URL
# When prompted, enter: https://your-backend-url.railway.app

# Deploy to production
vercel --prod
```

---

## 🔄 Step 3: Update CORS Origins

1. **Go back to Railway Dashboard**
   - Open your backend service
   - Go to "Variables" tab
   - Find `CORS_ORIGINS` variable
   - Click "Edit"
   - Update value to:
     ```
     https://your-frontend-url.vercel.app,http://localhost:5173
     ```
     (Replace with your actual Vercel URL)

2. **Redeploy Backend**
   - Railway will auto-redeploy when you save the variable
   - Or manually trigger: Deployments → Redeploy

---

## 🧪 Step 4: Test Your Deployment

1. **Visit your Vercel URL** in browser
2. **Test Sign Up/Login**
   - Create a new account
   - Try logging in
3. **Test Research Functionality**
   - Enter a research topic
   - Start research
   - Check if report generates
4. **Check Browser Console**
   - Press F12 → Console tab
   - Look for any errors

---

## 🐛 Troubleshooting

### Backend Issues:

**Problem**: Backend not starting
- **Solution**: Check Railway logs → Deployments → Latest → Logs
- Check if `GROQ_API_KEY` is set correctly

**Problem**: CORS errors in browser
- **Solution**: Update `CORS_ORIGINS` with your frontend URL
- Make sure no trailing slashes in URLs

**Problem**: Port error
- **Solution**: Railway sets `PORT` automatically, don't override it

### Frontend Issues:

**Problem**: Can't connect to backend
- **Solution**: Check `VITE_API_URL` environment variable
- Make sure it's `https://` not `http://`
- No trailing slash at the end

**Problem**: Build fails
- **Solution**: Run `npm run build` locally first
- Check for TypeScript/ESLint errors
- Make sure all dependencies are in `package.json`

**Problem**: 404 errors on routes
- **Solution**: Check `vercel.json` configuration
- Make sure output directory is `dist`

---

## 📝 Environment Variables Summary

### Backend (Railway):
```
GROQ_API_KEY=your_groq_api_key_here
CORS_ORIGINS=https://your-frontend.vercel.app,http://localhost:5173
PORT=8000 (auto-set by Railway, don't override)
```

### Frontend (Vercel):
```
VITE_API_URL=https://your-backend.railway.app
```

---

## 💰 Cost Estimate

- **Railway Free Tier**: $5/month credit (enough for small apps)
- **Vercel Free Tier**: Unlimited deployments
- **Total Cost**: $0/month for small projects

---

## 🎉 You're Done!

Your app should now be live at:
- **Frontend**: `https://your-app.vercel.app`
- **Backend**: `https://your-app.railway.app`

Share your frontend URL with others to test!
