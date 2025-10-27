# 🎓 Railway Deployment - Student Quick Start

## 🚀 5-Step Deployment Process

### **Step 1: Sign Up for Railway (2 minutes)**

1. Go to **https://railway.app/**
2. Click **"Login with GitHub"**
3. Authorize Railway
4. In Railway dashboard, go to **Account Settings** → **Billing**
5. Click **"Redeem GitHub Student Developer Pack"**
6. ✅ You now have **$5/month free credits!**

---

### **Step 2: Create MySQL Database (1 minute)**

1. In Railway dashboard, click **"New Project"**
2. Click **"Deploy"** → **"Provision MySQL"**
3. Railway creates database instantly
4. Click on the MySQL service card
5. Go to **"Variables"** tab
6. **Copy these values** (you'll need them):
   ```
   MYSQLHOST
   MYSQLPORT
   MYSQLUSER
   MYSQLPASSWORD
   MYSQLDATABASE
   ```

---

### **Step 3: Import Database Schema (3 minutes)**

#### Option A: Using Railway's Web Interface (Easier)
1. In MySQL service, click **"Data"** tab
2. Click **"Query"** button
3. Copy contents of `database/schema/updated_schema.sql`
4. Paste and click **"Run Query"**

#### Option B: Using MySQL Workbench or Command Line
1. In MySQL service, find **"Connection String"** in Variables tab
2. Use MySQL Workbench or command line:
   ```bash
   mysql -h [MYSQLHOST] -P [MYSQLPORT] -u [MYSQLUSER] -p[MYSQLPASSWORD] [MYSQLDATABASE]
   ```
3. Then run:
   ```sql
   SOURCE database/schema/updated_schema.sql;
   ```

---

### **Step 4: Deploy Backend (5 minutes)**

1. In Railway project, click **"New"** → **"GitHub Repo"**
2. Select: **codeDulan/photo-frame-web**
3. Railway detects it's a monorepo

#### Configure Backend Service:
4. Click on the new service card
5. Click **"Settings"** tab:
   - **Service Name**: `photoframe-backend`
   - **Root Directory**: `backend`
   - **Watch Paths**: `backend/**`

6. Click **"Variables"** tab → **"Raw Editor"** and paste:
   ```
   PORT=${{RAILWAY_PUBLIC_PORT}}
   DB_HOST=${{MySQL.MYSQLHOST}}
   DB_PORT=${{MySQL.MYSQLPORT}}
   DB_USER=${{MySQL.MYSQLUSER}}
   DB_PASSWORD=${{MySQL.MYSQLPASSWORD}}
   DB_NAME=${{MySQL.MYSQLDATABASE}}
   NODE_ENV=production
   ```

7. Click **"Settings"** → **"Networking"** → **"Generate Domain"**
8. **Copy the domain** (e.g., `photoframe-backend-production.up.railway.app`)
9. Keep this - you'll need it for frontend!

---

### **Step 5: Deploy Frontend (5 minutes)**

1. In Railway project, click **"New"** → **"GitHub Repo"**
2. Select: **codeDulan/photo-frame-web** (same repo)

#### Configure Frontend Service:
3. Click on the new service card
4. Click **"Settings"** tab:
   - **Service Name**: `photoframe-frontend`
   - **Root Directory**: `frontend`
   - **Watch Paths**: `frontend/**`

5. Click **"Variables"** tab → **"Raw Editor"** and paste:
   ```
   PORT=${{RAILWAY_PUBLIC_PORT}}
   VITE_API_URL=https://[YOUR-BACKEND-DOMAIN]/api
   VITE_WHATSAPP_NUMBER=+94XXXXXXXXXX
   ```
   **Replace:**
   - `[YOUR-BACKEND-DOMAIN]` with the domain from Step 4
   - `+94XXXXXXXXXX` with your actual WhatsApp number

6. Click **"Settings"** → **"Networking"** → **"Generate Domain"**
7. **Copy this domain** - this is your website URL!

8. **Update Backend CORS**: Go back to backend service → Variables → Add:
   ```
   FRONTEND_URL=https://[YOUR-FRONTEND-DOMAIN]
   ```

9. Both services will redeploy automatically

---

## ✅ Verify Deployment

### Test Backend:
Visit: `https://your-backend-domain.railway.app/api/health`

Should see:
```json
{
  "success": true,
  "message": "API server is running",
  "timestamp": "2025-10-26T..."
}
```

### Test Frontend:
Visit: `https://your-frontend-domain.railway.app`

You should see your Photo Frame website! 🎉

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

## 🔧 Common Issues & Solutions

### ❌ Backend: Database connection error
**Solution:**
- Check Variables tab - make sure all `${{MySQL.*}}` variables are linked
- Click **"Settings"** → **"Service Variables"** → **"Reference Variables"**
- Ensure MySQL service is selected

### ❌ Frontend: Can't reach backend API
**Solution:**
- Check `VITE_API_URL` in frontend variables
- Make sure it includes `/api` at the end
- Check CORS: backend needs `FRONTEND_URL` variable set

### ❌ Build fails
**Solution:**
- Click failed deployment → **"View Logs"**
- Check for missing dependencies or build errors
- Verify `Root Directory` is set correctly (`backend` or `frontend`)

### ❌ Port binding error
**Solution:**
- Railway auto-assigns ports
- Make sure backend uses `process.env.PORT`
- Variables should have `PORT=${{RAILWAY_PUBLIC_PORT}}`

---

## 💡 Tips for Students

1. **Free Tier Limits**:
   - $5/month = ~500MB RAM + 5GB storage
   - Perfect for development/portfolio projects
   - Enough for moderate traffic

2. **Save Credits**:
   - Railway sleeps inactive services (free tier)
   - First request might be slow - this is normal
   - Consider upgrading for production use

3. **Database Backups**:
   - Railway auto-backs up daily (free tier)
   - Export manually: MySQL service → **"Data"** → **"Export"**
   - Save to GitHub or Google Drive

4. **Environment Variables**:
   - Never commit `.env` files!
   - Always use Railway's Variables tab
   - Update `.env.example` files for documentation

5. **Custom Domain** (Optional):
   - Railway domains work great: `*.railway.app`
   - Want custom domain? Go to **Settings** → **"Domains"**
   - SSL is automatic!

---

## 🎯 What's Next?

After successful deployment:

✅ **Test All Features**: Create orders, check database
✅ **Monitor Performance**: Check Railway metrics
✅ **Set Up Alerts**: Railway can email you on failures
✅ **Add to Portfolio**: Share your live project URL!
✅ **Keep Learning**: Check Railway docs for advanced features

---

## 📞 Need Help?

- **Railway Docs**: https://docs.railway.app/
- **Railway Discord**: https://discord.gg/railway (Very active community!)
- **GitHub Issues**: Open issue in your repo
- **Railway Status**: https://status.railway.app/

---

## 🎉 You're Done!

Your Photo Frame web app is now live on the internet! 🚀

**Share your deployed app**:
- Add to your GitHub README
- Share on LinkedIn/Portfolio
- Show to potential employers

**Remember**: This is a real production deployment with:
- ✅ Auto-scaling
- ✅ SSL/HTTPS
- ✅ Auto-deployment from GitHub
- ✅ Database backups
- ✅ Monitoring & logs

Great job! 🎓
