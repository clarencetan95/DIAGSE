# DIAGSE Project Structure

## Complete File Tree

```
ITSframework_app/
│
├── 📄 START_HERE.md              ← Read this first!
├── 📄 QUICKSTART.md              ← 5-minute setup guide
├── 📄 PROTOTYPE_SUMMARY.md       ← What we've built
├── 📄 PROJECT_STATUS.md          ← Development roadmap
├── 📄 README.md                  ← Full documentation
├── 📄 STRUCTURE.md               ← This file
│
├── 🔧 docker-compose.yml         ← Development environment
├── 🔧 .gitignore                 ← Git ignore rules
│
├── 🧪 test_api.sh                ← API test script
├── 🧪 check_setup.sh             ← Setup verification
│
├── 📁 docs/
│   ├── DIAGSE - MVP REQUIREMENTS.md  ← Detailed requirements
│   └── DIAGSE - MVP DESIGN.md        ← Technical design
│
├── 📁 backend/                   ← FastAPI backend
│   ├── 📄 Dockerfile
│   ├── 📄 requirements.txt
│   ├── 📄 .env.example
│   │
│   └── 📁 app/
│       ├── 📄 main.py            ← FastAPI application entry point
│       │
│       ├── 📁 core/              ← Core functionality
│       │   ├── config.py         ← Settings and configuration
│       │   └── security.py       ← JWT auth, password hashing
│       │
│       ├── 📁 db/                ← Database setup
│       │   └── database.py       ← SQLAlchemy connection
│       │
│       ├── 📁 models/            ← Database models (SQLAlchemy)
│       │   ├── __init__.py
│       │   ├── user.py           ← User model (teachers, students)
│       │   ├── activity.py       ← Activity model (ITS config)
│       │   ├── progress.py       ← Student progress tracking
│       │   └── interaction_log.py ← Interaction history
│       │
│       ├── 📁 schemas/           ← Request/Response schemas (Pydantic)
│       │   ├── user.py           ← User schemas
│       │   └── activity.py       ← Activity schemas
│       │
│       ├── 📁 api/               ← API endpoints
│       │   ├── __init__.py
│       │   ├── auth.py           ← Authentication (register, login)
│       │   ├── activities.py     ← Activity CRUD
│       │   ├── ai.py             ← AI suggestions (placeholder)
│       │   ├── student.py        ← Student runtime (placeholder)
│       │   └── analytics.py      ← Analytics (placeholder)
│       │
│       └── 📁 services/          ← Business logic (to be created)
│           ├── ai_service.py     ← OpenAI integration
│           ├── runtime_service.py ← Step progression
│           └── analytics_service.py ← Data aggregation
│
└── 📁 frontend/                  ← React frontend (to be created)
    ├── 📄 package.json
    ├── 📄 vite.config.ts
    ├── 📄 tsconfig.json
    │
    └── 📁 src/
        ├── 📁 components/
        │   ├── authoring/        ← DIAGSE wizard
        │   ├── runtime/          ← Student interface
        │   └── shared/           ← Reusable components
        ├── 📁 hooks/             ← React hooks
        ├── 📁 stores/            ← State management
        ├── 📁 api/               ← API client
        └── 📁 types/             ← TypeScript types
```

---

## Backend Structure Explained

### 📁 app/core/
**Purpose:** Core application functionality

- `config.py` - Environment variables, settings
- `security.py` - JWT tokens, password hashing, auth dependencies

### 📁 app/db/
**Purpose:** Database connection and session management

- `database.py` - SQLAlchemy engine, session factory, Base class

### 📁 app/models/
**Purpose:** Database table definitions (SQLAlchemy ORM)

- `user.py` - Users table (teachers and students)
- `activity.py` - Activities table (ITS configurations)
- `progress.py` - Student progress per activity
- `interaction_log.py` - Detailed interaction history

**Key Pattern:** Each model inherits from `Base` and maps to a database table.

### 📁 app/schemas/
**Purpose:** Request/response validation (Pydantic)

