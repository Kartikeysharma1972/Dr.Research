# 📖 Step-by-Step Deployment Guide for Render

Complete walkthrough to deploy Dr.Research AI Agent on Render.

---

## 🎯 Overview

You have a research agent project with:
- **Backend**: FastAPI (Python) - AI research pipeline
- **Frontend**: React + Vite - User interface

We'll deploy both on Render's free tier.

---

## 📋 Before You Start

### 1. Get Your Groq API Key
1. Visit https://console.groq.com/keys
2. Sign up or log in
3. Click "Create API Key"
4. Copy and save the key (starts with `gsk_`)

### 2. Prepare Your GitHub Repository
1. Make sure all code is committed
2. Push to GitHub: `git push origin main`
3. Repository should be at: `https://github.com/Kartikeysharma1972/Dr.Research`

### 3. Create Render Account
1. Go to https://render.com
2. Click "Get Started"
3. Sign up with GitHub (recommended)
4. Authorize Render to access your repositories

---

## 🔧 PART 1: Deploy Backend (FastAPI)

### Step 1: Create Web Service

1. **Login to Render Dashboard**
   - Go to https://dashboard.render.com
   - You should see your dashboard

2. **Start New Service**
   - Click the blue "New +" button (top right)
   - Select "Web Service" from dropdown

3. **Connect Repository**
   - You'll see "Create a new Web Service"
   - If not connected: Click "Connect account" → Authorize GitHub
   - Find your repository in the list
   - Click "Connect" next to `Kartikeysharma1972/Dr.Research`

### Step 2: Configure Backend Settings

You'll see a configuration form. Fill it out:

**Name & Region:**
- **Name**: `research-agent-backend` (or any name you like)
- **Region**: Select closest to you (e.g., Oregon (US West), Frankfurt (EU))

**Branch:**
- **Branch**: `main` (or your default branch)

**Root Directory:**
- Click "Edit" next to Root Directory
- Enter: `research-agent/backend`
- This tells Render where your backend code is

**Environment:**
- **Runtime**: Should auto-detect "Python 3" ✅
- If not, select "Python 3" from dropdown

