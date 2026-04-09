# SkillGap AI Coach - Development Guide

## Quick Start

### Prerequisites
- Node.js 16+ 
- npm or yarn
- Groq API Key (get free at https://console.groq.com)

### Installation

```bash
# Clone or navigate to the project
cd AI\ model\ hackacthon

# Backend Setup
cd server
npm install
cp .env.example .env
# Edit .env and add your GROQ_API_KEY
npm run dev

# Frontend Setup (new terminal)
cd client
npm install
npm run dev
```

Then open: **http://localhost:5173**

---

## Project Architecture

```
AI model hackacthon/
├── server/
│   ├── index.js              # Express app setup + middleware
│   ├── controllers/
│   │   └── analyzeController.js  # Skill gap analysis + AI roadmap
│   ├── routes/
│   │   └── analyze.js        # API endpoint definition
│   ├── skillData.js          # Career goals & required skills
│   ├── package.json
│   └── .env                  # API keys (KEEP PRIVATE!)
│
└── client/
    ├── src/
    │   ├── App.jsx           # Main component
    │   ├── components/
    │   │   ├── SkillForm.jsx      # Input form + history
    │   │   └── RoadmapResult.jsx  # Results + export
    │   ├── main.jsx
    │   └── index.css
    ├── index.html
    ├── vite.config.js
    └── package.json
```

---

## API Reference

### POST /analyze

**Request:**
```json
{
  "name": "Alex",
  "skills": ["HTML", "CSS", "JavaScript"],
  "goal": "Frontend Developer"
}
```

**Success Response (200):**
```json
{
  "name": "Alex",
  "goal": "Frontend Developer",
  "userSkills": ["HTML", "CSS", "JavaScript"],
  "missingSkills": ["React"],
  "roadmap": "Week 1: ... (full 4-week roadmap)"
}
```

**Error Responses:**
- **400**: Invalid input (missing fields, unknown goal)
- **429**: Rate limit exceeded (try again in a moment)
- **500**: Server error or invalid Groq API key
- **503**: API temporarily unavailable

---

## Key Features

### 1. Skill Gap Analysis
- Compares user skills against career path requirements
- Case-insensitive matching
- Returns missing skills

### 2. AI Roadmap Generation
- Uses Groq Llama 3.1 (70B model)
- 4-week structured learning plan
- Daily tasks, mini projects, free resources
- Automatically retries on failure

### 3. Progress Tracking
- Interactive checkboxes for each missing skill
- Visual progress bar
- Persistent progress within session

### 4. Data Persistence
- Last 5 analyses saved in localStorage
- Export roadmap as `.txt` file
- Quick reload of previous results

---

## Environment Variables

### Server (.env)
```
GROQ_API_KEY=gsk_your_key_here    # Required: Get from https://console.groq.com
PORT=5000                          # Optional: Server port
CLIENT_URL=http://localhost:5173   # Optional: CORS origin
```

### Client (Vite)
```
VITE_API_URL=http://localhost:5000  # Optional: Backend URL
```

---

## Supported Career Paths

| Goal | Required Skills |
|------|----------------|
| Frontend Developer | HTML, CSS, JavaScript, React |
| Backend Developer | Node.js, Express, MongoDB |
| Full Stack Developer | HTML, CSS, JavaScript, React, Node.js, MongoDB |
| Data Analyst | Python, SQL, Excel, Power BI |

---

## Development Commands

### Backend
```bash
npm run dev       # Development with nodemon
npm start         # Production
```

### Frontend
```bash
npm run dev       # Development server
npm run build     # Production build
npm run preview   # Preview production build
npm run lint      # ESLint check
```

---

## Error Handling

### Server-side
1. **Validation**: Input checks (name, skills array)
2. **Retry Logic**: 3 attempts with exponential backoff
3. **Error Messages**: Specific feedback for each error type
4. **Graceful Degradation**: Handles API timeouts gracefully

### Client-side
1. **Form Validation**: Non-empty checks, length limits
2. **Error Display**: User-friendly error messages
3. **Loading States**: Clear feedback during processing
4. **Timeout**: 30-second request timeout

---

## Troubleshooting

### "GROQ_API_KEY is not set"
```bash
# Make sure .env exists in server/
cat server/.env  # Check if GROQ_API_KEY is there
```

### CORS Error
```bash
# Ensure CLIENT_URL matches your frontend URL
# Default: http://localhost:5173
```

### Port Already in Use
```bash
# Find process on port 5000
lsof -i :5000

# Kill process
kill -9 <PID>
```

### API Rate Limited (429)
- The retry mechanism will handle this automatically
- Wait 60+ seconds before trying again
- Check Groq console for usage

---

## Contributing

To add new career paths:

1. **Edit** `server/skillData.js`:
```javascript
const REQUIRED_SKILLS = {
  "Your New Path": ["Skill1", "Skill2", "Skill3"],
  // ...
};
```

2. **Update** client form in `SkillForm.jsx`:
```javascript
const GOALS = [
  // ... existing goals
  "Your New Path",
];
```

3. **Rebuild** and test

---

## Performance Tips

1. **Cache Bust**: Clear browser cache if styles don't update
2. **Network**: Use wired connection for faster API calls
3. **Storage**: Clear localStorage if memory becomes an issue
4. **Groq Limits**: Free tier has rate limits (~30 req/min)

---

## Security Notes

⚠️ **IMPORTANT**: 
- Never commit `.env` to version control
- `.gitignore` should include `.env`
- Rotate API keys regularly
- Don't expose API URL to untrusted clients

---

## License

Built for educational purposes during AI Model Hackathon.