- `user.py` - UserCreate, UserLogin, UserResponse, TokenResponse
- `activity.py` - ActivityCreate, ActivityUpdate, ActivityResponse

**Key Pattern:** Schemas validate incoming data and serialize outgoing data.

### 📁 app/api/
**Purpose:** REST API endpoints (FastAPI routers)

- `auth.py` - POST /register, POST /login, GET /me
- `activities.py` - CRUD operations for activities
- `ai.py` - AI suggestion endpoints (placeholder)
- `student.py` - Student runtime endpoints (placeholder)
- `analytics.py` - Analytics endpoints (placeholder)

**Key Pattern:** Each file is a FastAPI router with related endpoints.

### 📁 app/services/ (To Be Created)
**Purpose:** Business logic layer

- `ai_service.py` - OpenAI API calls, prompt construction
- `runtime_service.py` - Step progression, evaluation logic
- `analytics_service.py` - Data aggregation, statistics

**Key Pattern:** Services contain reusable business logic called by API endpoints.

---

## Database Schema

### users
```sql
id              UUID PRIMARY KEY
email           VARCHAR(255) UNIQUE
password_hash   VARCHAR(255)
name            VARCHAR(255)
role            ENUM('teacher', 'student')
created_at      TIMESTAMP
updated_at      TIMESTAMP
```

### activities
```sql
id                      UUID PRIMARY KEY
teacher_id              UUID FOREIGN KEY → users.id
title                   VARCHAR(255)
subject                 VARCHAR(100)
level                   VARCHAR(20)
status                  ENUM('draft', 'published')
diagnosed_gap           TEXT
skill_label             VARCHAR(255)
bloom_level             VARCHAR(50)
syllabus_topic          VARCHAR(100)
alignment_justification TEXT
stimulus_type           ENUM('question', 'text', 'image')
stimulus_content        TEXT
stimulus_annotations    JSONB
steps                   JSONB (array of step objects)
fading_enabled          BOOLEAN
fading_threshold        INTEGER
due_date                TIMESTAMP
created_at              TIMESTAMP
updated_at              TIMESTAMP
```

### student_progress
```sql
id                UUID PRIMARY KEY
activity_id       UUID FOREIGN KEY → activities.id
student_id        UUID FOREIGN KEY → users.id
current_step_index INTEGER
completed         BOOLEAN
step_states       JSONB (array of state objects)
started_at        TIMESTAMP
updated_at        TIMESTAMP
completed_at      TIMESTAMP
```

### interaction_logs
```sql
id               UUID PRIMARY KEY
activity_id      UUID FOREIGN KEY → activities.id
student_id       UUID FOREIGN KEY → users.id
step_index       INTEGER
step_id          UUID
response         TEXT
selection        JSONB
classification   VARCHAR(20)
error_type       VARCHAR(100)
feedback         TEXT
scaffold_level   VARCHAR(20)
attempt_number   INTEGER
response_time_ms INTEGER
created_at       TIMESTAMP
```

---

## API Routes

### Authentication (`/api/v1/auth`)
```
POST   /register    - Create new user account
POST   /login       - Login and get JWT token
GET    /me          - Get current user info
```

### Activities (`/api/v1/activities`)
```
POST   /                    - Create activity
GET    /                    - List activities
GET    /{id}                - Get activity details
PUT    /{id}                - Update activity
DELETE /{id}                - Delete draft activity
POST   /{id}/publish        - Publish activity
POST   /{id}/unpublish      - Unpublish activity
POST   /{id}/stimulus       - Upload stimulus file
```

### AI Suggestions (`/api/v1/ai`)
```
POST   /suggest-skills      - Get skill suggestions
POST   /suggest-steps       - Get expert step suggestions
POST   /suggest-scaffolds   - Get scaffold suggestions
POST   /suggest-diagnostics - Get diagnostic suggestions
```

