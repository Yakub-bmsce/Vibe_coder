# Visual Logic Translator

A web-based educational tool that helps B.Tech AI/DS students understand algorithms by translating plain language descriptions into Python code with visual flowcharts.

## Overview

The Visual Logic Translator bridges the gap between conceptual understanding and code implementation. Students often get lost in programming syntax and miss the underlying logic of AI/DS concepts. This tool allows students to describe algorithms in plain language (including local languages like Hindi, Tamil, Telugu, Bengali) and receive:

- **Executable Python code** with clear comments
- **Visual flowcharts** showing the algorithm flow
- **Educational explanations** connecting concepts to implementation

## Features

- **Multi-language Support**: Describe algorithms in English or local languages
- **AI/DS Focus**: Recognizes machine learning concepts (linear regression, k-means, decision trees, etc.)
- **Code Generation**: Produces clean, PEP 8 compliant Python code with appropriate libraries
- **Visual Flowcharts**: Automatic flowchart generation using Mermaid.js
- **Interactive Output**: Syntax highlighting, copy-to-clipboard, download options
- **Translation History**: Save and review previous translations
- **Educational Explanations**: Step-by-step breakdowns of how code works

## Technology Stack

### Backend
- **Python 3.10+** with FastAPI framework
- **OpenAI API** (or similar LLM) for code generation
- **PostgreSQL** for history storage
- **SQLAlchemy** for database ORM
- **langdetect** for language detection
- **Google Translate API** for multi-language support

### Frontend
- **React** with TypeScript
- **Monaco Editor** for code display
- **Mermaid.js** for flowchart rendering
- **Axios** for API communication

### Development
- **Docker** for containerization
- **pytest** with Hypothesis for property-based testing
- **Black** for code formatting
- **Uvicorn** as ASGI server

## Getting Started

### Prerequisites

- Python 3.10 or higher
- Node.js 16+ and npm
- PostgreSQL 14+
- OpenAI API key (or alternative LLM service)
- Google Translate API key (or alternative translation service)

### Installation

1. **Clone the repository**
```bash
git clone <repository-url>
cd visual-logic-translator
```

2. **Set up backend**
```bash
cd backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```

3. **Set up frontend**
```bash
cd frontend
npm install
```

4. **Configure environment variables**

Create a `.env` file in the backend directory:
```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/visual_logic_translator

# LLM Service
OPENAI_API_KEY=your_openai_api_key_here
OPENAI_MODEL=gpt-4

# Translation Service
GOOGLE_TRANSLATE_API_KEY=your_google_translate_key_here

# Security
JWT_SECRET_KEY=your_secret_key_here
JWT_ALGORITHM=HS256

# Server
API_HOST=0.0.0.0
API_PORT=8000
CORS_ORIGINS=http://localhost:3000

# Rate Limiting
RATE_LIMIT_PER_MINUTE=10
```

5. **Initialize database**
```bash
cd backend
python -m alembic upgrade head
```

### Running the Application

**Development Mode:**

1. Start the backend:
```bash
cd backend
uvicorn app.main:app --reload --port 8000
```

2. Start the frontend:
```bash
cd frontend
npm start
```

3. Open your browser to `http://localhost:3000`

**Using Docker:**

```bash
docker-compose up
```

## Usage

1. **Enter a description**: Type or paste an algorithm description in plain language
2. **Select language** (optional): The system auto-detects, but you can specify
3. **Submit**: Click "Translate" to generate code and flowchart
4. **Review output**: 
   - View generated Python code with syntax highlighting
   - See the visual flowchart
   - Read the educational explanation
5. **Interact**:
   - Copy code to clipboard
   - Download code as .py file
   - Download flowchart as image
   - Save to history for later review

### Example Descriptions

**English:**
```
Create a linear regression model that predicts house prices based on square footage. 
Load sample data, split into training and testing sets, train the model, and evaluate accuracy.
```

**Hindi:**
```
एक k-means क्लस्टरिंग एल्गोरिथ्म बनाएं जो ग्राहकों को तीन समूहों में विभाजित करे।
```

**Simple Algorithm:**
```
Write a function that finds the maximum value in a list of numbers using a loop.
```

## API Documentation

Once the backend is running, visit `http://localhost:8000/docs` for interactive API documentation (Swagger UI).

