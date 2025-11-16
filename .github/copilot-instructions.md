# OctoFit Tracker - AI Coding Agent Instructions

## Project Overview
**OctoFit Tracker** is a Django + React fitness application built for Mergington High School to enable student activity logging, gamification, and team-based competition. The app includes user authentication, activity tracking, team management, leaderboards, and personalized recommendations.

## Architecture & Key Components

### Directory Structure
```
octofit-tracker/
├── backend/                           # Django REST API
│   ├── venv/                         # Python virtual environment
│   ├── requirements.txt              # Python dependencies
│   └── octofit_tracker/              # Django project root
│       ├── manage.py
│       ├── settings.py               # Must include ALLOWED_HOSTS config
│       ├── urls.py                   # Must use CODESPACE_NAME variable
│       └── management/commands/      # Custom management commands
└── frontend/                          # React app
    └── src/
        └── index.js                  # Must import bootstrap CSS
```

### Tech Stack
- **Backend**: Django 4.1.7, Django REST Framework, Djongo (MongoDB ORM)
- **Database**: MongoDB (via djongo, no authentication), collections: users, teams, activities, leaderboard, workouts
- **Frontend**: React with Bootstrap, react-router-dom
- **Ports**: 8000 (Django, public), 3000 (React, public), 27017 (MongoDB, private)

## Critical Development Workflows

### Backend Setup (Never change directories - always reference full paths)
```bash
# Activate Python environment
source octofit-tracker/backend/venv/bin/activate

# Install dependencies (only once)
pip install -r octofit-tracker/backend/requirements.txt

# Run migrations
python octofit-tracker/backend/manage.py migrate

# Check MongoDB status
ps aux | grep mongod

# Populate test data (after migrations)
python octofit-tracker/backend/manage.py populate_db
```

### Frontend Setup
```bash
# Dependencies are installed via npm in octofit-tracker/frontend
npm install --prefix octofit-tracker/frontend

# Bootstrap CSS must be imported at top of src/index.js:
# import 'bootstrap/dist/css/bootstrap.min.css';
```

### Testing Endpoints
- Use `curl` to test REST API endpoints
- Base URL depends on environment:
  - Codespace: `https://{CODESPACE_NAME}-8000.app.github.dev`
  - Local: `http://localhost:8000`

## Project-Specific Patterns & Conventions

### Django Settings Configuration
Always include in `settings.py`:
```python
import os
ALLOWED_HOSTS = ['localhost', '127.0.0.1']
if os.environ.get('CODESPACE_NAME'):
    ALLOWED_HOSTS.append(f"{os.environ.get('CODESPACE_NAME')}-8000.app.github.dev")
```

Enable CORS for all origins and methods to allow React frontend communication.

### Django URL Configuration
Always build URLs using CODESPACE_NAME environment variable:
```python
import os
codespace_name = os.environ.get('CODESPACE_NAME')
if codespace_name:
    base_url = f"https://{codespace_name}-8000.app.github.dev"
else:
    base_url = "http://localhost:8000"
```

### Serializers
- Convert MongoDB `ObjectId` fields to strings in serializers
- Ensure API responses are JSON-serializable

### Database Access
- **Always use Django ORM (models/serializers)**, never direct MongoDB scripts
- Use `djongo` for MongoDB integration through Django models
- Test data: Uses superhero names with teams "marvel" and "dc"
- Email field must have unique index in MongoDB

### Management Commands
Location: `octofit-tracker/backend/octofit_tracker/management/commands/`
Example: `populate_db.py` for test data initialization
- Use Django ORM for all data operations
- Include help message describing command purpose

## Cross-Component Communication

### Frontend-Backend Integration
- React frontend (port 3000) calls Django API (port 8000)
- CORS must be fully enabled in Django settings
- Base URL uses environment variable for portability

### Agent Mode Rules
- **Never change directories** - always use full relative paths
- Point to specific directories when issuing commands
- Use `source` to activate Python venv, not subshell commands
- Prefer sequential operations over bash shortcuts when working with venv

## Image Assets
- OctoFit branding image: `docs/octofitapp-small.png`
- Use this for UI/branding in React frontend

## Key Files to Reference
- Backend setup guide: `.github/instructions/octofit_tracker_setup_project.instructions.md`
- Django patterns: `.github/instructions/octofit_tracker_django_backend.instructions.md`
- React patterns: `.github/instructions/octofit_tracker_react_frontend.instructions.md`
- Database initialization prompt: `.github/prompts/init-populate-octofit_db.prompt.md`
- Project story/requirements: `docs/octofit_story.md`
