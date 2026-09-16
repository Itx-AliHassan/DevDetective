# 🕵️ DevDetective

### AI-Powered Code Analysis & Project Health Platform

DevDetective is a web-based developer platform that analyzes GitHub repositories and provides a detailed report about **code quality, security, architecture, testing, and documentation**.

The system combines **Node.js, Python, static analysis tools, GitHub APIs, MongoDB, and AI** to turn raw technical findings into understandable recommendations.

---

# 1. Project Vision

The goal is **not** to build another simple AI code-review chatbot.

DevDetective should work more like a **software project detective**:

```text
GitHub Repository
       ↓
Repository Scanner
       ↓
Code Analysis
       ↓
Security Analysis
       ↓
Architecture Analysis
       ↓
Testing Analysis
       ↓
Documentation Analysis
       ↓
AI Explanation
       ↓
Project Health Score
       ↓
Interactive Dashboard
```

The system should answer:

> "How healthy is this project, what problems does it have, and what should the developer do about them?"

---

# 2. Recommended Architecture

The best architecture for this project is a **hybrid Node.js + Python architecture**.

```text
                    ┌─────────────────┐
                    │    React App    │
                    │    Frontend     │
                    └────────┬────────┘
                             │
                             ↓
                    ┌─────────────────┐
                    │ Node.js/Express │
                    │   Main Backend  │
                    └────────┬────────┘
                             │
              ┌──────────────┼──────────────┐
              ↓              ↓              ↓
        ┌──────────┐   ┌───────────┐   ┌──────────┐
        │ MongoDB  │   │  GitHub   │   │  Python  │
        │ Database │   │    API    │   │ Analyzer │
        └──────────┘   └───────────┘   └────┬─────┘
                                             │
                         ┌───────────────────┼────────────────┐
                         ↓                   ↓                ↓
                   Code Quality         Security          Architecture
                         │                   │                │
                         └───────────────────┼────────────────┘
                                             ↓
                                        AI Service
                                             ↓
                                       Final Report
```

## Why this architecture?

### Node.js

Node.js should be responsible for the main application.

Use it for:

* Authentication
* Users
* GitHub OAuth
* GitHub API communication
* Repository management
* Scan management
* API endpoints
* Database operations
* Report management
* Frontend communication

You already work with Node.js, so this keeps the main backend in your strongest ecosystem.

### Python

Python should be the **analysis engine**.

Use Python for:

* AST analysis
* Code parsing
* Static analysis
* Security analysis
* Code metrics
* AI processing where useful
* Analysis orchestration

Python has a strong ecosystem for source-code analysis, making it a better choice for this specific part of the system.

### MongoDB

MongoDB stores application and analysis data.

It should store:

* Users
* GitHub repositories
* Scan history
* Issues
* Reports
* Scores
* AI explanations

---

# 3. Technology Stack

## Frontend

```text
React
JavaScript
Tailwind CSS
React Router
Axios
React Hot Toast
Recharts
```

### Why?

React handles the dashboard and application interface.

Tailwind handles styling.

React Router handles application pages.

Axios communicates with the Node.js backend.

Recharts can display project-health statistics and charts.

---

# 4. Backend Stack

```text
Node.js
Express.js
MongoDB
Mongoose
JWT
Cookie-based Authentication
Axios
GitHub API
GitHub OAuth
```

### Main responsibility

The Node.js backend acts as the **central controller** of the entire application.

---

# 5. Analysis Engine

```text
Python
AST
ESLint
Ruff/Pylint
Semgrep
Bandit
Dependency analysis tools
```

The exact tools can be adjusted during development.

The important idea is:

```text
Python
   ↓
Receive repository
   ↓
Analyze files
   ↓
Run specialized scanners
   ↓
Normalize results
   ↓
Return structured findings
```

---

# 6. AI Layer

AI should **not** be responsible for detecting everything.

Bad architecture:

```text
Entire Repository
       ↓
      AI
       ↓
   "Find bugs"
```

This can produce unreliable results.

Recommended architecture:

