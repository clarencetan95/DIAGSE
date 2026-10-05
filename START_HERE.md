# 🚀 START HERE - DIAGSE Prototype

Welcome to the DIAGSE platform prototype! This guide will get you up and running in minutes.

---

## 📋 What Is This?

**DIAGSE** is a code-free platform for teachers to create intelligent tutoring systems (ITS). Teachers follow a 6-stage design framework to build adaptive learning activities, and students get personalized feedback powered by AI.

**Current Status:** Working backend prototype with authentication and activity management.

---

## ⚡ Quick Start (5 Minutes)

### 1. Check Your Setup
```bash
./check_setup.sh
```

This will verify you have Docker and Docker Compose installed.

### 2. Configure Environment
```bash
# Copy the example environment file
cp backend/.env.example backend/.env

# Edit it and add your AI provider key
nano backend/.env  # or use your preferred editor
```

**Choose your AI provider:**

**Option 1: Google Gemini (FREE - Recommended)**
```bash
AI_PROVIDER=gemini
GEMINI_API_KEY=your-key-here
```
Get free key: https://aistudio.google.com/app/apikey

**Option 2: OpenAI (Default)**
```bash
AI_PROVIDER=openai
OPENAI_API_KEY=sk-your-key-here
```
Get key: https://platform.openai.com/api-keys

**More options:** See [QUICK_AI_SETUP.md](QUICK_AI_SETUP.md) for Perplexity, Claude, and detailed comparison.

### 3. Start the Services
```bash
docker-compose up -d
```

Wait about 10 seconds for services to start.

### 4. Test It Works
```bash
./test_api.sh
```

You should see successful API responses!

### 5. Explore the API
Open in your browser:
- **Swagger UI**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc

---

## 📚 Documentation

Choose your path:

### 🏃 I want to start coding now
→ Read [QUICKSTART.md](QUICKSTART.md)

### 📖 I want to understand the project
→ Read [PROTOTYPE_SUMMARY.md](PROTOTYPE_SUMMARY.md)

### 🗺️ I want to see the roadmap
→ Read [PROJECT_STATUS.md](PROJECT_STATUS.md)

### 📋 I want detailed requirements
→ Read [docs/DIAGSE - MVP REQUIREMENTS.md](docs/DIAGSE%20-%20MVP%20REQUIREMENTS.md)

### 🏗️ I want technical design details
→ Read [docs/DIAGSE - MVP DESIGN.md](docs/DIAGSE%20-%20MVP%20DESIGN.md)

### 🔧 I want full documentation
→ Read [README.md](README.md)

---

## 🎯 What Can You Do Right Now?

### As a Developer

**Test the Library Features:**
```bash
# Run comprehensive library test
./test_library.sh

# This tests:
# - Creating activities
# - Saving and editing
# - Duplicating activities
# - Publishing to students
# - Student discovery and progress
```

**Test the API:**
```bash
# Check health
curl http://localhost:8000/health

# Register a teacher
curl -X POST http://localhost:8000/api/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "teacher@test.com",
    "password": "Test123!",
    "name": "Test Teacher",
    "role": "teacher"
  }'

# Explore in Swagger UI
open http://localhost:8000/docs
```

**View the code:**
```bash
# Backend structure
tree backend/app/

# Key files:
# - backend/app/main.py          # FastAPI app
# - backend/app/models/          # Database models
# - backend/app/api/             # API endpoints
# - backend/app/core/security.py # Authentication
```

**Make changes:**
1. Edit files in `backend/app/`
2. Backend auto-reloads
3. Test immediately in Swagger UI

### As a Teacher (Testing)

**Try the API in Swagger UI:**
1. Go to http://localhost:8000/docs
2. Click "Authorize" button
3. Register → Login → Get token
4. Try creating an activity
5. Give feedback on the API design

---

## 🛠️ Common Commands

```bash
# Start services
docker-compose up -d

# View logs
docker-compose logs -f backend

# Stop services
docker-compose down

# Restart after code changes
docker-compose restart backend

# Reset everything (WARNING: deletes data)
docker-compose down -v
docker-compose up -d

# Run tests
./test_api.sh

# Check setup
./check_setup.sh
```

---

## 🎨 What's Built vs What's Next

### ✅ Built (Working Now)
- PostgreSQL database with proper schema
- FastAPI backend with JWT authentication
- Activity CRUD operations
- Publish/unpublish functionality
- API documentation (Swagger)
- Docker development environment

### 🔨 Next to Build (Priority Order)
1. **AI Service** - OpenAI integration for suggestions and classification
2. **Frontend** - React app with DIAGSE wizard
3. **Stimulus Tools** - Image annotation and text highlighting
4. **Runtime Engine** - Student activity player with AI evaluation
5. **Analytics** - Teacher dashboard with student progress

See [PROJECT_STATUS.md](PROJECT_STATUS.md) for detailed roadmap.

---

## 🤔 Key Concepts

### How DIAGSE Works

**Teacher Side (Authoring):**
1. Teacher fills out DIAGSE wizard (6 stages)
2. System stores configuration as JSON
3. Teacher publishes activity

**Student Side (Runtime):**
1. Student opens activity
2. Runtime engine loads configuration
3. For each step:
   - Show stimulus and prompt
   - Student responds
   - AI evaluates using teacher's config
   - Show feedback
   - Apply fading rules

**No code is generated!** It's all configuration-driven.

### Architecture Pattern

```
Teacher Config (JSON in DB)
    +
Prompt Templates (in code)
    +
Student Response (runtime)
    ↓
AI Prompt (constructed dynamically)
    ↓
OpenAI API
    ↓
Feedback (from teacher config)
```

---

## 🐛 Troubleshooting

**Services won't start:**
```bash
# Check if ports are in use
lsof -i :8000
lsof -i :5432

# View logs
docker-compose logs
```

**Can't connect to API:**
```bash
# Check if backend is running
docker-compose ps

# Restart backend
docker-compose restart backend
```

**Database errors:**
```bash
# Reset database
docker-compose down -v
docker-compose up -d
```

**Need help?**
- Check the logs: `docker-compose logs backend`
- Review the documentation
- Check the code comments

---

## 🎓 Learning Resources

### Understanding the Codebase

**Start here:**
1. `backend/app/main.py` - Entry point
2. `backend/app/models/` - Database schema
3. `backend/app/api/auth.py` - Authentication example
4. `backend/app/api/activities.py` - CRUD example

**Key patterns:**
- FastAPI dependency injection
- SQLAlchemy ORM
- Pydantic validation
- JWT authentication

### Understanding DIAGSE

**Read these in order:**
1. [PROTOTYPE_SUMMARY.md](PROTOTYPE_SUMMARY.md) - Overview
2. [docs/DIAGSE - MVP REQUIREMENTS.md](docs/DIAGSE%20-%20MVP%20REQUIREMENTS.md) - What we're building
3. [docs/DIAGSE - MVP DESIGN.md](docs/DIAGSE%20-%20MVP%20DESIGN.md) - How we're building it

---

## 🚀 Ready to Build?

**Pick your next step:**

1. **Explore the prototype** → Run `./test_api.sh` and play with Swagger UI
2. **Implement AI service** → See Milestone 1 in [PROJECT_STATUS.md](PROJECT_STATUS.md)
3. **Build the frontend** → See Milestone 2 in [PROJECT_STATUS.md](PROJECT_STATUS.md)
4. **Add a feature** → Pick something from the roadmap
5. **Fix a bug** → Check the known issues

**Questions?** Review the documentation or ask for clarification.

**Let's build this! 🎉**
