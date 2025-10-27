# 🚀 Deployment Guide - Photo Frame Web Application

## GitHub Education Pack Benefits for Hosting

With your **GitHub Student Developer Pack**, you get:

### 🎯 **Recommended: Railway** (Best for this project)
- ✅ **$5/month free credits** (enough for this app)
- ✅ Built-in MySQL database
- ✅ Auto-deploy from GitHub
- ✅ Easy environment variables setup
- ✅ Free SSL certificates
- ✅ No credit card required with Education Pack

### 🔄 **Alternative Options:**
- **Heroku**: $13/month free credits
- **DigitalOcean**: $200 credit for 1 year
- **Microsoft Azure**: $100 free credit

---

## 🎓 Step-by-Step Deployment with Railway

### **Phase 1: Prepare Your Project**

#### 1. Push Code to GitHub (Already Done ✅)
Your repository: `https://github.com/codeDulan/photo-frame-web.git`

#### 2. Create Railway Configuration Files

We need to add configuration files to tell Railway how to deploy your app.

---

### **Phase 2: Sign Up for Railway**

#### 1. Visit Railway
Go to: **https://railway.app/**

#### 2. Sign In with GitHub
- Click **"Login"** → **"Login with GitHub"**
- Authorize Railway to access your GitHub account

#### 3. Verify Student Benefits
- Go to **Account Settings** → **Billing**
- Click **"Apply for GitHub Student Developer Pack"**
- You'll get **$5/month credit** (renews monthly while student)

---

### **Phase 3: Deploy Database**

#### 1. Create New Project
- Click **"New Project"**
- Select **"Database - MySQL"**
- Railway will create a MySQL database instantly

#### 2. Save Database Credentials
- Click on the MySQL service
- Go to **"Variables"** tab
- You'll see:
  ```
  MYSQL_HOST
  MYSQL_PORT
  MYSQL_USER
  MYSQL_PASSWORD
  MYSQL_DATABASE
  DATABASE_URL
  ```
- Keep this tab open - you'll need these values

#### 3. Import Your Database Schema
- Click on MySQL service → **"Data"** tab
- Click **"Connect"** to get MySQL URL
- Use a MySQL client (MySQL Workbench or command line):
  ```bash
  mysql -h [MYSQL_HOST] -P [MYSQL_PORT] -u [MYSQL_USER] -p[MYSQL_PASSWORD]
  ```
- Then run:
  ```sql
  USE [MYSQL_DATABASE];
  SOURCE database/schema/updated_schema.sql;
  ```

---

### **Phase 4: Deploy Backend**

#### 1. Add Backend to Project
- In your Railway project, click **"New"** → **"GitHub Repo"**
- Select **"codeDulan/photo-frame-web"**
- Railway will detect it's a monorepo

#### 2. Configure Backend Service
- Click **"Settings"** tab
- **Root Directory**: Set to `backend`
- **Build Command**: `npm install`
- **Start Command**: `npm start`
- **Port**: Railway auto-detects (uses 3001 from your code)
- **Watch Paths**: `backend/**`

#### 3. Set Environment Variables
- Click **"Variables"** tab
- Click **"+ New Variable"** for each or use **"Raw Editor"** and paste:
  ```
  DB_HOST = "${{MySQL.MYSQLHOST}}"
  DB_PORT = "${{MySQL.MYSQLPORT}}"
  DB_USER = "${{MySQL.MYSQLUSER}}"
  DB_PASSWORD = "${{MySQL.MYSQLPASSWORD}}"
  DB_NAME = "${{MySQL.MYSQLDATABASE}}"
  FRONTEND_URL = "https://[YOUR_FRONTEND_DOMAIN]"
  NODE_ENV = "production"
  ```
- Railway's `${{...}}` syntax auto-links to your MySQL service!

#### 4. Deploy
- Railway automatically deploys on push
- Click **"Deployments"** to see progress
- Once deployed, click **"Settings"** → **"Generate Domain"** to get public URL
- Copy this URL (e.g., `https://your-backend.railway.app`)

---

### **Phase 5: Deploy Frontend**

#### 1. Add Frontend to Project
- In Railway project, click **"New"** → **"GitHub Repo"**
- Select same repo: **"codeDulan/photo-frame-web"**

#### 2. Configure Frontend Service
- Click **"Settings"** tab
- **Service Name**: `photoframe-frontend`
- **Root Directory**: Set to `frontend`
- **Build Command**: `npm install && npm run build`
- **Start Command**: Leave empty (static site)
- **Watch Paths**: `frontend/**`
- **Install Command**: `npm install`

#### 3. Set Environment Variables
- Click **"Variables"** tab and add as previously:
  ```
  BACKEND_DOMAIN = "your-backend.railway.app"
  VITE_API_URL = "https://${{BACKEND_DOMAIN}}$/api"
  VITE_WHATSAPP_NUMBER = "+94XXXXXXXXXX"
  ```
- Replace `your-backend.railway.app` with your actual backend URL from Phase 4