```text
Repository
    ↓
Static Analysis
    ↓
Actual Finding
    ↓
AI
    ↓
Explanation + Recommendation
```

For example:

```text
Scanner detects:

Hardcoded secret
File: auth.js
Line: 42
Severity: Critical
```

Then AI receives the finding and generates:

```text
Why this is dangerous:
The secret is stored directly inside source code.

Recommended fix:
Move the secret into an environment variable.
```

This makes the system more reliable.

---

# 7. GitHub Integration

GitHub is the entry point for repository analysis.

## Required functionality

### GitHub OAuth

User clicks:

```text
Connect GitHub
```

Then:

```text
User
 ↓
GitHub
 ↓
Authorization
 ↓
DevDetective
```

The application receives the required authorization information.

### Repository selection

After authentication:

```text
Your Repositories

□ AudioVault
□ BankSystem
□ Chess Game
□ Portfolio
```

User selects one.

```text
[ Analyze Repository ]
```

---

# 8. Repository Scanning

The scanning process should look like:

```text
1. User selects repository
              ↓
2. Backend validates access
              ↓
3. Repository information retrieved
              ↓
4. Required files collected
              ↓
5. Analysis job created
              ↓
6. Python analyzer starts
              ↓
7. Static analysis runs
              ↓
8. Security analysis runs
              ↓
9. Architecture analysis runs
              ↓
10. Testing/documentation analysis
              ↓
11. AI explanation
              ↓
12. Results stored in MongoDB
              ↓
13. Dashboard displays report
```

---

# 9. Backend Responsibilities

The backend should contain these major modules.

```text
backend/
│
├── config/
│   ├── db.js
│   └── github.js
│
├── controllers/
│   ├── auth.controller.js
│   ├── github.controller.js
│   ├── repository.controller.js
│   ├── scan.controller.js
│   └── report.controller.js
│
├── models/
│   ├── User.js
│   ├── Repository.js
│   ├── Scan.js
│   ├── Issue.js
│   └── Report.js
│
├── routes/
│   ├── auth.routes.js
│   ├── github.routes.js
│   ├── repository.routes.js
│   ├── scan.routes.js
│   └── report.routes.js
│
├── middleware/
│   ├── auth.middleware.js
│   ├── error.middleware.js
│   └── rateLimit.middleware.js
│
├── services/
│   ├── github.service.js
│   ├── scan.service.js
│   ├── python.service.js
│   └── ai.service.js
│
├── utils/
│   ├── scoring.js
│   └── validators.js
│
└── server.js
```

The exact structure can change as the project grows.

---

# 10. Backend API

## Authentication

```http
POST /api/auth/register
POST /api/auth/login
POST /api/auth/logout
GET  /api/auth/me
```

Purpose:

* Create account
* Login
* Logout
* Get current user

---

## GitHub

```http
GET /api/github/connect
GET /api/github/callback
GET /api/github/repositories
```

Purpose:

* GitHub authentication
* Get repositories
* Manage GitHub connection

---

## Repositories

```http
GET    /api/repositories
GET    /api/repositories/:id
POST   /api/repositories
DELETE /api/repositories/:id
```

Purpose:

Manage repositories connected to DevDetective.

---

## Scans

```http
POST /api/scans
GET  /api/scans
GET  /api/scans/:id
GET  /api/scans/:id/status
```

Purpose:

Start and monitor repository analysis.

---

## Reports

```http
GET /api/reports/:scanId
GET /api/reports/:scanId/issues
GET /api/reports/:scanId/score
```

Purpose:

Retrieve analysis results.

---

# 11. Database Design

## User

```text
User
├── name
├── email
├── password
├── githubId
├── githubAccessToken
├── createdAt
└── updatedAt
```

Sensitive GitHub credentials must be handled securely.

---

## Repository

```text
Repository
├── userId
├── githubRepoId
├── name
├── fullName
├── owner
├── language
├── url
├── defaultBranch
└── createdAt
```

---

## Scan

```text
Scan
├── repositoryId
├── status
├── startedAt
├── completedAt
├── securityScore
├── qualityScore
├── testingScore
├── documentationScore
├── architectureScore
└── overallScore
```

