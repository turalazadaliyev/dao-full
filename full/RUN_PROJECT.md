# DonCoin Project - How to Run

## 🚀 Quick Start (5 Minutes)

### **Option 1: Run Everything Locally**

#### **Step 1: Open Two Terminals**

**Terminal 1 - Backend:**
```powershell
cd c:\Users\asusr\Downloads\full\backend\doncoin_backend
```

**Terminal 2 - Frontend:**
```powershell
cd c:\Users\asusr\Downloads\full\frontend\Doncoin2
```

---

### **Step 2: Start Backend**

In **Terminal 1**, run:

```powershell
# Activate virtual environment
.\.venv\Scripts\Activate.ps1

# Run migrations (first time only)
python manage.py migrate

# Start development server
python manage.py runserver
```

**Expected Output:**
```
Starting development server at http://127.0.0.1:8000/
```

---

### **Step 3: Start Frontend**

In **Terminal 2**, run:

```powershell
# Install dependencies (first time only)
npm install --legacy-peer-deps

# Start development server
npm run dev
```

**Expected Output:**
```
> ready - started server on 0.0.0.0:3000, url: http://localhost:3000
```

---

### **Step 4: Access the Application**

- **Frontend:** Open http://localhost:3000 in your browser
- **Backend API:** http://127.0.0.1:8000/api/
- **Admin Panel:** http://127.0.0.1:8000/admin/

---

## 📝 Detailed Setup Instructions

### **Backend Setup (Django)**

#### **Prerequisites**
- Python 3.9+ installed
- pip package manager

#### **Full Setup from Scratch**

```powershell
# Navigate to backend
cd c:\Users\asusr\Downloads\full\backend\doncoin_backend

# Create virtual environment (if not exists)
python -m venv .venv

# Activate virtual environment
.\.venv\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt

# Create .env file from template (IMPORTANT!)
Copy-Item .env.example .env

# Apply database migrations
python manage.py migrate

# Create superuser (for admin panel) - OPTIONAL
python manage.py createsuperuser

# Run development server
python manage.py runserver
```

#### **Environment Variables (.env file)**

Create `.env` file in `doncoin_backend/`:

```env
# Django Settings
DJANGO_SECRET_KEY=your-secret-key-here-change-in-production
DJANGO_DEBUG=True
DJANGO_ALLOWED_HOSTS=localhost,127.0.0.1

# JWT Configuration
SIMPLE_JWT_ROTATE=True

# Rate Limiting
DRF_ANON_RATE=20/hour
DRF_USER_RATE=100/hour

# CORS Configuration
CORS_ALLOWED_ORIGINS=http://localhost:3000,http://127.0.0.1:3000

# Database (optional - uses SQLite by default)
# DATABASE_URL=postgresql://user:password@localhost/doncoin
```

#### **Common Backend Commands**

```powershell
# Run server on different port
python manage.py runserver 8001

# Run tests
python manage.py test

# Make migrations after model changes
python manage.py makemigrations

# Apply migrations
python manage.py migrate

# Reset database completely
python manage.py migrate base zero
python manage.py migrate

# Create admin user
python manage.py createsuperuser

# Shell/REPL
python manage.py shell

# Collect static files (production)
python manage.py collectstatic
```

---

### **Frontend Setup (Next.js)**

#### **Prerequisites**
- Node.js 18+ and npm installed
- Check: `node --version` and `npm --version`

#### **Full Setup from Scratch**

```powershell
# Navigate to frontend
cd c:\Users\asusr\Downloads\full\frontend\Doncoin2

# Install dependencies
npm install --legacy-peer-deps

# Create .env.local file
Copy-Item .env.example .env.local

# Start development server
npm run dev

# Or with custom port
npm run dev -- -p 3001
```

#### **Environment Variables (.env.local)**

Create `.env.local` file in `Doncoin2/`:

```env
# Backend API
NEXT_PUBLIC_BACKEND_URL=http://127.0.0.1:8000

# Request timeout (milliseconds)
NEXT_PUBLIC_REQUEST_TIMEOUT=30000

# Security settings
NEXT_PUBLIC_ENABLE_CSP=true
NEXT_PUBLIC_SECURE_COOKIES=false

# Node environment
NODE_ENV=development
```

#### **Common Frontend Commands**

```powershell
# Start development server
npm run dev

# Build for production
npm run build

# Start production server
npm start

# Run linter
npm run lint

# Check for vulnerabilities
npm audit

# Fix vulnerabilities
npm audit fix

# View installed packages
npm list

# Update packages
npm update
```

---

## 🧪 Testing the Application

### **Test Backend API**

```powershell
# 1. Test registration
curl -X POST http://127.0.0.1:8000/api/users/register/ `
  -H "Content-Type: application/json" `
  -d '{
    "email": "test@example.com",
    "password": "TestPassword123!",
    "first_name": "Test",
    "last_name": "User"
  }'

# 2. Test login
curl -X POST http://127.0.0.1:8000/api/users/login/ `
  -H "Content-Type: application/json" `
  -d '{
    "email": "test@example.com",
    "password": "TestPassword123!"
  }'

# 3. Test protected endpoint (replace TOKEN with access token)
curl -X GET http://127.0.0.1:8000/api/users/profile/ `
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### **Test Frontend**

1. Open http://localhost:3000
2. Navigate to **Login** page
3. Click **Register** and create account
4. Login with credentials
5. View dashboard and features

---

