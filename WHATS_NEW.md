# What's New - Multi-Provider AI Support

## 🎉 Major Update: Use Any AI Provider!

DIAGSE now supports **4 major AI providers** out of the box. Switch between them with just environment variables - no code changes needed!

---

## Supported Providers

| Provider | Status | Free Tier | Best For |
|----------|--------|-----------|----------|
| **OpenAI** | ✅ Ready | No | Quality & reliability |
| **Google Gemini** | ✅ Ready | Yes! | Development & cost |
| **Perplexity** | ✅ Ready | No | Citations & sources |
| **Anthropic Claude** | ✅ Ready | No | Long context |

---

## Quick Start

### 1. Choose Your Provider

**For Free Development:**
```bash
AI_PROVIDER=gemini
GEMINI_API_KEY=your-free-key
```
Get key: https://aistudio.google.com/app/apikey

**For Production:**
```bash
AI_PROVIDER=openai
OPENAI_API_KEY=your-key
```

### 2. Add to backend/.env

```bash
# Edit your .env file
nano backend/.env

# Add your chosen provider config
AI_PROVIDER=gemini
GEMINI_API_KEY=AIza...
```

### 3. Restart

```bash
docker-compose restart backend
```

### 4. Test

```bash
./test_ai_providers.sh
```

Done! 🚀

---

## What Changed?

### New Files

- `backend/app/services/ai_service.py` - Unified AI service
- `AI_PROVIDERS.md` - Detailed provider guide
- `QUICK_AI_SETUP.md` - Quick setup reference
- `test_ai_providers.sh` - Test script for AI endpoints

### Updated Files

- `backend/app/api/ai.py` - Now uses multi-provider service
- `backend/.env.example` - Shows all provider options
- `docker-compose.yml` - Passes all provider env vars
- `START_HERE.md` - Updated setup instructions
- `README.md` - Added provider information

### New API Endpoints

- `POST /api/v1/ai/suggest-skills` - ✅ Fully implemented
- `POST /api/v1/ai/suggest-steps` - ✅ Fully implemented
- `POST /api/v1/ai/evaluate-response` - ✅ Fully implemented

---

## Why This Matters

### Cost Savings
- **Gemini free tier:** 15 requests/min, 1500/day
- Perfect for development and testing
- Switch to paid provider only when needed

### Flexibility
- Not locked into one provider
- Switch anytime without code changes
- Compare quality and cost easily

### Reliability
- If one provider has issues, switch to another
- No downtime waiting for provider fixes
- Redundancy for production

### Features
- **Perplexity:** Get citations and sources
- **Claude:** Analyze long documents (200K tokens)
- **Gemini:** Fast and cost-effective
- **OpenAI:** Most reliable and consistent

---

## Migration Guide

### Already Using OpenAI?

No changes needed! OpenAI is still the default.

But you can switch to save costs:

```bash
# Add to backend/.env
AI_PROVIDER=gemini
GEMINI_API_KEY=your-free-key

# Restart
docker-compose restart backend
```

Your existing activities and data are unchanged.

### Starting Fresh?

Follow the Quick Start above. We recommend Gemini for development (free tier).

---

## Examples

### Test All Providers

```bash
# Test OpenAI
echo "AI_PROVIDER=openai" >> backend/.env
docker-compose restart backend
./test_ai_providers.sh

# Test Gemini
sed -i '' 's/AI_PROVIDER=.*/AI_PROVIDER=gemini/' backend/.env
docker-compose restart backend
./test_ai_providers.sh

# Test Perplexity
sed -i '' 's/AI_PROVIDER=.*/AI_PROVIDER=perplexity/' backend/.env
docker-compose restart backend
./test_ai_providers.sh
```

### Use in Your Code

The AI service is already integrated! Just use the existing endpoints:

```bash
# Suggest skills
curl -X POST http://localhost:8000/api/v1/ai/suggest-skills \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "diagnosed_gap": "Students describe but cannot infer purpose",
    "subject": "History",
    "level": "Sec 3"
  }'

# Suggest steps
curl -X POST http://localhost:8000/api/v1/ai/suggest-steps \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "diagnosed_gap": "Cannot infer purpose",
    "skill_label": "Visual source inference",
    "bloom_level": "Analyze",
    "num_steps": 3
  }'

# Evaluate response
curl -X POST http://localhost:8000/api/v1/ai/evaluate-response \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "step_prompt": "What is the intended audience?",
    "student_response": "Young men to join military",
    "correct_criteria": "Identifies audience and explains visual elements",
    "scaffold_level": "full"
  }'
```

---

## Cost Comparison

For 1000 students completing 10 activities each:

| Provider | Model | Estimated Cost |
|----------|-------|----------------|
| Gemini | gemini-1.5-flash | **FREE** (within limits) |
| Gemini | gemini-1.5-pro | ~$12 |
| OpenAI | gpt-4o-mini | ~$75 |
| Perplexity | sonar-small | ~$20 |
| OpenAI | gpt-4o | ~$1,250 |
| Claude | sonnet | ~$1,800 |

**Recommendation:** Start with Gemini free tier, evaluate quality, then decide.

---

## Documentation

- **Quick Setup:** [QUICK_AI_SETUP.md](QUICK_AI_SETUP.md)
- **Detailed Guide:** [AI_PROVIDERS.md](AI_PROVIDERS.md)
- **Getting Started:** [START_HERE.md](START_HERE.md)
- **Full README:** [README.md](README.md)

---

## Troubleshooting

**Q: Can I use multiple providers at once?**
A: Not simultaneously, but you can switch anytime by changing `AI_PROVIDER` in `.env`

**Q: Will switching providers affect my data?**
A: No! All activities, users, and progress are stored in the database independently.

**Q: Which provider should I use?**
A: For development: Gemini (free). For production: OpenAI (reliable) or Gemini (cost-effective).

**Q: Can I add my own provider?**
A: Yes! See the "Advanced: Custom Provider" section in [AI_PROVIDERS.md](AI_PROVIDERS.md)

**Q: Do I need to change my code?**
A: No! Just change environment variables and restart.

---

## Next Steps

1. **Try it now:** Follow [QUICK_AI_SETUP.md](QUICK_AI_SETUP.md)
2. **Test providers:** Run `./test_ai_providers.sh`
3. **Compare quality:** Try the same prompt with different providers
4. **Choose best fit:** Balance cost, quality, and features
5. **Build your ITS:** Use the AI endpoints in your activities

---

## Feedback

Found an issue? Have a suggestion? Let us know!

**Happy building! 🚀**
