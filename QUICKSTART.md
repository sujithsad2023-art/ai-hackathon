# 🚀 Quick Start Guide

## Prerequisites
- **Node.js 16+** installed
- **npm** or **yarn** package manager
- **Groq API Key** (Free at https://console.groq.com)

---

## 1️⃣ Setup Backend (Server)

```bash
# Navigate to server directory
cd server

# Install dependencies
npm install

# Create/Edit .env file with your API key
# (File should already exist - just verify it has your key)
cat .env
# Expected content:
# GROQ_API_KEY=gsk_your_actual_key_here
# PORT=5000

# Start development server
npm run dev
```

✅ **Success**: You should see:
```
SkillGap AI Coach API running on http://localhost:5000
📋 Ensure your client connects to this server
```

---

## 2️⃣ Setup Frontend (Client)

**In a NEW terminal:**

```bash
# Navigate to client directory
cd client

# Install dependencies
npm install

# Start development server
npm run dev
```

✅ **Success**: You should see:
```
➜  Local:   http://localhost:5173/
```

---

## 3️⃣ Open the App

**Open your browser** and go to:
```
http://localhost:5173
```

🎉 **You should see the SkillGap AI Coach form!**

---

## 4️⃣ Try It Out

### Test Case 1: Get Recommended Roadmap
1. **Name**: `Alex`
2. **Skills**: `HTML, CSS`
3. **Goal**: `Frontend Developer`
4. Click **"Analyze My Skills →"**

✅ Expected: You'll see missing skills (JavaScript, React) and a 4-week roadmap

### Test Case 2: Export Roadmap
1. After getting results, click **"📥 Export"** button
2. A `.txt` file will download automatically
3. Open the file to view your roadmap

### Test Case 3: Load Recent Analysis
1. Click **"← Start Over"**
2. Notice the **"📚 Recent Analyses"** section with your previous analysis
3. Click on it to instantly reload your results

---

## 🔍 Troubleshooting

### ❌ "Failed to connect to server"
**Solution:**
- Make sure server is running on port 5000
- Check server terminal for errors
- Verify no firewall is blocking port 5000

### ❌ "Invalid Groq API key"
**Solution:**
- Edit `server/.env`
- Get a fresh API key from https://console.groq.com
- Ensure key starts with `gsk_`
- Restart server: `npm run dev`

### ❌ "Port 5000 already in use"
**Solution:**
```bash
# Find process using port 5000
lsof -i :5000

# Kill it (use PID from above)
kill -9 <PID>

# Or change port in .env
PORT=5001
```

### ❌ Website won't load at localhost:5173
**Solution:**
- Make sure frontend server is running
- Check frontend terminal for errors
- Try clearing browser cache: Ctrl+Shift+Delete

---

## 📚 Next Steps

After verifying the app works:

1. **Read** [SOLUTION_SUMMARY.md](SOLUTION_SUMMARY.md) - Overview of all improvements
2. **Learn** [IMPROVEMENTS.md](IMPROVEMENTS.md) - Details about new features
3. **Develop** [DEVELOPMENT_GUIDE.md](DEVELOPMENT_GUIDE.md) - For developers
4. **Test** [TESTING_GUIDE.md](TESTING_GUIDE.md) - Complete testing checklist

---

## 🎯 Key Features to Try

| Feature | How to Use |
|---------|-----------|
| 💾 **Save Progress** | Results auto-save in browser (localStorage) |
| 📥 **Export Roadmap** | Click "📥 Export" button on results page |
| 📚 **Load Recent** | Previous analyses appear as cards below form |
| ✅ **Track Progress** | Check off skills as you learn them |
| 📊 **See Progress Bar** | Visual indicator of learning progress |
| 🔄 **Start Over** | Reset form and start a new analysis |

---

## 📞 Commands Reference

### Backend
```bash
cd server
npm install          # Install dependencies
npm run dev          # Development mode (auto-reload)
npm start            # Production mode
```

### Frontend
```bash
cd client
npm install          # Install dependencies
npm run dev          # Development mode
npm run build        # Create production build
npm run preview      # Preview production build
npm run lint         # Check code style
```

---

## 🔐 Security Reminder

⚠️ **Important:**
- Never commit your `.env` file
- Never share your GROQ_API_KEY
- The `.gitignore` file prevents accidental commits

---

## 💡 Tips & Tricks

### Speed Up Development
```bash
# Run both servers in one terminal (macOS/Linux)
(cd server && npm run dev) & (cd client && npm run dev)
```

### Monitor API Calls
- Open browser DevTools (F12)
- Go to **Network** tab
- Watch requests to `localhost:5000/analyze`

### Debug Backend
- Check server terminal for logs
- Add `console.log()` in `analyzeController.js`
- Use `curl` to test API directly:
```bash
curl -X POST http://localhost:5000/analyze \
  -H "Content-Type: application/json" \
  -d '{"name":"Test","skills":["HTML"],"goal":"Frontend Developer"}'
```

---

## 🎓 Learning Path

If you want to understand how everything works:

1. **Frontend Flow**
   - User submits form → SkillForm.jsx
   - Calls API → SkillForm.jsx line ~45
   - Receives result → RoadmapResult.jsx
   - Saves to localStorage → SkillForm.jsx line ~30

2. **Backend Flow**
   - API receives request → server/index.js line 35
   - Validates input → analyzeController.js line 11
   - Identifies gaps → skillData.js line 10
   - Generates roadmap → analyzeController.js line 30
   - Retries if needed → analyzeController.js line 5

3. **Key Files to Read**
   - Backend logic: `server/controllers/analyzeController.js`
   - Career paths: `server/skillData.js`
   - Frontend logic: `client/src/components/SkillForm.jsx`
   - Results UI: `client/src/components/RoadmapResult.jsx`

---

## 📊 Expected Performance

- **Form Load Time**: < 2 seconds
- **API Response Time**: 4-8 seconds (Groq API)
- **Export Download**: < 1 second
- **History Load**: < 500ms

---

## ✨ What's New?

This improved version includes (compared to original):

✅ **Automatic Retry Logic** - Handles temporary failures  
✅ **Data Persistence** - Save your analyses  
✅ **Export Roadmap** - Download as file  
✅ **Recent Analyses Panel** - Quick reload previous results  
✅ **Better Error Messages** - More helpful feedback  
✅ **Enhanced Loading States** - Clear feedback during processing  
✅ **Request Logging** - Server tracks all requests  
✅ **Input Validation** - Better security & UX  

---

**Happy Learning! 🎓**

For detailed information, see [SOLUTION_SUMMARY.md](SOLUTION_SUMMARY.md)