#### 4. Configure Static Site Serving
Railway needs to serve your built Vite app. We'll add a simple server.

#### 5. Deploy
- Railway builds and deploys automatically
- Generate domain: **"Settings"** → **"Generate Domain"**
- Your app will be live at: `https://your-app.railway.app`

**Update Backend CORS**: Go back to backend service → Variables → Update `FRONTEND_URL`:
   ```
   FRONTEND_URL=https://your-app.railway.app
   ```
---

### **Phase 6: Testing Your Deployment**

#### 1. Test Backend API
Visit: `https://your-backend.railway.app/api/health`

Should return:
```json
{
  "status": "healthy",
  "database": "connected",
  "timestamp": "..."
}
```

#### 2. Test Frontend
Visit: `https://your-app.railway.app`

#### 3. Test Full Flow
- Try creating an order
- Check if data saves to database
- Test WhatsApp integration

---

## 📊 Monitor Your App

### View Logs:
1. Click on any service card
2. Click **"Deployments"** tab
3. Click on latest deployment
4. Click **"View Logs"** to see real-time logs

### Check Metrics:
1. Click on service card
2. Click **"Metrics"** tab
3. See CPU, Memory, Network usage

### Monitor Credits:
1. Go to **Account Settings** → **Billing**
2. Check usage against your $5/month credit

---

## 🔄 Auto-Deployment

Railway automatically watches your GitHub repo:

- **Push to main branch** → Auto-deploys to production
- **Create pull request** → Creates preview environment
- **Merge PR** → Deploys to production

To trigger a manual redeploy:
1. Click service card → **"Deployments"**
2. Click **⋯** on latest deployment
3. Click **"Redeploy"**

---

## 🔧 Configuration Files Needed

### 1. Backend: Add Health Check Endpoint

Already exists in `backend/src/server.js` ✅

### 2. Frontend: Create Static Server for Production

Railway needs a way to serve your built Vite app. We'll create a simple Express server.

---

## 📝 Quick Setup Checklist

Before deploying, let's create the necessary files:

- [ ] Add `nixpacks.toml` for Railway (both frontend and backend)
- [ ] Update CORS settings in backend to allow Railway frontend domain
- [ ] Add production build script for frontend serving
- [ ] Update environment variable examples

---

## 🌐 Custom Domain (Optional)

Once deployed on Railway:

1. Go to your frontend service → **"Settings"** → **"Domains"**
2. Click **"Custom Domain"**
3. Add your domain (if you have one)
4. Update DNS records as instructed
5. Railway handles SSL automatically!

---

## 💰 Cost Estimation

With GitHub Education Pack on Railway:
- **Free Tier**: $5/month credit
- **Typical Usage**: 
  - MySQL database: ~$1-2/month
  - Backend service: ~$1-2/month
  - Frontend service: ~$0.50-1/month
- **Total**: ~$3-5/month (covered by education credits!)

---

## 🔄 Auto-Deployment

Railway watches your GitHub repository:
- **Any push to main branch** → Auto-deploys
- **Pull request created** → Creates preview deployment
- **Merge PR** → Deploys to production

---

## 📊 Monitoring

Railway provides:
- **Metrics**: CPU, Memory, Network usage
- **Logs**: Real-time application logs
- **Alerts**: Email notifications for issues
- **Crash Detection**: Auto-restarts if app crashes

---

## 🆘 Troubleshooting

### Backend won't connect to database
- Check environment variables are set correctly
- Verify database credentials in MySQL service
- Check logs: Railway dashboard → Backend service → "Logs"

### Frontend can't reach backend
- Verify `VITE_API_URL` points to correct backend URL
- Check CORS settings allow frontend domain
- Check backend logs for errors

### Build fails
- Check build logs in Railway
- Verify `package.json` scripts are correct
- Ensure Node.js version compatibility

### Database connection timeout
- Railway MySQL might be sleeping (free tier)
- First request might be slow - this is normal
- Consider upgrading if needed

---

## 🎯 Next Steps

After successful deployment:

1. **Test thoroughly** - Try all features
2. **Set up monitoring** - Enable Railway alerts
3. **Configure backups** - Railway auto-backs up MySQL daily
4. **Add custom domain** - Make it professional
5. **Monitor usage** - Check credits don't run out

---

## 📚 Useful Links

- **Railway Docs**: https://docs.railway.app/
- **Railway Discord**: https://discord.gg/railway (Great community support!)
- **GitHub Education Pack**: https://education.github.com/pack
- **Railway Templates**: https://railway.app/templates

---

## 🚨 Important Notes

1. **Keep `.env` files in `.gitignore`** - Never commit secrets!
2. **Use environment variables** - Railway injects them at runtime
3. **Monitor your credits** - Check Railway dashboard monthly
4. **Database backups** - Railway auto-backs up, but export manually too
5. **Update dependencies** - Keep packages up to date for security

---

## ✅ Ready to Deploy?

Let's create the necessary configuration files and deploy your app! 🚀
