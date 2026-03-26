# 🚀 Quick Start - Deploy to Render in 15 Minutes

Fast-track guide to get your research agent live on Render.

---

## 📋 What You Need

1. **Groq API Key** - Get free key at https://console.groq.com/keys
2. **Render Account** - Sign up free at https://render.com
3. **GitHub Repo** - Your code pushed to GitHub

---

## 🎯 Deployment Steps

### STEP 1: Deploy Backend (5 minutes)

1. **Go to Render** → https://dashboard.render.com
2. **Click** "New +" → "Web Service"
3. **Connect** your GitHub repository
4. **Configure:**
   - Name: `research-agent-backend`
   - Root Directory: `research-agent/backend`
   - Build Command: `pip install -r requirements.txt`
   - Start Command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
5. **Add Environment Variables:**
   - `GROQ_API_KEY` = your Groq API key
   - `CORS_ORIGINS` = `http://localhost:5173`
6. **Click** "Create Web Service"
7. **Wait** for deployment (3-5 min)
8. **Copy** your backend URL (e.g., `https://xxx.onrender.com`)

### STEP 2: Deploy Frontend (5 minutes)

1. **Click** "New +" → "Static Site"
2. **Connect** same GitHub repository
3. **Configure:**
   - Name: `research-agent-frontend`
   - Root Directory: `research-agent/frontend`
   - Build Command: `npm install && npm run build`
   - Publish Directory: `dist`
4. **Add Environment Variable:**
   - `VITE_API_URL` = your backend URL from Step 1
5. **Click** "Create Static Site"
6. **Wait** for build (2-4 min)
7. **Copy** your frontend URL

### STEP 3: Update CORS (2 minutes)

1. **Go back** to backend service
2. **Click** "Environment" tab
3. **Edit** `CORS_ORIGINS` variable
4. **Update** to: `https://your-frontend-url.onrender.com,http://localhost:5173`
5. **Save** - backend will auto-redeploy

### STEP 4: Test (3 minutes)

1. **Open** your frontend URL
2. **Sign up** with test account
3. **Start research** on any topic
4. **Watch** it work! ✅

---

## 🎉 Done!

Your app is live at: `https://your-frontend.onrender.com`

---

## 📚 Need More Details?

- **Full Guide**: See `RENDER_DEPLOYMENT_GUIDE.md`
- **Local Testing**: See `LOCAL_SETUP.md`
- **Checklist**: See `DEPLOYMENT_CHECKLIST.md`

---

## 🐛 Quick Troubleshooting

**CORS Error?**
→ Make sure Step 3 is complete and backend redeployed

**Can't connect to backend?**
→ Check `VITE_API_URL` has correct backend URL (with https://)

**Backend not starting?**
→ Check logs in Render dashboard, verify Groq API key

**Build failed?**
→ Check build logs, verify all dependencies are listed

---

## 💡 Pro Tips

- Free tier has cold starts (first request takes 30-60s)
- Backend spins down after 15 min inactivity
- Users data resets on free tier (upgrade for persistent storage)
- Check Render logs if anything goes wrong

---

**Happy Deploying! 🚀**