Possible statuses:

```text
QUEUED
RUNNING
COMPLETED
FAILED
```

---

## Issue

```text
Issue
├── scanId
├── category
├── severity
├── title
├── description
├── file
├── line
├── codeSnippet
├── recommendation
├── aiExplanation
└── status
```

Possible categories:

```text
SECURITY
QUALITY
PERFORMANCE
ARCHITECTURE
TESTING
DOCUMENTATION
```

Possible severity:

```text
CRITICAL
HIGH
MEDIUM
LOW
INFO
```

---

# 12. Python Analyzer

The Python project can have a separate structure.

```text
analyzer/
│
├── analyzers/
│   ├── javascript/
│   ├── python/
│   ├── security/
│   ├── architecture/
│   ├── testing/
│   └── documentation/
│
├── parsers/
│
├── scanners/
│
├── scoring/
│
├── ai/
│
├── models/
│
└── main.py
```

---

# 13. Code Quality Analysis

The system should detect things such as:

```text
Unused variables
Unused imports
Duplicate code
Very large functions
Complex functions
Poor naming
Missing error handling
Code smells
Repeated logic
Potential maintainability problems
```

The analyzer should return structured data.

Example:

```json
{
  "category": "QUALITY",
  "severity": "MEDIUM",
  "file": "controllers/user.js",
  "line": 42,
  "title": "Complex Function",
  "message": "Function contains high logical complexity."
}
```

---

# 14. Security Analysis

Security analysis is one of the most important parts.

Potential checks:

```text
Hardcoded API keys
Hardcoded passwords
Exposed secrets
Weak authentication patterns
Unsafe functions
Dependency vulnerabilities
Missing input validation
Potential injection risks
Insecure configuration
```

Example:

```text
🔴 CRITICAL

Hardcoded Secret

File:
config/auth.js

Line:
42

Reason:
A sensitive authentication secret is directly
stored in source code.

Recommendation:
Move the secret to an environment variable.
```

---

# 15. Architecture Analysis

The system should inspect the structure of the project.

Example:

```text
src/
├── controllers/
├── models/
├── routes/
├── services/
└── middleware/
```

It can identify:

* Extremely large files
* Circular dependencies
* Poor separation of responsibilities
* Unusual project structure
* High coupling
* Missing expected layers

The architecture analyzer should provide recommendations rather than claiming that one architecture is universally correct.

---

# 16. Testing Analysis

The system should identify important functionality that appears to lack tests.

For example:

```text
Authentication
├── login()
├── register()
└── logout()

Testing Status:

login()       ⚠ No test detected
register()    ⚠ No test detected
logout()      ✓ Test detected
```

Then AI can suggest:

```text
Recommended Tests

1. Valid login
2. Invalid password
3. Invalid email
4. Missing credentials
5. Non-existent user
```

---

# 17. Documentation Analysis

DevDetective should inspect:

```text
README.md
API documentation
Comments
Function documentation
Environment documentation
Installation instructions
```

It can identify missing documentation and generate suggestions.

Possible generated documentation:

```text
Project Overview
Installation
Environment Variables
API Endpoints
Authentication
Database Structure
Project Architecture
Usage
```

---

# 18. Project Health Score

The dashboard should provide multiple scores.

Example:

```text
Security       82/100
Code Quality   76/100
Architecture   71/100
Testing        48/100
Documentation  63/100

-------------------------

Overall Health
      68/100
```

## Important

The scoring system should be based on **documented rules**, not simply an AI-generated number.

For example:

```text
Base Score = 100

Critical issue → -20
High issue     → -10
Medium issue   → -5
Low issue      → -2
```

The exact scoring formula should be configurable and documented.

---

# 19. Frontend Structure

Recommended React structure:

