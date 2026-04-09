# SkillGap AI Coach

An AI-powered web app that identifies your skill gaps and generates a personalized 4-week learning roadmap using Groq AI.

---

## ✨ Features

- 🎯 **Skill Gap Analysis** - Compares your skills against career path requirements
- 🤖 **AI-Generated Roadmaps** - Uses Groq Llama 3.1 (70B) for personalized learning plans
- 📊 **Progress Tracking** - Interactive checkboxes for each missing skill
- 💾 **Data Persistence** - Save and reload your last 5 analyses
- 📥 **Export Roadmap** - Download your learning plan as a text file
- 🔄 **Smart Retry Logic** - Automatic retries with exponential backoff
- 🎨 **Responsive UI** - Works on mobile, tablet, and desktop

---

## 🚀 Quick Start

```
AI model hackacthon/
├── client/          # React + Vite + Tailwind frontend
└── server/          # Node.js + Express backend
```

---

## Setup & Run

### 1. Backend

```bash
cd server
```

Add your Groq API key to `.env`:
```
GROQ_API_KEY=gsk-your-key-here
PORT=5000
```

```bash
npm install
npm run dev       # development (nodemon)
# or
npm start         # production
```

Server runs at: http://localhost:5000

---

### 2. Frontend

```bash
cd client
npm install
npm run dev
```

App runs at: http://localhost:5173

---

## API

### POST /analyze

**Request:**
```json
{
  "name": "Alex",
  "skills": ["HTML", "CSS"],
  "goal": "Frontend Developer"
}
```

**Response:**
```json
{
  "name": "Alex",
  "goal": "Frontend Developer",
  "userSkills": ["HTML", "CSS"],
  "missingSkills": ["JavaScript", "React"],
  "roadmap": "Week 1: ..."
}
```

---

## 📚 Documentation

- **[IMPROVEMENTS.md](IMPROVEMENTS.md)** - Complete list of improvements & features added
- **[DEVELOPMENT_GUIDE.md](DEVELOPMENT_GUIDE.md)** - Full development & deployment guide
- **[TESTING_GUIDE.md](TESTING_GUIDE.md)** - Testing checklist & quality assurance

---

## Career Goals Supported

| Goal | Required Skills |
|------|----------------|
| Frontend Developer | HTML, CSS, JavaScript, React |
| Backend Developer | Node.js, Express, MongoDB |
| Full Stack Developer | HTML, CSS, JavaScript, React, Node.js, MongoDB |
| Data Analyst | Python, SQL, Excel, Power BI |
