# Deployment Guide - Route Compliance Manager

This guide will help you deploy the application to Railway (backend) and Vercel (frontend).

## Prerequisites

- GitHub account with repository: https://github.com/Neno254-pixel/truck-app
- Railway account (https://railway.app)
- Vercel account (https://vercel.com)
- Railway CLI installed (`npm install -g @railway/cli`)
- Vercel CLI installed (`npm install -g vercel`)

## Backend Deployment (Railway)

### Current Deployment

✅ **Backend is already deployed!**

- **URL**: https://track-app-production-9522.up.railway.app
- **Project**: track app
- **Status**: Running

### To Redeploy or Update

1. **Navigate to backend directory:**

   ```bash
   cd truck-log-app/backend
   ```

2. **Login to Railway:**

   ```bash
   railway login
   ```

3. **Deploy:**

   ```bash
   railway up
   ```

4. **View logs:**
   ```bash
   railway logs
   ```

### Environment Variables (Railway Dashboard)

Set these in Railway dashboard if needed:

- `DEBUG=False` (for production)
- `SECRET_KEY=your-secret-key-here`
- `ALLOWED_HOSTS=track-app-production-9522.up.railway.app`

## Frontend Deployment (Vercel)

### Step 1: Prepare Frontend

1. **Create environment file:**

   ```bash
   cd truck-log-app/frontend
   cp .env.example .env
   ```

2. **Update `.env` with Railway backend URL:**
   ```
   REACT_APP_API_URL=https://track-app-production-9522.up.railway.app
   ```

### Step 2: Deploy to Vercel

**Option A: Using Vercel CLI**

1. **Login to Vercel:**

   ```bash
   vercel login
   ```

2. **Deploy:**

   ```bash
   cd truck-log-app/frontend
   vercel
   ```

3. **Follow prompts:**

   - Set up and deploy: Yes
   - Which scope: Your account
   - Link to existing project: No
   - Project name: route-compliance-manager (or your choice)
   - Directory: ./
   - Override settings: No

4. **Set environment variable:**

   ```bash
   vercel env add REACT_APP_API_URL
   ```

   Enter: `https://track-app-production-9522.up.railway.app`

5. **Deploy to production:**
   ```bash
   vercel --prod
   ```

**Option B: Using Vercel Dashboard**

1. Go to https://vercel.com/new
2. Import your GitHub repository: `Neno254-pixel/truck-app`
3. Configure project:
   - **Framework Preset**: Create React App
   - **Root Directory**: `truck-log-app/frontend`
   - **Build Command**: `npm run build`
   - **Output Directory**: `build`
4. Add environment variable:
   - **Key**: `REACT_APP_API_URL`
   - **Value**: `https://track-app-production-9522.up.railway.app`
5. Click "Deploy"

## Update Frontend to Use Environment Variable

The frontend needs to be updated to use the environment variable instead of hardcoded localhost URL.

### Files to Update:

1. **frontend/src/App.js** - Change API URL to use environment variable
2. **frontend/src/components/TripForm.jsx** - Change API URL to use environment variable

## Post-Deployment

### Test the Deployment

1. **Test Backend:**

   ```bash
   curl https://track-app-production-9522.up.railway.app/api/
   ```

2. **Test Frontend:**
   - Visit your Vercel URL
   - Try creating a trip
   - Verify it connects to the Railway backend

### Update CORS Settings

If you encounter CORS errors, update `backend/trucklog/settings.py`:

```python
CORS_ALLOWED_ORIGINS = [
    "http://localhost:3000",
    "http://127.0.0.1:3000",
    "https://your-vercel-app.vercel.app",  # Add your Vercel URL
]
```

Then redeploy:

```bash
cd truck-log-app/backend
railway up
```

## Monitoring

### Railway

- View logs: `railway logs`
- Dashboard: https://railway.com/project/359b6376-f521-4561-8703-ec7b0e18a7d3

### Vercel

- Dashboard: https://vercel.com/dashboard
- View deployment logs in the Vercel dashboard

## Troubleshooting

### Backend Issues

- Check Railway logs: `railway logs`
- Verify environment variables in Railway dashboard
- Ensure database migrations ran successfully

### Frontend Issues

- Check Vercel deployment logs
- Verify `REACT_APP_API_URL` environment variable is set
- Check browser console for CORS errors

### CORS Errors

- Add your Vercel domain to `CORS_ALLOWED_ORIGINS` in `settings.py`
- Redeploy backend after changes

## Local Development

### Backend

```bash
cd truck-log-app/backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
python manage.py runserver
```

### Frontend

```bash
cd truck-log-app/frontend
npm install
npm start
```

Set `.env` to use localhost:

```
REACT_APP_API_URL=http://localhost:8000
```

## URLs

- **Backend (Railway)**: https://track-app-production-9522.up.railway.app
- **Frontend (Vercel)**: Will be provided after deployment
- **GitHub Repository**: https://github.com/Neno254-pixel/truck-app
- **Railway Project**: https://railway.com/project/359b6376-f521-4561-8703-ec7b0e18a7d3