### Main Endpoints

- `POST /api/translate` - Translate description to code and flowchart
- `GET /api/history` - Retrieve user's translation history
- `GET /api/translation/:id` - Get specific translation by ID
- `DELETE /api/translation/:id` - Delete translation from history
- `POST /api/execute` - Execute generated code (optional, sandboxed)

## Development

### Project Structure

```
visual-logic-translator/
├── backend/
│   ├── app/
│   │   ├── main.py              # FastAPI application
│   │   ├── api/                 # API routes
│   │   ├── core/                # Core components
│   │   │   ├── language_processor.py
│   │   │   ├── code_generator.py
│   │   │   ├── visualization_generator.py
│   │   │   ├── explanation_generator.py
│   │   │   └── history_store.py
│   │   ├── models/              # Data models
│   │   ├── schemas/             # Pydantic schemas
│   │   └── utils/               # Utilities
│   ├── tests/                   # Test suite
│   ├── requirements.txt
│   └── .env
├── frontend/
│   ├── src/
│   │   ├── components/          # React components
│   │   ├── services/            # API client
│   │   ├── types/               # TypeScript types
│   │   └── App.tsx
│   ├── package.json
│   └── tsconfig.json
├── docker-compose.yml
└── README.md
```

### Running Tests

**Backend tests:**
```bash
cd backend
pytest tests/ -v
```

**Property-based tests:**
```bash
pytest tests/property/ -v --hypothesis-show-statistics
```

**Frontend tests:**
```bash
cd frontend
npm test
```

### Code Quality

**Format code:**
```bash
black backend/app
```

**Lint code:**
```bash
flake8 backend/app
pylint backend/app
```

**Type checking:**
```bash
mypy backend/app
```

## Supported AI/DS Concepts

The system recognizes and generates appropriate implementations for:

- **Supervised Learning**: Linear Regression, Logistic Regression, Decision Trees, Random Forests, SVM, KNN
- **Unsupervised Learning**: K-Means Clustering, Hierarchical Clustering, PCA, DBSCAN
- **Data Processing**: Normalization, Standardization, One-Hot Encoding, Feature Scaling
- **Evaluation**: Train-Test Split, Cross-Validation, Confusion Matrix, Accuracy, Precision, Recall
- **Data Structures**: Arrays, Lists, Trees, Graphs, Hash Tables
- **Algorithms**: Sorting, Searching, Dynamic Programming, Greedy Algorithms

## Supported Languages

- English (en)
- Hindi (hi)
- Tamil (ta)
- Telugu (te)
- Bengali (bn)

Technical terms (AI/DS concepts) are preserved during translation to maintain accuracy.

## Security

- Input validation prevents code injection
- Generated code is scanned for dangerous operations
- Rate limiting (10 requests/minute per user)
- JWT-based authentication
- Sandboxed code execution (optional feature)
- No file system or network access in generated code

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Follow PEP 8 for Python code
- Use TypeScript for frontend code
- Write tests for new features
- Update documentation as needed
- Ensure all tests pass before submitting PR

## Troubleshooting

**Issue: Code generation fails**
- Check OpenAI API key is valid and has credits
- Verify API rate limits haven't been exceeded
- Check logs for specific error messages

**Issue: Translation not working**
- Verify Google Translate API key is configured
- Check supported language codes match input
- Review API quota limits

**Issue: Database connection errors**
- Ensure PostgreSQL is running
- Verify DATABASE_URL in .env is correct
- Check database user has proper permissions

**Issue: Frontend can't connect to backend**
- Verify backend is running on correct port
- Check CORS_ORIGINS includes frontend URL
- Review browser console for specific errors

## License

[Specify your license here]

## Acknowledgments

- Built for B.Tech AI/DS students
- Inspired by the need to focus on logic over syntax
- Uses OpenAI GPT models for code generation
- Flowcharts powered by Mermaid.js

## Contact

[Your contact information or project maintainer details]

## Roadmap

Future enhancements planned:
- Support for more programming languages (Java, JavaScript, C++)
- Interactive code editing with live flowchart updates
- Mobile application
- Voice input for descriptions
- Integration with Jupyter notebooks
- Gamification features
- LMS integration

---

**Note**: This is an educational tool designed to help students learn. Generated code should be reviewed and understood before use in production environments.
