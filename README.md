# 8112.AI - Consumer Health Decision Engine

![Project Status](https://img.shields.io/badge/Status-Beta-blue)
![Frontend](https://img.shields.io/badge/Frontend-React_%7C_Tailwind-61DAFB)
![Backend](https://img.shields.io/badge/Backend-FastAPI-009688)
![AI](https://img.shields.io/badge/AI-Gemini_Pro_Vision-8E75B2)

8112.AI is an AI-native health co-pilot designed to help consumers make safer dietary decisions. By combining Computer Vision with Large Language Models (LLMs), the system analyzes food labels, ingredients, and nutritional facts against a user's specific health profile (allergies, conditions, and goals) to provide real-time safety verdicts.



## Features

* **Real-Time Health Analysis:** Instant "Safe", "Caution", or "Avoid" verdicts based on strict user constraints.
* **Visual Label Scanning:** Users can upload images of food packaging; the AI extracts text, reads nutrition tables, and analyzes ingredients.
* **Dynamic User Profiling:** Context-aware responses that consider specific allergies (e.g., Peanuts), health conditions (e.g., Hypothyroidism), and fitness goals (e.g., Muscle Gain).
* **Transparent Reasoning Engine:** A modern UI that displays the AI's "Thinking" process step-by-step before delivering the final verdict, increasing user trust.
* **Integrated Web Research:** The agent performs live web searches to cross-reference safety data for obscure ingredients or conflicting information.

## Tech Stack

### Frontend
* **Framework:** React (Vite ecosystem)
* **Styling:** Tailwind CSS
* **Animations:** Framer Motion (used for the thinking state and result reveals)
* **Icons:** Lucide React

### Backend
* **Framework:** FastAPI
* **Language:** Python 3.9+
* **Validation:** Pydantic Data Models
* **Server:** Uvicorn

### AI and LLM
* **Model:** Google Gemini 1.5 Pro or Flash (Multimodal capabilities)
* **Agent Logic:** Custom Agent State (Plan, Search, Reasoning, Verdict)
* **Search Integration:** Optional integration with Tavily or Google Search API

## Project Structure

```text
8112-ai/
├── frontend/          # React Application
│   ├── src/
│   │   ├── components/# UI Components (ThinkingIndicator, VerdictBadge)
│   │   ├── App.tsx    # Main Application Logic
│   │   └── ...
│   ├── tailwind.config.js
│   └── package.json
│
├── backend/           # FastAPI Server
│   ├── main.py        # App entry point and API endpoints
│   ├── agent.py       # LLM Logic, Prompt Engineering, and Chain
│   ├── models.py      # Pydantic Data Models for Request/Response
│   ├── requirements.txt
│   └── .env           # Environment Variables (API Keys)
│
└── README.md