**Build & Start Commands:**
- **Build Command**: `pip install -r requirements.txt`
- **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`

**Instance Type:**
- **Plan**: Free (should be selected by default)

### Step 3: Add Environment Variables

Scroll down to "Environment Variables" section:

1. **Click "Add Environment Variable"**

2. **Add First Variable:**
   - Key: `GROQ_API_KEY`
   - Value: Paste your Groq API key (from prerequisites)
   - Click "Add"

3. **Add Second Variable:**
   - Click "Add Environment Variable" again
   - Key: `CORS_ORIGINS`
   - Value: `http://localhost:5173`
   - Click "Add"
   - (We'll update this later with frontend URL)

4. **Add Third Variable (Optional):**
   - Click "Add Environment Variable" again
   - Key: `PYTHON_VERSION`
   - Value: `3.11.0`
   - Click "Add"

### Step 4: Deploy Backend

1. **Review Settings**
   - Double-check all settings are correct
   - Especially: Root Directory, Build Command, Start Command

2. **Create Service**
   - Scroll to bottom
   - Click blue "Create Web Service" button

3. **Wait for Deployment**
   - You'll see deployment logs streaming
   - This takes 3-5 minutes
   - Watch for:
     - "Installing dependencies..."
     - "Starting service..."
     - "Your service is live 🎉"

4. **Verify Deployment**
   - Status should show "Live" with green dot ✅
   - If you see errors, check the logs

### Step 5: Get Backend URL

1. **Copy Your Backend URL**
   - At the top of the page, you'll see your service URL
   - Format: `https://research-agent-backend.onrender.com`
   - Click the copy icon or select and copy
   - **SAVE THIS URL** - paste it somewhere safe!

2. **Test Backend**
   - Open new browser tab
   - Visit: `https://your-backend-url.onrender.com/health`
   - Should see: `{"status":"ok","service":"research-agent-api"}`
   - If you see this, backend is working! ✅

---

## 🎨 PART 2: Deploy Frontend (React)

### Step 1: Create Static Site

1. **Go Back to Dashboard**
   - Click "Dashboard" in top left
   - Or visit https://dashboard.render.com

2. **Create New Static Site**
   - Click "New +" button
   - Select "Static Site"

3. **Connect Same Repository**
   - Find `Kartikeysharma1972/Dr.Research` in the list
   - Click "Connect"

### Step 2: Configure Frontend Settings

Fill out the configuration form:

**Name & Branch:**
- **Name**: `research-agent-frontend` (or any name)
- **Branch**: `main`

**Root Directory:**
- Click "Edit"
- Enter: `research-agent/frontend`
- This tells Render where your frontend code is

**Build Settings:**
- **Build Command**: `npm install && npm run build`
- **Publish Directory**: `dist`
- These should auto-fill, but verify they're correct

### Step 3: Add Environment Variable

Scroll to "Environment Variables":

1. **Click "Add Environment Variable"**

2. **Add Variable:**
   - Key: `VITE_API_URL`
   - Value: Paste your backend URL (from Part 1, Step 5)
   - Example: `https://research-agent-backend.onrender.com`
   - **IMPORTANT**: No trailing slash at the end!
   - **IMPORTANT**: Must be `https://` not `http://`

### Step 4: Deploy Frontend

1. **Review Settings**
   - Double-check Root Directory is `research-agent/frontend`
   - Verify `VITE_API_URL` has correct backend URL

2. **Create Static Site**
   - Scroll to bottom
   - Click blue "Create Static Site" button

3. **Wait for Build**
   - Build logs will stream
   - This takes 2-4 minutes
   - Watch for:
     - "Installing dependencies..."
     - "Building application..."
     - "Build successful!"
     - "Your site is live 🎉"

4. **Verify Deployment**
   - Status should show "Live" with green dot ✅

### Step 5: Get Frontend URL

1. **Copy Your Frontend URL**
   - At the top, you'll see your site URL
   - Format: `https://research-agent-frontend.onrender.com`
   - Click copy icon or select and copy
   - **SAVE THIS URL** - this is your live app!

2. **Test Frontend**
   - Click the URL to open in new tab
   - You should see your login/signup page
   - If you see the page, frontend is deployed! ✅

---

## 🔄 PART 3: Update CORS Configuration

Now we need to tell the backend to accept requests from the frontend.

### Step 1: Go Back to Backend Service

1. **Navigate to Backend**
   - Click "Dashboard" in top left
   - Find and click on `research-agent-backend` service

2. **Go to Environment Tab**
   - Click "Environment" in left sidebar
   - You'll see your environment variables

### Step 2: Update CORS_ORIGINS

1. **Find CORS_ORIGINS Variable**
   - Look for `CORS_ORIGINS` in the list
   - Current value: `http://localhost:5173`

2. **Edit Variable**
   - Click the pencil/edit icon next to `CORS_ORIGINS`
   - Update the value to include your frontend URL:
   ```
   https://research-agent-frontend.onrender.com,http://localhost:5173
   ```
   - Replace `research-agent-frontend.onrender.com` with YOUR actual frontend URL
   - **IMPORTANT**: Separate URLs with comma, no spaces
   - Keep `http://localhost:5173` for local development

3. **Save Changes**
   - Click "Save" or checkmark icon
   - Render will automatically redeploy the backend

### Step 3: Wait for Redeploy

1. **Monitor Redeployment**
   - Click "Events" tab to see deployment progress
   - Or watch the status indicator
   - Takes 1-2 minutes

2. **Verify Redeployment**
   - Status should return to "Live" ✅

---

## ✅ PART 4: Test Your Live Application

### Step 1: Open Your App

1. **Visit Frontend URL**
   - Open your frontend URL in browser
   - Example: `https://research-agent-frontend.onrender.com`

2. **Check Page Loads**
   - Should see login/signup modal
   - UI should look correct
   - No loading errors

### Step 2: Test Sign Up

1. **Click "Sign Up" Tab**
2. **Fill in Form:**
   - Name: `Test User`
   - Email: `test@example.com`
   - Password: `test123`
   - Confirm Password: `test123`
3. **Click "Create Account"**
4. **Verify Success:**
   - Should see "Account created! Please log in." message
   - Should auto-switch to login tab

### Step 3: Test Login

1. **Enter Credentials:**
   - Email: `test@example.com`
   - Password: `test123`
2. **Click "Sign In"**
3. **Verify Success:**
   - Modal should close
   - Should see main dashboard/research interface
   - Welcome message with your name

### Step 4: Test Research Pipeline

1. **Enter Research Topic:**
   - Type: `Benefits of renewable energy`
   - Or any topic you're interested in

2. **Start Research:**
   - Click "Start Research" button
   - Should see pipeline visualization appear

3. **Watch Progress:**
   - Pipeline should show nodes: Planner → Search → Extraction → Synthesis → Reflection → Refinement
   - Each node should progress through: pending → running → done
   - Real-time updates should appear
   - Sub-questions should appear after Planner completes

4. **Wait for Completion:**
   - Takes 1-3 minutes depending on topic
   - All nodes should complete
   - Final report should appear

5. **Verify Report:**
   - Report should have structured sections
   - Should include citations and sources
   - Should be well-formatted

### Step 5: Check for Errors

1. **Open Browser Console:**
   - Press `F12` on keyboard
   - Click "Console" tab

2. **Look for Errors:**
   - Should see no red error messages
   - Some info/log messages are normal
   - If you see CORS errors, go back to Part 3

3. **Test Error Handling:**
   - Try logging in with wrong password → Should show error
   - Try signing up with same email → Should show "Email already registered"
   - Try empty research topic → Should show validation error

---

## 🎉 Deployment Complete!

If all tests passed, your app is successfully deployed!

**Your Live URLs:**
- **Frontend (share this)**: `https://research-agent-frontend.onrender.com`
- **Backend API**: `https://research-agent-backend.onrender.com`
- **API Documentation**: `https://research-agent-backend.onrender.com/docs`

---

## 🐛 Troubleshooting Common Issues

### Issue 1: CORS Error in Browser Console

**Error Message:** `Access to fetch at 'https://...' has been blocked by CORS policy`

**Solution:**
1. Go to backend service in Render
2. Check `CORS_ORIGINS` includes your frontend URL
3. Make sure format is: `https://frontend-url.onrender.com,http://localhost:5173`
4. No spaces, separated by comma
5. Save and wait for redeploy

### Issue 2: Backend Not Starting

**Error Message:** Build fails or service won't start

**Solution:**
1. Check Render logs (Logs tab in backend service)
2. Look for error messages
3. Common causes:
   - Missing `GROQ_API_KEY` → Add in Environment tab
   - Wrong Python version → Add `PYTHON_VERSION=3.11.0`
   - Missing dependencies → Check requirements.txt

### Issue 3: Frontend Can't Connect to Backend

**Error Message:** Network errors, can't reach API

**Solution:**
1. Go to frontend service in Render
2. Check Environment variables
3. Verify `VITE_API_URL` is correct:
   - Should be `https://` not `http://`
   - Should be your backend URL
   - No trailing slash
4. Save and redeploy frontend

### Issue 4: White Screen / Blank Page

**Solution:**
1. Check browser console for errors (F12)
2. Check Render build logs
3. Verify build completed successfully
4. Check Publish Directory is `dist`
5. Try rebuilding: Manual Deploy → Deploy latest commit

### Issue 5: First Request Very Slow

**This is Normal on Free Tier!**
- Free tier services "spin down" after 15 minutes of inactivity
- First request after inactivity takes 30-60 seconds (cold start)
- Subsequent requests are fast
- Upgrade to paid plan to eliminate cold starts

### Issue 6: Users Not Persisting

**This is Expected on Free Tier!**
- Free tier has ephemeral storage
- `users.csv` file resets when service restarts
- Solutions:
  - Upgrade to paid plan with persistent disk ($7/month)
  - Or migrate to PostgreSQL database

---

## 📊 Monitoring Your App

### Check Service Health

1. **Render Dashboard**
   - Visit https://dashboard.render.com
   - See status of both services
   - Green = healthy, Red = issues

2. **View Logs**
   - Click on service → Logs tab
   - See real-time application logs
   - Useful for debugging

3. **Check Metrics**
   - Click on service → Metrics tab
   - See bandwidth, requests, response times

### Free Tier Limits

- **Bandwidth**: 100 GB/month
- **Build Minutes**: 500 minutes/month
- **Services**: Unlimited
- **Cold Starts**: After 15 min inactivity
- **Storage**: Ephemeral (resets on restart)

---

## 🚀 Next Steps

### Immediate
- [ ] Share your frontend URL with users
- [ ] Monitor for any errors in first 24 hours
- [ ] Test all features thoroughly

### Short Term
- [ ] Set up custom domain (optional)
- [ ] Add monitoring/alerts
- [ ] Implement analytics

### Long Term (Production)
- [ ] Upgrade to paid plan ($7/month) for:
  - Persistent storage
  - No cold starts
  - Better performance
- [ ] Migrate to PostgreSQL database
- [ ] Implement JWT authentication
- [ ] Add rate limiting
- [ ] Set up error tracking (Sentry)
- [ ] Add automated backups

---

## 📞 Getting Help

**Render Support:**
- Documentation: https://render.com/docs
- Community: https://community.render.com
- Status: https://status.render.com

**Check Logs First:**
- 90% of issues can be solved by reading logs
- Backend logs show Python errors
- Frontend build logs show npm/build errors

**Common Resources:**
- Render Python docs: https://render.com/docs/deploy-fastapi
- Render Static Site docs: https://render.com/docs/deploy-vite

---

## ✅ Deployment Success Checklist

- [x] Backend deployed and showing "Live"
- [x] Frontend deployed and showing "Live"
- [x] CORS configured correctly
- [x] Sign up works
- [x] Login works
- [x] Research pipeline completes
- [x] No console errors
- [x] All features tested
- [x] URLs saved and shared

---

**Congratulations! Your Dr.Research AI Agent is now live! 🎊**

Share your app: `https://research-agent-frontend.onrender.com`
