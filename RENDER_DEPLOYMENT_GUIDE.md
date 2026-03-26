# 🚀 Render Deployment Guide - Dr.Research AI Agent

Complete step-by-step guide to deploy your research agent project on Render.

---

## 📋 Prerequisites

Before starting, ensure you have:
- ✅ GitHub account
- ✅ Render account (sign up at https://render.com)
- ✅ Groq API key (get from https://console.groq.com/keys)
- ✅ Project pushed to GitHub repository

---

## 🎯 Deployment Overview

We'll deploy:
1. **Backend** (FastAPI) → Render Web Service
2. **Frontend** (React/Vite) → Render Static Site

---

## Part 1: Deploy Backend on Render

### Step 1: Create New Web Service

1. **Login to Render**
   - Go to https://dashboard.render.com
   - Click "New +" button → Select "Web Service"

2. **Connect GitHub Repository**
   - Click "Connect account" if not already connected
   - Find and select your repository: `Kartikeysharma1972/Dr.Research` (or your repo name)
   - Click "Connect"

### Step 2: Configure Backend Service

Fill in the following settings:

**Basic Settings:**
- **Name**: `research-agent-backend` (or any name you prefer)
- **Region**: Choose closest to your users (e.g., Oregon, Frankfurt)
- **Branch**: `main` (or your default branch)
- **Root Directory**: `research-agent/backend`
- **Runtime**: `Python 3`

**Build & Deploy Settings:**
- **Build Command**: `pip install -r requirements.txt`
- **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`

### Step 3: Add Environment Variables

Click "Advanced" → "Add Environment Variable" and add these:

| Key | Value | Notes |
|-----|-------|-------|
| `GROQ_API_KEY` | `your_groq_api_key_here` | Get from https://console.groq.com/keys |
| `CORS_ORIGINS` | `http://localhost:5173` | Will update after frontend deployment |
| `PYTHON_VERSION` | `3.11.0` | Optional: specify Python version |

### Step 4: Deploy Backend

1. Click "Create Web Service"
2. Wait for deployment (3-5 minutes)
3. Watch the logs for any errors
4. Once deployed, you'll see "Live" status ✅

### Step 5: Get Backend URL

1. Copy your backend URL from the dashboard
   - Format: `https://research-agent-backend.onrender.com`
   - **SAVE THIS URL** - you'll need it for frontend!

---

## Part 2: Deploy Frontend on Render

### Step 1: Create Static Site

1. **From Render Dashboard**
   - Click "New +" → Select "Static Site"

2. **Connect Same Repository**
   - Select your repository: `Kartikeysharma1972/Dr.Research`
   - Click "Connect"

### Step 2: Configure Frontend Service

Fill in the following settings:

**Basic Settings:**
- **Name**: `research-agent-frontend` (or any name you prefer)
- **Branch**: `main`
- **Root Directory**: `research-agent/frontend`

**Build Settings:**
- **Build Command**: `npm install && npm run build`
- **Publish Directory**: `dist`

### Step 3: Add Environment Variable

Click "Advanced" → "Add Environment Variable":

| Key | Value |
|-----|-------|
| `VITE_API_URL` | `https://your-backend-url.onrender.com` |

⚠️ **IMPORTANT**: Replace with your actual backend URL from Part 1, Step 5

### Step 4: Deploy Frontend

1. Click "Create Static Site"
2. Wait for build and deployment (2-4 minutes)
3. Watch build logs for any errors
4. Once deployed, you'll see "Live" status ✅

### Step 5: Get Frontend URL

1. Copy your frontend URL from the dashboard
   - Format: `https://research-agent-frontend.onrender.com`
   - **SAVE THIS URL** - this is your live app!

---

## Part 3: Update CORS Settings

### Step 1: Update Backend CORS

1. **Go back to Backend Service**
   - Navigate to your backend web service in Render dashboard
   - Click "Environment" tab

2. **Update CORS_ORIGINS**
   - Find `CORS_ORIGINS` variable
   - Click "Edit"
   - Update value to:
     ```
     https://research-agent-frontend.onrender.com,http://localhost:5173
     ```
   - Replace `research-agent-frontend.onrender.com` with your actual frontend URL
   - Click "Save Changes"

3. **Redeploy Backend**
   - Render will automatically redeploy
   - Wait for deployment to complete (1-2 minutes)

---

## Part 4: Test Your Deployment

### Step 1: Access Your App

1. Open your frontend URL in browser
   - Example: `https://research-agent-frontend.onrender.com`

2. You should see the login/signup page ✅

### Step 2: Test Authentication

1. Click "Sign Up"
2. Create a test account:
   - Name: Test User
   - Email: test@example.com
   - Password: test123
3. Click "Create Account"
4. Should show success message ✅

### Step 3: Test Research

1. After login, enter a research topic:
   - Example: "Impact of AI on healthcare"
2. Click "Start Research"
3. Watch the pipeline progress in real-time ✅
4. Wait for final report to generate

### Step 4: Check for Errors

1. Press `F12` to open browser console
2. Look for any red errors
3. Common issues:
   - **CORS errors**: Make sure Part 3 is completed correctly
   - **API connection errors**: Verify backend URL in frontend env vars
   - **500 errors**: Check backend logs in Render dashboard

---

## 🐛 Troubleshooting

### Backend Issues

**Problem: Backend won't start**
- Check Render logs: Dashboard → Your Service → Logs
- Verify `GROQ_API_KEY` is set correctly
- Check Python version compatibility
- Verify all dependencies in requirements.txt

**Problem: CORS errors in browser**
- Make sure `CORS_ORIGINS` includes your frontend URL
- No trailing slashes in URLs
- Redeploy backend after updating CORS
- Clear browser cache

**Problem: 500 Internal Server Error**
- Check backend logs for Python errors
- Verify Groq API key is valid
- Check if all required packages are installed

### Frontend Issues

**Problem: Can't connect to backend**
- Verify `VITE_API_URL` in Render environment variables
- Make sure it's `https://` not `http://`
- No trailing slash at the end
- Redeploy frontend after updating

**Problem: Build fails**
- Check Render build logs
- Verify all dependencies in package.json
- Try building locally: `cd frontend && npm run build`
- Check Node.js version compatibility

**Problem: White screen / blank page**
- Check browser console for errors
- Verify build completed successfully
- Check if dist folder was created
- Verify publish directory is set to `dist`

### Database Issues

**Problem: Users not persisting**
- Backend uses CSV file storage
- On Render free tier, files are ephemeral
- Consider upgrading to paid plan for persistent disk
- Or migrate to a proper database (PostgreSQL)

---

## 📝 Environment Variables Reference

### Backend (Web Service)
```
GROQ_API_KEY=gsk_xxxxxxxxxxxxxxxxxxxxx
CORS_ORIGINS=https://your-frontend.onrender.com,http://localhost:5173
PYTHON_VERSION=3.11.0
```

### Frontend (Static Site)
```
VITE_API_URL=https://your-backend.onrender.com
```

---

## 🔄 Continuous Deployment

Both services are configured for auto-deployment:
- Push to GitHub → Render automatically rebuilds and deploys
- Check "Events" tab in Render to see deployment history
- Disable auto-deploy in Settings if needed

---

## 💡 Important Notes

### Free Tier Limitations
- **Backend**: Spins down after 15 minutes of inactivity
- **First request**: May take 30-60 seconds (cold start)
- **Storage**: Files are ephemeral (users.csv will reset)
- **Bandwidth**: 100GB/month free

### Upgrading to Paid Plan
- **Persistent storage**: $7/month for 1GB disk
- **Always-on**: No cold starts
- **Better performance**: More resources

### Custom Domain (Optional)
1. Go to Settings → Custom Domain
2. Add your domain
3. Update DNS records as instructed
4. Wait for SSL certificate (automatic)

---

## 🎉 You're Live!

Your app is now deployed:
- **Frontend**: `https://research-agent-frontend.onrender.com`
- **Backend**: `https://research-agent-backend.onrender.com`
- **API Docs**: `https://research-agent-backend.onrender.com/docs`

Share your frontend URL with users! 🚀

---

## 📞 Need Help?

- **Render Docs**: https://render.com/docs
- **Render Community**: https://community.render.com
- **Check Logs**: Always check logs first for errors
- **GitHub Issues**: Report bugs in your repository

---

## 🔐 Security Best Practices

1. **Never commit API keys** to GitHub
2. **Use environment variables** for all secrets
3. **Enable HTTPS** (automatic on Render)
4. **Rotate API keys** periodically
5. **Monitor usage** to detect abuse
6. **Add rate limiting** for production use

---

## 🚀 Next Steps

1. ✅ Test all features thoroughly
2. ✅ Set up monitoring and alerts
3. ✅ Add custom domain (optional)
4. ✅ Upgrade to paid plan for production
5. ✅ Implement proper database (PostgreSQL)
6. ✅ Add user session management (JWT tokens)
7. ✅ Set up error tracking (Sentry)
8. ✅ Add analytics (Google Analytics)

---

**Good luck with your deployment! 🎊**