### Student Runtime (`/api/v1/student`)
```
GET    /activities          - List assigned activities
GET    /activities/{id}     - Get activity for student
POST   /activities/{id}/start   - Start or resume activity
POST   /activities/{id}/submit  - Submit step response
GET    /activities/{id}/progress - Get current progress
```

### Analytics (`/api/v1/analytics`)
```
GET    /activities/{id}             - Activity-level stats
GET    /activities/{id}/students    - List students with progress
GET    /activities/{id}/students/{sid} - Detailed student responses
```

---

## Configuration Files

### docker-compose.yml
Defines two services:
- `db` - PostgreSQL 15 database
- `backend` - FastAPI application

### backend/Dockerfile
Python 3.11 slim image with:
- System dependencies (gcc, postgresql-client)
- Python dependencies from requirements.txt
- Application code

### backend/.env
Environment variables:
- `DATABASE_URL` - PostgreSQL connection string
- `JWT_SECRET` - Secret key for JWT tokens
- `OPENAI_API_KEY` - OpenAI API key
- `CORS_ORIGINS` - Allowed frontend origins

### backend/requirements.txt
Python dependencies:
- FastAPI, Uvicorn (web framework)
- SQLAlchemy, Alembic (database)
- python-jose, passlib (authentication)
- OpenAI (AI integration)
- Pydantic (validation)

---

## Development Workflow

### 1. Make Changes
Edit files in `backend/app/`

### 2. Auto-Reload
Backend automatically reloads (thanks to `--reload` flag)

### 3. Test
- Use Swagger UI: http://localhost:8000/docs
- Or run: `./test_api.sh`

### 4. View Logs
```bash
docker-compose logs -f backend
```

### 5. Commit
```bash
git add .
git commit -m "Description of changes"
```

---

## Next Files to Create

### Backend
- [ ] `app/services/ai_service.py` - OpenAI integration
- [ ] `app/services/runtime_service.py` - Step progression logic
- [ ] `app/services/selection_validator.py` - Image/text validation
- [ ] `app/services/analytics_service.py` - Data aggregation
- [ ] `tests/test_auth.py` - Authentication tests
- [ ] `tests/test_activities.py` - Activity CRUD tests

### Frontend
- [ ] `frontend/package.json` - Dependencies
- [ ] `frontend/src/main.tsx` - Entry point
- [ ] `frontend/src/App.tsx` - Root component
- [ ] `frontend/src/components/authoring/DIAGSEWizard.tsx`
- [ ] `frontend/src/components/runtime/ActivityPlayer.tsx`

### Documentation
- [ ] API usage examples
- [ ] Deployment guide
- [ ] Contributing guidelines

---

## Key Technologies

### Backend
- **FastAPI** - Modern Python web framework
- **SQLAlchemy** - SQL toolkit and ORM
- **PostgreSQL** - Relational database
- **Pydantic** - Data validation
- **python-jose** - JWT tokens
- **passlib** - Password hashing
- **OpenAI** - AI integration

### Frontend (To Be Added)
- **React 18** - UI library
- **TypeScript** - Type safety
- **Vite** - Build tool
- **TanStack Query** - Data fetching
- **Zustand** - State management
- **Tailwind CSS** - Styling
- **Fabric.js** - Image annotation

### Infrastructure
- **Docker** - Containerization
- **Docker Compose** - Multi-container orchestration
- **Nginx** - Reverse proxy (production)

---

## Useful Commands

```bash
# Development
docker-compose up -d              # Start services
docker-compose logs -f backend    # View logs
docker-compose restart backend    # Restart after changes
docker-compose down               # Stop services

# Testing
./test_api.sh                     # Test API
./check_setup.sh                  # Verify setup
curl http://localhost:8000/health # Health check

# Database
docker-compose exec db psql -U diagse  # Connect to DB
docker-compose down -v            # Reset database

# Code Quality
cd backend
black app/                        # Format code
isort app/                        # Sort imports
pytest                            # Run tests
```

---

**Ready to explore? Start with [START_HERE.md](START_HERE.md)!**
