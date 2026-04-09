# Testing Guide - SkillGap AI Coach

## Manual Testing Checklist

### 1. Backend Tests

#### Startup Tests
- [ ] Server starts without errors
- [ ] `.env` validation works (error if GROQ_API_KEY missing)
- [ ] Logs show: "SkillGap AI Coach API running on http://localhost:5000"
- [ ] Health check: `curl http://localhost:5000` returns status message

#### API Endpoint Tests

**Test 1: Valid Request**
```bash
curl -X POST http://localhost:5000/analyze \
  -H "Content-Type: application/json" \
  -d '{"name":"John","skills":["HTML","CSS"],"goal":"Frontend Developer"}'
```
Expected: 200 with roadmap

**Test 2: Missing Field**
```bash
curl -X POST http://localhost:5000/analyze \
  -H "Content-Type: application/json" \
  -d '{"name":"John","goal":"Frontend Developer"}'
```
Expected: 400 with error message

**Test 3: Invalid Goal**
```bash
curl -X POST http://localhost:5000/analyze \
  -H "Content-Type: application/json" \
  -d '{"name":"John","skills":["HTML"],"goal":"Invalid Job"}'
```
Expected: 400 with "Unknown goal" error

**Test 4: Empty Skills Array**
```bash
curl -X POST http://localhost:5000/analyze \
  -H "Content-Type: application/json" \
  -d '{"name":"John","skills":[],"goal":"Frontend Developer"}'
```
Expected: 400 with error message

#### Retry Logic Tests
(Note: These require Groq API to be throttled/unavailable)
- [ ] API automatically retries on failure
- [ ] Exponential backoff works (1s, 2s, 3s waits)
- [ ] Success message appears after retry
- [ ] Error shown after 3 failed retries

---

### 2. Frontend Tests

#### Form Input Tests
- [ ] Name field accepts input
- [ ] Skills field accepts comma-separated values
- [ ] Goal dropdown shows all 4 options
- [ ] "Analyze My Skills" button is clickable

#### Validation Tests
- [ ] Error shown when submitting empty name: "❌ Please enter your name."
- [ ] Error shown when submitting no skills: "❌ Please enter at least one skill."
- [ ] Form clears errors when valid input is entered
- [ ] Skills with spaces are trimmed correctly

#### API Communication Tests
- [ ] Loading spinner appears during request
- [ ] Spinner shows "Generating your personalized roadmap..."
- [ ] Success: Result page loads with roadmap
- [ ] Error: Error message displays with API error

#### Result Page Tests
- [ ] User greeting shows correct name
- [ ] Goal displays correctly
- [ ] Current skills shown in green badges
- [ ] Missing skills shown in red badges
- [ ] "No missing skills" message shows when user has all skills
- [ ] Progress tracker shows correct skill count
- [ ] All checkboxes are functional
- [ ] Progress bar updates when skills are checked

#### Export Functionality Tests
- [ ] "📥 Export" button is visible
- [ ] Clicking export downloads `.txt` file
- [ ] Filename format: `roadmap-{name}.txt`
- [ ] File contains all required data:
  - [ ] User name
  - [ ] Career goal
  - [ ] Current skills list
  - [ ] Missing skills list
  - [ ] Full roadmap text
  - [ ] Generation timestamp

#### History/localStorage Tests
- [ ] After first analysis, "Recent Analyses" section appears
- [ ] Up to 5 previous analyses are shown
- [ ] Each history card shows:
  - [ ] User name
  - [ ] Career goal
  - [ ] Timestamp
- [ ] Clicking a history card reloads that result
- [ ] History persists after browser refresh
- [ ] History persists across browser sessions

#### Start Over Button
- [ ] Clicking "← Start Over" returns to form
- [ ] All form fields are empty
- [ ] Previous result is cleared

#### UI/UX Tests
- [ ] Page is responsive on mobile (320px)
- [ ] Page is responsive on tablet (768px)
- [ ] Page is responsive on desktop (1024px+)
- [ ] Colors are consistent
- [ ] Text is readable
- [ ] Buttons have hover effects
- [ ] No console errors

---

### 3. Integration Tests

