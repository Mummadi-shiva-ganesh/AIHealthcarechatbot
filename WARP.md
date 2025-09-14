# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

HealthBot is an AI-driven health chatbot designed for rural and semi-urban populations. It's a full-stack application with a React frontend and Flask backend, featuring multilingual support and integration with Google's Gemini AI.

## Architecture

### Backend (Flask)
- **Main Application**: `app.py` - Complete Flask app with all routes and database models
- **Launcher**: `run.py` - Application startup script with environment validation
- **Database**: SQLAlchemy with SQLite (dev) / PostgreSQL (production)
- **AI Integration**: Google Gemini API for health consultations
- **Authentication**: JWT-based auth with Flask-JWT-Extended
- **Key Models**: User, ChatHistory, Disease, VaccinationSchedule, OutbreakAlert

### Frontend (React)
- **Location**: `client/` directory
- **Architecture**: React with Context API for state management
- **Routing**: React Router with protected routes
- **Styling**: Styled Components with responsive design
- **Key Context**: `AuthContext.js` handles authentication flow
- **Pages**: Home, Dashboard, Chat, Diseases, Vaccination, Alerts, Profile

### Database Schema
- **Users**: Authentication and profile data with multilingual preferences
- **ChatHistory**: Conversation logs with language tracking
- **Disease**: Comprehensive disease information with symptoms/treatments
- **VaccinationSchedule**: Age-based vaccination recommendations
- **OutbreakAlert**: Location-based health alerts

## Development Commands

### Initial Setup
```bash
# Complete environment setup (use appropriate script for your OS)
# Windows
start.bat

# Linux/Mac
./start.sh

# Manual setup
python test_setup.py  # Validates environment
cp env.example .env    # Configure environment variables
pip install -r requirements.txt
cd client && npm install
```

### Backend Development
```bash
# Start backend server
python run.py

# Alternative npm script
npm start

# Development server with auto-reload
python app.py  # If you need direct Flask development
```

### Frontend Development
```bash
# Start React development server
cd client && npm start

# Or from root directory
npm run client

# Build for production
cd client && npm run build
# Or from root
npm run build
```

### Testing
```bash
# Environment and dependency validation
python test_setup.py

# This validates:
# - Python version compatibility
# - Required packages installation
# - Environment variables configuration
# - Database connectivity
# - Gemini AI API connection
# - Node.js and client dependencies
```

### Database Operations
```bash
# Database is auto-created on first run via run.py
# Manual database initialization (if needed)
python -c "from app import app, db; app.app_context().push(); db.create_all()"

# The application auto-populates disease data from the disease_data dictionary in app.py
```

## Environment Configuration

### Required Environment Variables (.env file)
```bash
# Core Configuration
DATABASE_URL=sqlite:///health_chatbot.db  # SQLite for dev
GEMINI_API_KEY=your_gemini_api_key        # Required for AI functionality
JWT_SECRET_KEY=your_secret_key            # Required for authentication

# Optional Configuration
PORT=5000                                 # Server port
FLASK_ENV=development                     # Environment mode
MAX_CONTENT_LENGTH=16777216              # File upload limit
UPLOAD_FOLDER=uploads                    # Upload directory
```

### Frontend Environment Variables
The React app uses `REACT_APP_API_URL` for backend communication, configured in Vercel dashboard or locally via proxy in `client/package.json`.

## Key Development Patterns

### API Structure
- All API endpoints are prefixed with `/api/`
- JWT authentication required for protected routes
- Consistent JSON response format with error handling
- CORS enabled for frontend communication

### Frontend State Management
- `AuthContext` provides global authentication state
- JWT tokens stored in localStorage
- Axios configured with automatic token attachment
- Toast notifications for user feedback

### AI Integration Pattern
- Gemini API calls wrapped with fallback responses
- Structured prompts for consistent formatting
- Chat responses formatted with markdown-like syntax
- Comprehensive error handling for AI service unavailability

### Database Patterns
- SQLAlchemy ORM with relationship definitions
- Automatic timestamp tracking on models
- Soft deletes using is_active flags where applicable
- Comprehensive disease data pre-loaded in application code

## Deployment Architecture

### Current Deployment
- **Frontend**: Vercel (configured via `vercel.json`)
- **Backend**: Can be deployed to Heroku, Railway, or similar platforms
- **Database**: SQLite for development, PostgreSQL recommended for production

### Deployment Commands
```bash
# Frontend deployment (Vercel)
cd client && vercel

# Backend deployment varies by platform
# See DEPLOYMENT.md for comprehensive deployment instructions
```

## Important Files and Directories

### Configuration Files
- `env.example` - Template for environment variables
- `vercel.json` - Vercel frontend deployment configuration
- `requirements.txt` - Python dependencies
- `client/package.json` - React dependencies and scripts

### Key Backend Files
- `app.py` - Main Flask application with all routes and models
- `run.py` - Application launcher with environment validation
- `test_setup.py` - Comprehensive setup validation script

### Key Frontend Files
- `client/src/contexts/AuthContext.js` - Authentication state management
- `client/src/App.js` - Main application routing and structure
- `client/src/pages/Chat.js` - AI chat interface with custom message rendering

### Helper Scripts
- `start.bat` / `start.sh` - Cross-platform application startup
- `DEPLOYMENT.md` - Comprehensive deployment documentation

## Development Notes

### AI Service Integration
- Gemini API integration includes comprehensive fallback mechanisms
- Chat responses are formatted with structured markdown for consistent UI rendering
- The system gracefully handles API failures with informative fallback responses

### Frontend Responsive Design
- Mobile-first approach with extensive media queries
- Styled Components used for dynamic styling
- Framer Motion for smooth animations and transitions

### Database Considerations
- Disease data is embedded in the application code (disease_data dictionary)
- Chat history is preserved with language tracking
- User profiles include multilingual preferences

### Error Handling Strategy
- Comprehensive try-catch blocks in all API routes
- User-friendly error messages with toast notifications
- Fallback responses when external services are unavailable

This codebase follows a monorepo structure with clear separation between frontend and backend concerns, comprehensive error handling, and production-ready deployment configurations.