## 🔐 Security Features (Implemented in Sprint 3)

### **Rate Limiting** ✅
- Anonymous users: 20 requests/hour
- Authenticated users: 100 requests/hour

### **Brute-Force Protection** ✅
- Max 5 failed login attempts
- 1-hour lockout after 5 failures

### **Token Security** ✅
- Access token: 1 hour lifetime
- Refresh token: 7 days lifetime
- Automatic token rotation enabled

### **Input Validation** ✅
- Frontend: DOMPurify sanitization
- Backend: Django serializer validation

### **CSRF Protection** ✅
- CSRF tokens on forms
- Token validation on state-changing requests

---

## 📊 Database Management

### **SQLite (Development)**

Database file: `doncoin_backend/db.sqlite3`

```powershell
# Clear database
Remove-Item db.sqlite3

# Recreate with migrations
python manage.py migrate
```

### **PostgreSQL (Production)**

Update `.env`:
```env
DATABASE_URL=postgresql://user:password@localhost:5432/doncoin
```

Then:
```powershell
pip install psycopg2-binary
python manage.py migrate
```

---

## 🐛 Troubleshooting

### **Backend Issues**

| Issue | Solution |
|-------|----------|
| **Port 8000 already in use** | `python manage.py runserver 8001` |
| **ModuleNotFoundError** | Activate venv: `.\.venv\Scripts\Activate.ps1` |
| **Database error** | Run: `python manage.py migrate` |
| **Import errors** | Run: `pip install -r requirements.txt` |
| **Permission denied (.venv)** | Set execution policy: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser` |

### **Frontend Issues**

| Issue | Solution |
|-------|----------|
| **Port 3000 already in use** | `npm run dev -- -p 3001` |
| **Module not found** | Run: `npm install --legacy-peer-deps` |
| **CORS errors** | Check `NEXT_PUBLIC_BACKEND_URL` in `.env.local` |
| **Blank page** | Check browser console (F12) for errors |
| **npm install fails** | Use: `npm install --legacy-peer-deps` |

### **Connection Issues**

| Issue | Solution |
|-------|----------|
| **Frontend can't reach backend** | 1. Check backend is running (port 8000) 2. Verify `NEXT_PUBLIC_BACKEND_URL` 3. Check CORS in backend settings |
| **Login fails** | 1. Verify user exists in database 2. Check backend logs 3. Verify credentials |
| **API returns 401** | Token expired - login again or check JWT settings |

---

## 📱 Access Points

### **Development Servers**

| Service | URL | Purpose |
|---------|-----|---------|
| **Frontend** | http://localhost:3000 | Main application |
| **Backend API** | http://127.0.0.1:8000/api/ | API endpoints |
| **Django Admin** | http://127.0.0.1:8000/admin/ | Database management |
| **API Docs** | http://127.0.0.1:8000/api/schema/ | API documentation |

### **Default Admin Credentials**

After running `python manage.py createsuperuser`:
- URL: http://127.0.0.1:8000/admin/
- Username: admin
- Password: (whatever you set)

---

## 🚀 Production Deployment

See: `DEPLOYMENT.md` in backend folder for:
- Nginx configuration
- Gunicorn setup
- PostgreSQL database
- SSL/HTTPS setup
- Environment configuration
- Monitoring setup

---

## 📂 Project Structure

```
full/
├── backend/
│   └── doncoin_backend/
│       ├── doncoin/
│       │   ├── config/          # Django settings
│       │   ├── base/            # Main app (models, views)
│       │   ├── api/             # API endpoints
│       │   └── manage.py
│       ├── requirements.txt
│       ├── .env.example
│       └── db.sqlite3
│
├── frontend/
│   └── Doncoin2/
│       ├── src/
│       │   ├── app/             # Next.js pages
│       │   ├── components/      # React components
│       │   └── lib/             # Utilities (api.js, auth.js)
│       ├── package.json
│       ├── .env.example
│       └── next.config.mjs
│
└── Documentation files
    ├── RUN_PROJECT.md           # This file
    ├── SPRINT3_COMPLETION.txt
    ├── SPRINT1_SPRINT2_ASSESSMENT.md
    └── Other guides
```

---

## ✅ Quick Health Check

After running both servers:

```powershell
# Check backend
curl http://127.0.0.1:8000/api/

# Check frontend (in browser)
# http://localhost:3000

# Check if login page loads
# http://localhost:3000/login
```

Both should respond without errors ✅

---

## 📚 Documentation Files

For more detailed information, see:

| Document | Purpose |
|----------|---------|
| **backend/README.md** | Backend documentation index |
| **backend/QUICKSTART.md** | Backend quick reference |
| **backend/SECURITY.md** | Security implementation details |
| **backend/DEPLOYMENT.md** | Production deployment guide |
| **frontend/SPRINT3_FRONTEND_SECURITY.md** | Frontend security guide |
| **SPRINT3_COMPLETION.txt** | Overall project completion report |
| **SPRINT1_SPRINT2_ASSESSMENT.md** | Feature completeness analysis |

---

## 🎯 Next Steps After Running

1. **Test the application** - Navigate through pages, try login/register
2. **Check console errors** - F12 in browser, check for API errors
3. **Review documentation** - Read SECURITY.md and deployment guide
4. **Configure for production** - See DEPLOYMENT.md when ready to launch

---

**Last Updated:** December 2, 2025  
**Status:** ✅ Ready to run  
**All Systems:** Fully documented and operational