#### Full User Flow: New User
1. [ ] Visit frontend
2. [ ] Enter name: "Alice"
3. [ ] Enter skills: "Python, SQL"
4. [ ] Select goal: "Data Analyst"
5. [ ] Click "Analyze"
6. [ ] Wait for loading
7. [ ] Result shows:
   - [ ] Greeting "Hi Alice! 👋"
   - [ ] Current skills: Python, SQL
   - [ ] Missing skills: Excel, Power BI
   - [ ] 4-week roadmap appears
8. [ ] Check a missing skill, progress bar updates
9. [ ] Click "📥 Export", file downloads
10. [ ] Click "← Start Over", returns to form

#### Full User Flow: Returning User
1. [ ] Submit first analysis
2. [ ] On result page, check skills and export
3. [ ] Click "← Start Over"
4. [ ] Verify "Recent Analyses" section shows
5. [ ] Click first recent analysis
6. [ ] Verify same result loads
7. [ ] Refresh page
8. [ ] Verify history still exists

#### Error Recovery Flow
1. [ ] Disconnect internet/Groq API
2. [ ] Submit analysis
3. [ ] Wait for retries
4. [ ] Error message appears
5. [ ] Reconnect internet
6. [ ] Submit again, should work
7. [ ] Result displays correctly

---

### 4. Performance Tests

#### Load Time Tests
- [ ] Form loads in < 2 seconds
- [ ] Result page loads in < 5 seconds
- [ ] API response time: 4-8 seconds (Groq API)
- [ ] Export download: < 1 second

#### Memory Tests
- [ ] No memory leaks in browser console
- [ ] localStorage doesn't exceed 5MB
- [ ] No "out of memory" errors

#### Concurrent Request Tests
- [ ] Rapid successive submissions handle gracefully
- [ ] Only latest request is processed
- [ ] No race conditions

---

### 5. Error Scenario Tests

#### Network Errors
- [ ] Timeout (> 30s) shows error: "⏱️ Service temporarily unavailable"
- [ ] Connection refused shows error: "❌ Failed to connect to server"
- [ ] CORS error shows appropriate message

#### API Errors
- [ ] Invalid API key shows: "🔑 Invalid Groq API key"
- [ ] Rate limit (429) shows: "⏱️ API rate limit hit"
- [ ] Service unavailable (503) shows: "⚠️ Service temporarily unavailable"

#### Input Errors
- [ ] Names > 50 chars rejected
- [ ] XSS attempts (e.g., `<script>`) sanitized
- [ ] Special characters in skills handled

---

### 6. Accessibility Tests

- [ ] Form labels are properly associated with inputs
- [ ] Keyboard navigation works (Tab, Enter)
- [ ] Color contrast passes WCAG AA
- [ ] Screen reader can navigate (test with Narrator/VoiceOver)
- [ ] Loading spinner is not distracting

---

### 7. Browser Compatibility Tests

Test in:
- [ ] Chrome (latest)
- [ ] Firefox (latest)
- [ ] Safari (latest)
- [ ] Edge (latest)
- [ ] Mobile Safari (iOS)
- [ ] Chrome Mobile (Android)

---

## Automated Testing (Future)

### Unit Tests (Jest + React Testing Library)
```bash
npm test
```

### E2E Tests (Cypress)
```bash
npm run cypress
```

---

## Test Data

Use these credentials for testing:

**Test Case 1: Complete Skill Set**
```
Name: Sarah
Skills: HTML, CSS, JavaScript, React
Goal: Frontend Developer
Expected: "You already have all skills" message
```

**Test Case 2: No Skills**
```
Name: Mike
Skills: Basic Cooking
Goal: Backend Developer
Expected: Node.js, Express, MongoDB as missing
```

**Test Case 3: Partial Skills**
```
Name: Emma
Skills: Python, Excel
Goal: Data Analyst
Expected: SQL, Power BI as missing
```

**Test Case 4: Case Insensitivity**
```
Name: Dev
Skills: html, css, JAVASCRIPT
Goal: Frontend Developer
Expected: React as missing (case doesn't matter)
```

---

## Regression Testing

After each update, verify:
1. All API endpoints still work
2. Old analyses from localStorage load correctly
3. No new console errors
4. Response times haven't degraded
5. UI still looks correct

