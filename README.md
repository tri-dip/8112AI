8112.AI - Consumer Health Decision Engine
8112.AI is an AI-native health co-pilot designed to help consumers make safer dietary decisions. By combining Computer Vision with Large Language Models (LLMs), the system analyzes food labels, ingredients, and nutritional facts against a user's specific health profile (allergies, conditions, and goals) to provide real-time safety verdicts.

📸 Screenshots
(Place your screenshots here, e.g., screenshots/interface.png)

✨ Features
⚡ Real-Time Health Analysis: Instant "Safe", "Caution", or "Avoid" verdicts based on user constraints.

👁️ Visual Label Scanning: Upload images of food packaging; the AI extracts text and analyzes ingredients.

🧬 Dynamic User Profiling: Context-aware responses considering allergies (e.g., Peanuts), conditions (e.g., Hypothyroidism), and goals (e.g., Muscle Gain).

🧠 "Thinking" UI: Modern, transparent AI reasoning process that shows the user exactly what the agent is analyzing before delivering the verdict.

🌐 Integrated Research: The agent performs live web searches to cross-reference safety data for obscure ingredients.

🛠️ Tech Stack
Frontend
Framework: React (Vite/Next.js)

Styling: Tailwind CSS

Animations: Framer Motion (for the thinking/loading states)

Icons: Lucide React

Backend
Framework: FastAPI

Language: Python 3.9+

Validation: Pydantic

Server: Uvicorn

AI & LLM
Model: Google Gemini 1.5 Pro / Flash (Multimodal)

Agent Logic: Custom Agent State (Plan, Search, Reasoning, Verdict)

Search Tool: (Optional: Tavily / Google Search API)

📂 Project Structure
Bash

8112-ai/
├── frontend/          # React Application
│   ├── src/
│   │   ├── components/# UI Components (ThinkingIndicator, VerdictBadge)
│   │   ├── App.tsx    # Main Logic (DrishtiAI component)
│   │   └── ...
│   ├── tailwind.config.js
│   └── package.json
│
├── backend/           # FastAPI Server
│   ├── main.py        # App entry point & endpoints
│   ├── agent.py       # LLM Logic & Prompt Engineering
│   ├── models.py      # Pydantic Data Models
│   ├── requirements.txt
│   └── .env           # API Keys
│
└── README.md
🚀 Getting Started
1. Prerequisites
Node.js (v18+)

Python (v3.9+)

A Google Gemini API Key (Get one here)

2. Backend Setup
Navigate to the backend directory:

Bash

cd backend
Create a virtual environment and install dependencies:

Bash

# Windows
python -m venv venv
venv\Scripts\activate

# Mac/Linux
python3 -m venv venv
source venv/bin/activate

# Install requirements
pip install -r requirements.txt
Create a .env file in the backend folder:

Code snippet

GOOGLE_API_KEY=your_gemini_api_key_here
# If you are using search:
# TAVILY_API_KEY=your_tavily_key
Run the server:

Bash

uvicorn main:app --reload --port 8000
The backend is now running at http://localhost:8000

3. Frontend Setup
Open a new terminal and navigate to the frontend directory:

Bash

cd frontend
Install dependencies:

Bash

npm install
Run the development server:

Bash

npm run dev
The frontend is now running at http://localhost:5173 (or 3000)

🔌 API Reference
POST /process
This is the main endpoint that handles both text queries and image analysis.

Request Body (JSON):

JSON

{
  "user_query": "Can I eat this if I have hypothyroidism?",
  "user_profile": {
    "allergies": ["Peanuts"],
    "conditions": ["Hypothyroidism"],
    "goals": ["Reduce Inflammation"]
  },
  "image_data": "data:image/jpeg;base64,/9j/4AAQSkZJRg..." // Optional Base64 string
}
Response (JSON):

JSON

{
  "plan": "1. Analyze ingredients. 2. Check goitrogen content...",
  "final_verdict": "CAUTION",
  "reasoning": "Contains soy, which may interfere with thyroid medication...",
  "search_results": "Soy isoflavones can inhibit...",
  "product_json": {
    "IngredientList": ["Soy Protein", "Salt"],
    "NutritionFacts": {"Sodium": "500mg"}
  },
  "next_suggestion": ["Check specific soy content", "Look for alternative"]
}
🤝 Contributing
Contributions are welcome! Please follow these steps:

Fork the repository.

Create a feature branch (git checkout -b feature/NewFeature).

Commit your changes.

Push to the branch.

Open a Pull Request.

📄 License
This project is licensed under the MIT License - see the LICENSE file for details.

Developed by Tridip Kalita