```text
frontend/
│
├── src/
│   │
│   ├── components/
│   │   ├── Navbar.jsx
│   │   ├── Sidebar.jsx
│   │   ├── ScoreCard.jsx
│   │   ├── IssueCard.jsx
│   │   ├── SeverityBadge.jsx
│   │   └── Loading.jsx
│   │
│   ├── pages/
│   │   ├── Landing.jsx
│   │   ├── Login.jsx
│   │   ├── Register.jsx
│   │   ├── Dashboard.jsx
│   │   ├── Repositories.jsx
│   │   ├── RepositoryDetails.jsx
│   │   ├── Scan.jsx
│   │   ├── Report.jsx
│   │   └── Settings.jsx
│   │
│   ├── services/
│   │   ├── api.js
│   │   ├── auth.api.js
│   │   ├── github.api.js
│   │   ├── scan.api.js
│   │   └── report.api.js
│   │
│   ├── hooks/
│   │
│   ├── context/
│   │
│   ├── utils/
│   │
│   ├── App.jsx
│   └── main.jsx
```

Keep the frontend modular, but **don't create files just for the sake of having files**.

---

# 20. Main Frontend Pages

## Landing Page

Explain:

```text
What is DevDetective?
How does it work?
Why use it?
Features
GitHub integration
Call to action
```

---

## Authentication

```text
Login
Register
GitHub Connect
```

---

## Dashboard

Main overview:

```text
Welcome back 👋

Repositories
Total Scans
Issues Found
Average Health

Recent Projects

Project Health
Security Issues
Quality Issues
```

---

## Repository Page

Show:

```text
Repository Name
Language
Last Scan
Health Score

[Start New Scan]
```

---

## Scan Page

Show live status:

```text
Analyzing Repository...

✓ Repository downloaded
✓ Code structure analyzed
✓ Security scan completed
⏳ AI analysis running
○ Generating report
```

---

# 21. Report Page

This is the **main feature of the frontend**.

Display:

```text
Project Health: 78/100
```

Then:

```text
Security       82
Code Quality   76
Architecture   71
Testing        48
Documentation  63
```

Then:

```text
Critical Issues
High Issues
Medium Issues
Low Issues
```

Users can click an issue to see:

```text
File
Line
Code
Problem
Why it matters
AI Explanation
Suggested Solution
```

---

# 22. Recommended User Flow

The complete user experience should be:

```text
Landing Page
      ↓
Create Account
      ↓
Connect GitHub
      ↓
Select Repository
      ↓
Start Analysis
      ↓
Wait for Scan
      ↓
Project Health Report
      ↓
Explore Issues
      ↓
Read AI Explanations
      ↓
Fix Problems
      ↓
Run Scan Again
      ↓
Compare Results
```

---

# 23. Scan Processing

For the FYP MVP, start with a simple architecture:

```text
Node.js
   ↓
Start Python Process
   ↓
Python analyzes repository
   ↓
Python returns JSON
   ↓
Node.js stores results
   ↓
Frontend receives results
```

Example:

```text
Node.js
     │
     │ spawn()
     ↓
Python Analyzer
     │
     │ JSON
     ↓
Node.js
     │
     ↓
MongoDB
```

### Future improvement

If scans become large, introduce a job queue:

```text
Node.js
 ↓
Redis / Queue
 ↓
Worker
 ↓
Python Analyzer
```

For the initial FYP, **don't over-engineer this**.

---

# 24. Communication Between Node.js and Python

Use **JSON** as the communication format.

Example input:

```json
{
  "scanId": "12345",
  "repositoryPath": "/tmp/project",
  "languages": ["javascript", "python"]
}
```

Example output:

```json
{
  "scanId": "12345",
  "status": "completed",
  "issues": [],
  "scores": {
    "security": 82,
    "quality": 76
  }
}
```

This keeps the two systems independent.

---

# 25. Environment Variables

## Node.js

```env
PORT=
MONGO_URI=
JWT_SECRET=

GITHUB_CLIENT_ID=
GITHUB_CLIENT_SECRET=
GITHUB_CALLBACK_URL=

AI_API_KEY=
```

## Python

```env
AI_API_KEY=
```

Never commit `.env` files to GitHub.

Use:

```text
.env
.gitignore
.env.example
```

---

# 26. Security Requirements

Because DevDetective analyzes source code, security is extremely important.

The system should:

