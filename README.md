# DIAGSE - Code-Free ITS Authoring Platform

A web-based platform that enables teachers to design, configure, and deploy intelligent tutoring system (ITS) activities using a six-stage metacognitive design framework.

## Project Structure

```
ITSframework_app/
├── backend/           # FastAPI backend
│   ├── app/
│   │   ├── api/      # API routes
│   │   ├── core/     # Config, security
│   │   ├── models/   # SQLAlchemy models
│   │   ├── schemas/  # Pydantic schemas
│   │   ├── services/ # Business logic
│   │   └── db/       # Database setup
│   ├── tests/        # Backend tests
│   └── requirements.txt
├── frontend/         # React frontend (to be created)
├── docs/            # Documentation
└── docker-compose.yml
```

## Quick Start

### Prerequisites

- Docker and Docker Compose
- AI Provider API key (OpenAI, Gemini, Perplexity, or Claude)

### Setup

1. **Clone and navigate to the project:**
   ```bash
   cd ITSframework_app
   ```

2. **Set up environment variables:**
   ```bash
   # Create .env file in backend directory
   cp backend/.env.example backend/.env
   
   # Edit backend/.env and add your AI provider configuration
   # See QUICK_AI_SETUP.md for detailed provider options
   ```

3. **Start the services:**
   ```bash
   docker-compose up -d
   ```

4. **Check the backend is running:**
   ```bash
   curl http://localhost:8000/health
   ```

5. **Access the API documentation:**
   - Swagger UI: http://localhost:8000/docs
   - ReDoc: http://localhost:8000/redoc

### Development

**Backend development:**
```bash
# Install dependencies locally (optional, for IDE support)
cd backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

# Run tests
pytest

# Format code
black app/
isort app/
```

**Database migrations:**
```bash
# Create a new migration
docker-compose exec backend alembic revision --autogenerate -m "description"

# Apply migrations
docker-compose exec backend alembic upgrade head
```

**View logs:**
```bash
docker-compose logs -f backend
```

**Stop services:**
```bash
docker-compose down
```

## API Endpoints

### Authentication
- `POST /api/v1/auth/register` - Register new user
- `POST /api/v1/auth/login` - Login and get token
- `GET /api/v1/auth/me` - Get current user

### Activities (Teacher)
- `POST /api/v1/activities` - Create activity
- `GET /api/v1/activities` - List activities
- `GET /api/v1/activities/{id}` - Get activity
- `PUT /api/v1/activities/{id}` - Update activity
- `DELETE /api/v1/activities/{id}` - Delete draft activity
- `POST /api/v1/activities/{id}/publish` - Publish activity
- `POST /api/v1/activities/{id}/unpublish` - Unpublish activity
- `POST /api/v1/activities/{id}/duplicate` - Duplicate activity

### AI Suggestions (Teacher)
- `POST /api/v1/ai/suggest-skills` - Get skill suggestions
- `POST /api/v1/ai/suggest-steps` - Get step suggestions
- `POST /api/v1/ai/evaluate-response` - Evaluate student response

**Multi-Provider Support:**
- ✅ OpenAI (GPT-4, GPT-3.5)
- ✅ Google Gemini (Free tier available!)
- ✅ Perplexity (with citations)
- ✅ Anthropic Claude

See [QUICK_AI_SETUP.md](QUICK_AI_SETUP.md) for setup and [AI_PROVIDERS.md](AI_PROVIDERS.md) for detailed comparison.

### Student Runtime
- `GET /api/v1/student/activities` - List published activities
- `GET /api/v1/student/activities/{id}` - Get activity details
- `POST /api/v1/student/activities/{id}/start` - Start/resume activity
- `POST /api/v1/student/activities/{id}/submit` - Submit step response
- `GET /api/v1/student/activities/{id}/progress` - Get progress

### Analytics (Teacher)
- `GET /api/v1/analytics/activities/{id}` - Get activity analytics

## Library Features

DIAGSE includes a complete activity library system:

✅ **Save Projects** - Activities auto-save as drafts  
✅ **Continue Editing** - Return to any activity anytime  
✅ **Duplicate & Edit** - Clone activities for variations  
✅ **Deploy to Students** - Publish when ready  
✅ **Student Discovery** - Students see published activities  
✅ **Progress Tracking** - Track each student's progress  

See [LIBRARY_FEATURES.md](LIBRARY_FEATURES.md) for details.

**Test the library:**
```bash
./test_library.sh
```

## Testing the API

### Register a teacher:
```bash
curl -X POST http://localhost:8000/api/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "teacher@example.com",
    "password": "password123",
    "name": "Jane Teacher",
    "role": "teacher"
  }'
```

### Login:
```bash
curl -X POST http://localhost:8000/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "teacher@example.com",
    "password": "password123"
  }'
```

### Create an activity (use token from login):
```bash
curl -X POST http://localhost:8000/api/v1/activities \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -d '{
    "title": "Visual Source Inference",
    "subject": "History",
    "level": "Sec 3",
    "diagnosed_gap": "Students describe but cannot infer",
    "skill_label": "Visual source inference",
    "bloom_level": "Analyze"
  }'
```

## Next Steps

1. **Frontend Development:**
   - Set up React + TypeScript + Vite
   - Create DIAGSE wizard components
   - Build student runtime interface

2. **AI Service Implementation:**
   - Implement OpenAI integration
   - Create prompt templates
   - Add response classification logic

3. **Runtime Engine:**
   - Implement step progression logic
   - Add selection validation (image/text)
   - Build fading mechanism

4. **Testing:**
   - Add unit tests
   - Add integration tests
   - Test with real teachers

## Documentation

- [MVP Requirements](docs/DIAGSE%20-%20MVP%20REQUIREMENTS.md)
- [MVP Design](docs/DIAGSE%20-%20MVP%20DESIGN.md)

## License

TBD
