# Dr.Research - AI-Powered Research Agent

An autonomous research agent that decomposes complex topics, searches the web, extracts insights, and generates comprehensive research reports using advanced AI.

## Features

- 🤖 **AI-Powered Analysis** - Advanced language models decompose topics and synthesize insights
- � **Deep Web Research** - Searches 20+ authoritative sources for comprehensive information
- � **Structured Reports** - Generates professional reports with citations and statistics
- ⚡ **Real-time Progress** - Watch the research pipeline work with live status updates
- 👤 **User Authentication** - Secure signup/login system
- � **Session Management** - Track and manage multiple research sessions

## Tech Stack

**Backend:**
- FastAPI (Python)
- LangGraph for AI orchestration
- Server-Sent Events (SSE) for real-time updates
- CSV-based user storage

**Frontend:**
- React with Vite
- TailwindCSS for styling
- Lucide icons
- Real-time pipeline visualization

## Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/Kartikeysharma1972/Dr.Research.git
   cd Dr.Research
   ```

2. **Backend Setup**
   ```bash
   cd backend
   python -m venv venv
   venv\Scripts\activate  # Windows
   pip install -r requirements.txt
   python main.py
   ```

3. **Frontend Setup**
   ```bash
   cd frontend
   npm install
   npm run dev
   ```

4. **Access the Application**
   - Frontend: http://localhost:5173
   - Backend API: http://localhost:8000

## Docker Deployment

```bash
docker-compose up
```

## How It Works

1. **Planner** - Breaks your topic into sub-questions
2. **Agent** - Searches 20+ web sources for information
3. **Extractor** - Pulls and synthesizes key findings
4. **Refinement** - Self-reflects and refines the final report

## API Endpoints

- `POST /research` - Start a new research session
- `GET /stream/{session_id}` - Real-time SSE stream for research progress
- `GET /report/{session_id}` - Get final research report
- `POST /auth/signup` - User registration
- `POST /auth/login` - User authentication

## Environment Variables

Create a `.env` file in the backend directory:
```
CORS_ORIGINS=http://localhost:5173
# Add your API keys for web search and AI services
```

## License

This project is licensed under the MIT License.