* Never expose GitHub access tokens to the frontend unnecessarily.
* Never expose AI API keys.
* Never store secrets in source code.
* Validate uploaded/received data.
* Restrict repository access.
* Sanitize analysis inputs.
* Limit scan requests.
* Avoid executing untrusted project code directly.
* Isolate repository analysis where practical.

### VERY IMPORTANT

Do **not** simply execute arbitrary repository files using:

```text
node project.js
python project.py
```

A repository can contain malicious code.

DevDetective should primarily perform **static analysis** instead of running the user's application.

---

# 27. MVP Scope

For the first working version, build only:

```text
✓ User authentication
✓ GitHub connection
✓ Repository selection
✓ Repository scanning
✓ JavaScript/TypeScript analysis
✓ Python analysis
✓ Security analysis
✓ Code-quality analysis
✓ Basic architecture analysis
✓ AI explanations
✓ Health score
✓ Dashboard
✓ Scan history
```

This is enough for a strong FYP.

---

# 28. Future Features

After the MVP:

```text
VS Code Extension
        ↓
Pull Request Analysis
        ↓
CI/CD Integration
        ↓
Automatic Fix Suggestions
        ↓
More Programming Languages
        ↓
Team Collaboration
        ↓
Historical Analytics
        ↓
Urdu + English Explanations
```

---

# 29. Development Order

Do NOT build everything simultaneously.

Follow this order:

### Phase 1 — Foundation

```text
React setup
Node.js setup
MongoDB setup
Authentication
```

### Phase 2 — GitHub

```text
GitHub OAuth
Repository listing
Repository selection
```

### Phase 3 — Scanning

```text
Repository retrieval
Scan creation
Python analyzer
Node ↔ Python communication
```

### Phase 4 — Analysis

```text
Code quality
Security
Architecture
Testing
Documentation
```

### Phase 5 — AI

```text
Finding explanation
Recommendations
Documentation generation
Test suggestions
```

### Phase 6 — Dashboard

```text
Health score
Charts
Issue lists
Issue details
Scan history
```

### Phase 7 — Final Polish

```text
Error handling
Security
Performance
Responsive UI
Testing
Deployment
Documentation
```

---

# 30. Final Recommended Stack

```text
Frontend
──────────────
React
JavaScript
Tailwind CSS
React Router
Axios
Recharts

Backend
──────────────
Node.js
Express.js
MongoDB
Mongoose
JWT
GitHub API
GitHub OAuth

Analysis
──────────────
Python
AST
ESLint
Ruff/Pylint
Semgrep
Bandit

AI
──────────────
LLM API

Development
──────────────
Git
GitHub
VS Code
Postman

Optional Later
──────────────
Redis
BullMQ
Docker
GitHub Actions
```

---

# 31. The Core Principle

The most important design decision in DevDetective is:

> **Use traditional/static analysis to FIND problems and AI to EXPLAIN problems.**

Not:

> AI does everything.

The first approach gives you a more technically defensible and reliable FYP.

---

# 32. Final System

At completion, the project should work like this:

```text
                    DEVDETECTIVE
                         │
                         ↓
                  Connect GitHub
                         │
                         ↓
                 Select Repository
                         │
                         ↓
                    Start Scan
                         │
             ┌───────────┴───────────┐
             ↓                       ↓
       Static Analysis          Security Scan
             │                       │
             └───────────┬───────────┘
                         ↓
                  Architecture
                     Analysis
                         │
                         ↓
                  Testing Analysis
                         │
                         ↓
               Documentation Analysis
                         │
                         ↓
                    AI Layer
                         │
                         ↓
                 Project Health
                      Score
                         │
                         ↓
                 React Dashboard
                         │
                         ↓
                 Developer Fixes
                         │
                         ↓
                   Scan Again
```

## Final Goal

DevDetective should make a developer feel like they have a **second pair of eyes reviewing their entire project**.

The system should not simply say:

> "There is an error."

It should tell the developer:

> **What is wrong → Where it is → Why it matters → How serious it is → How to improve it.**

That is the core idea behind DevDetective.
