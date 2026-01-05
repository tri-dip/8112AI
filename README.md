# 8112.AI — Consumer Health Decision Engine

![Status](https://img.shields.io/badge/Status-Beta-blue)
![Frontend](https://img.shields.io/badge/Frontend-React_%7C_Tailwind-61DAFB)
![Backend](https://img.shields.io/badge/Backend-FastAPI-009688)

8112.AI is an AI-native consumer health co-pilot designed to help users make safer dietary decisions.
The system analyzes food labels, ingredients, and nutritional data against a user’s allergies, medical conditions, and health goals, and produces a safety verdict such as SAFE, CAUTION, AVOID, or INFO.

The platform integrates OCR-based label extraction, LLM-driven reasoning, and a transparent decision workflow.

---

### Live Site

**[Open Website](https://ai-health-udgam-ui.vercel.app)**


## Features

- Real-time safety verdicts based on user profiles
- Food label scanning with ingredient and nutrition extraction
- Transparent reasoning and decision workflow
- Optional research-aware analysis for uncertain or condition-specific queries
- Dynamic health profile persistence across interactions

---

---

## System Architecture

The 8112.AI system follows an agent-style processing pipeline. The user interacts through the frontend, and requests are processed by the FastAPI backend, which coordinates OCR extraction, LLM reasoning, optional web search, and verdict generation.

### High-Level Pipeline

    User Input
        |
        v
    React Frontend (UI)
        |
        v
    FastAPI Backend (Agent Controller)
        |
        |--- If image provided → OCR + Product Parser
        |
        v
    LLM Reasoning (Gemini)

        |
        |--- If LLM has sufficient context:
        |        • Generate reasoning
        |        • Produce verdict (SAFE / CAUTION / AVOID / INFO)
        |
        |--- Else (uncertain or missing knowledge):
        |        • Trigger Web Search (Exa.ai)
        |        • Retrieve supporting evidence
        |        • Synthesize search-grounded response
        |
        v
    Final Decision Layer
        |
        v
    Response Returned to Frontend
        |
        v
    UI Displays
        • Thinking / Plan Steps
        • Product Data (if scanned)
        • Research Findings (if used)
        • Verdict + Explanation
        • Suggested Next Steps


### Component Roles

- **Frontend (React UI)**  
  Handles interactions, displays agent thinking, reasoning, and verdict output.

- **FastAPI Backend**  
  Acts as the control layer for agent logic and data flow.

- **LLM Engine (Gemini)**  
  Performs intent classification, reasoning synthesis, and verdict generation.

- **OCR + Parsing Module (Image Inputs Only)**  
  Extracts ingredients, nutrition values, and product context from labels.

- **Web Search via Exa.ai (Fallback Mode)**  
  Activated when:
  - the query involves unfamiliar substances,
  - safety concerns require evidence,
  - or condition-specific validation is needed.

  The retrieved information is fed back into the LLM for grounded reasoning.

- **Decision & Output Layer**  
  Produces:
  - verdict,
  - reasoning summary,
  - actionable follow-up suggestions.

---

## Architecture Summary

The design follows a **“Reason → Retrieve → Refine” AI-agent pattern**:

1. **Reason first** using prior knowledge  
2. **Retrieve evidence only when needed** via Exa.ai  
3. **Refine final verdict** using grounded information

This approach improves:
- safety,
- transparency,
- reliability,
- and interpretability of decisions.
 
---

## Tech Stack

### Frontend
- React + TypeScript (Vite)
- Tailwind CSS
- Framer Motion
- Lucide Icons

### Backend
- FastAPI
- Pydantic
- Uvicorn

### Reasoning and Action Pipelines
- OpenCV
- EXA.AI
- Gemini models (text reasoning and parsing)
- OCR-driven ingredient and nutrition extraction pipeline

---

## Project Structure
```text
8112AI/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── hooks/
│   │   ├── lib/
│   │   ├── assets/
│   │   ├── styles/
│   │   ├── App.tsx
│   │   └── main.tsx
│   ├── public/
│   ├── package.json
│   └── vite.config.ts
│
├── backend/
│   ├── app/
│   │   ├── api/
│   │   │   ├── __init__.py
│   │   │   ├── routes_health.py
│   │   │   └── routes_agent.py
│   │   ├── core/
│   │   │   ├── __init__.py
│   │   │   ├── llm.py
│   │   │   ├── prompts.py
│   │   │   ├── search.py
│   │   │   └── reasoning.py
│   │   ├── cv/
│   │   │   ├── __init__.py
│   │   │   ├── cv_pipeline.py
│   │   │   ├── image_enhancement.py
│   │   │   └── ocr.py
│   │   ├── models/
│   │   │   ├── __init__.py
│   │   │   ├── product.py
│   │   │   └── agent_state.py
│   │   ├── services/
│   │   │   ├── __init__.py
│   │   │   ├── agent_service.py
│   │   │   └── product_parser.py
│   │   ├── utils/
│   │   │   ├── __init__.py
│   │   │   ├── config.py
│   │   │   └── logger.py
│   │   ├── main.py
│   │   └── __init__.py
│   ├── tests/
│   ├── requirements.txt
│   └── run.sh
│
├── README.md
```

## Backend Setup

### Prerequisites

- Python 3.10 or higher
- Google Gemini API key

### Installation

    cd backend
    python -m venv venv
    source venv/bin/activate    # Windows: venv\Scripts\activate
    pip install -r requirements.txt

Create a .env file:

    GOOGLE_API_KEY=your_api_key_here

Start the server:

    uvicorn main:app --reload --port 8000

The backend runs at:

    http://localhost:8000

---

## Frontend Setup

    cd frontend
    npm install
    npm run dev

The frontend runs at:

    http://localhost:3000

---

## API — /process

Method: POST  
Executes the agent reasoning pipeline and returns a safety verdict.

### Request Example

    {

    "user_query": "I'm currently cleaning out my dusty attic and suddenly my    chest feels very tight. I am wheezing loudly, my throat feels scratchy, and my 'rescue' inhaler isn't providing the usual relief. Should I wait for the antihistamine to kick in or take another puff?",

    "user_profile": {
        "allergies": ["dust mites", "mold", "cat dander"],
        "conditions": ["Exercise-induced asthma", "Chronic Sinusitis"],
        "goals": ["minimize reliance on steroid inhalers", "identify environmental triggers"]
    },

    "image_data": null
    }

### Response Example

    {
    "user_query": "I'm currently cleaning out my dusty attic and suddenly my chest feels very tight. I am wheezing loudly, my throat feels scratchy, and my 'rescue' inhaler isn't providing the usual relief. Should I wait for the antihistamine to kick in or take another puff?",
    "user_profile": {
        "allergies": [
            "dust",
            "dust mites",
            "cat dander",
            "mold"
        ],
        "conditions": [
            "tight chest",
            "Chronic Sinusitis",
            "wheezing",
            "Exercise-induced asthma"
        ],
        "goals": [
            "minimize reliance on steroid inhalers",
            "identify environmental triggers"
        ]
    },
    "image_data": null,
    "image_path": null,
    "product_json": null,
    "plan": "The user is experiencing acute respiratory distress (tight chest, wheezing) triggered by dust (an existing allergy trigger) and reports that their rescue inhaler is ineffective, while also mentioning an antihistamine. This combination requires immediate safety analysis regarding symptom management, potential severity of an asthma exacerbation, and the decision to use rescue medication versus waiting for an antihistamine. This falls under specific safety protocols for medical conditions.",
    "search_needed": true,
    "search_queries": [
        "inhaler not working for asthma attack symptoms",
        "wheezing and tight chest relief when antihistamine is pending",
        "protocol for escalating asthma treatment when rescue inhaler is ineffective"
    ],
    "search_results": "If your reliever inhaler (e.g., albuterol) isn’t easing wheezing, shortness of breath, or chest tightness during an attack, sit upright, use a spacer or alternative device if available, and seek emergency medical help immediately (call 911 or your local emergency number) while considering that poor technique or other conditions (like vocal‑cord dysfunction) may be involved【7】([homehealthpatienteducation.com](https://homehealthpatienteducation.com/what-to-do-if-albuterol-isnt-working-when-to-seek-medical-help)),【3】([nhs.uk](https://www.nhs.uk/conditions/asthma))\n\nWheezing and a feeling of tightness in the chest are classic signs of airway narrowing that can occur during an asthma flare‑up or an allergic reaction. Antihistamines (e.g., loratadine, cetirizine, fexofenadine) are very good at blocking the histamine that drives sneezing, itching and some inflammation, but they **do not act as bronchodilators** and therefore do **not provide rapid relief** for acute bronchospasm. Healthline notes that antihistamines “will not have an effect on acute symptoms such as shortness of breath or chest tightness” and recommends a quick‑acting rescue inhaler such as albuterol for those episodes ([healthline](https://www.healthline.com/health/asthma/asthma-and-antihistamines)).  \n\nIf you are waiting for an oral antihistamine to take effect (usually 30‑60 minutes), you can use several interim measures that are supported by reputable sources. The Cleveland Clinic lists non‑drug strategies such as breathing exercises, drinking warm herbal tea, avoiding smoke, and using a HEPA‑filter air purifier to reduce irritation ([clevelandclinic](https://my.clevelandclinic.org/health/symptoms/15203-wheezing)). For allergy‑related chest tightness, newer second‑generation antihistamines are often effective once they are absorbed, as described by Wyndly (“over‑the‑counter antihistamines … are often effective in relieving chest tightness caused by allergic reactions”) ([wyndly](https://www.wyndly.com/blogs/learn/chest-pain)). However, they should be viewed as **preventive or adjunctive** therapy, not a substitute for rescue medication.  \n\nIn practice, the safest approach is to keep a rescue inhaler (or nebulized bronchodilator) on hand for immediate symptom control, employ breathing or environmental measures while the antihistamine works, and follow up with your clinician to confirm whether your wheeze is primarily allergic, asthmatic, or a combination of both. If wheezing or chest tightness persists despite these steps, seek medical attention promptly.\n\nBased on the British Thoracic Society/ SIGN acute‑asthma guidelines and the UK NHS/NIH escalation pathways, the following steps outline how to step‑up treatment when a rescue inhaler (SABA) is not providing relief ([GGC Medicines](https://handbook.ggcmedicines.org.uk/guidelines/respiratory-system/management-of-acute-severe-asthma-in-adults-in-hospital), [CG001 Acute Severe Asthma](https://static1.squarespace.com/static/54a40119e4b0fbd5ffb33811/t/602683d3604f5b6a2f77b1e6/1613136853953/CG001+Asthma+v2.pdf), [NHLBI](https://www.nhlbi.nih.gov/files/docs/guidelines/11_sec5_exacerb.pdf)):\n\n1. **Recognise failure of the rescue inhaler** – persistent wheeze, breathlessness, inability to speak full sentences, peak expiratory flow < 50 % predicted (or < 75 % for moderate) despite correct inhaler technique ([GGC Medicines](https://handbook.ggcmedicines.org.uk/guidelines/respiratory-system/management-of-acute-severe-asthma-in-adults-in-hospital)).  \n\n2. **Administer supplemental oxygen** – deliver controlled O₂ to keep SpO₂ 94‑98 % (avoid hypoxia; CO₂ retention is uncommon) ([GGC Medicines](https://handbook.ggcmedicines.org.uk/guidelines/respiratory-system/management-of-acute-severe-asthma-in-adult…)), also recommended in the CG001 protocol ([CG001](https://static1.squarespace.com/static/54a40119e4b0fbd5ffb33811/t/602683d3604f5b6a2f77b1e6/1613136853953/CG001+Asthma+v2.pdf)).  \n\n3. **Escalate bronchodilator therapy**  \n   - Nebulise salbutamol 5 mg (or terbutaline 10 mg) via an oxygen‑driven nebuliser; repeat every 15‑30 min ([GGC Medicines](https://handbook.ggcmedicines.org.uk/guidelines/respiratory-system/management-of-acute-severe-asthma-in-adult…)).  \n   - If response remains inadequate, start **continuous nebulisation** ≈ 10 mg hr⁻¹ (if device available) ([CG001](https://static1.squarespace.com/static/54a40119e4b0fbd5ffb33811/t/602683d3604f5b6a2f77b1e6/1613136853953/CG001+Asthma+v2.pdf)).  \n\n4. **Add anticholinergic bronchodilator** – ipratropium bromide 0.5 mg nebulised in O₂, repeat every 4 hours ([GGC Medicines](https://handbook.ggcmedicines.org.uk/guidelines/respiratory-system/management-of-acute-severe-asthma-in-adult…)), also listed in CG001 ([CG001](https://static1.squarespace.com/static/54a40119e4b0fbd5ffb33811/t/602683d3604f5b6a2f77b1e6/1613136853953/CG001+Asthma+v2.pdf)).  \n\n5. **Give systemic corticosteroid** – prednisolone 40‑50 mg PO (or hydrocortisone 100 mg IV if oral not possible) ([GGC Medicines](https://handbook.ggcmedicines.org.uk/guidelines/respiratory-system/management-of-acute-severe-asthma-in-adult…)), echoed in CG001 ([CG001](https://static1.squarespace.com/static/54a40119e4b0fbd5ffb33811/t/602683d3604f5b6a2f77b1e6/1613136853953/CG001+Asthma+v2.pdf)).  \n\n6. **Consider adjunctive agents if no improvement after steps 2‑5**  \n   - **IV magnesium sulfate** 2 g (≈ 8 mmol) diluted in 100 ml saline over 20 min, single dose ([CG001](https://static1.squarespace.com/static/54a40119e4b0fbd5ffb33811/t/602683d3604f5b6a2f77b1e6/1613136853953/CG001+Asthma+v2.pdf); [NHLBI](https://www.nhlbi.nih.gov/files/docs/guidelines/11_sec5_exacerb.pdf)).  \n   - **IV salbutamol infusion** 5 mg in 50 ml saline (100 µg ml⁻¹), start 3 ml hr⁻¹ (≈ 3 µg min⁻¹) and titrate up to 12 ml hr⁻¹ as tolerated ([CG001](https://static1.squarespace.com/static/54a40119e4b0fbd5ffb33811/t/602683d3604f5b6a2f77b1e6/1613136853953/CG001+Asthma+v2.pdf)).  \n   - **IV aminophylline** 5 mg kg⁻¹ loading dose (100 ml over 10‑15 min) followed by infusion 0.5 mg kg⁻¹ hr⁻¹ if needed ([CG001](https://static1.squarespace.com/static/54a40119e4b0fbd5ffb33811/t/602683d3604f5b6a2f77b1e6/1613136853953/CG001+Asthma+v2.pdf)).  \n\n7. **Life‑threatening features – immediate emergency measures**  \n   - If peak flow < 33 % predicted, SpO₂ < 92 %, silent chest, cyanosis, or altered consciousness, give **IM adrenaline 0.5 mg** (1:1000) ([CG001](https://static1.squarespace.com/static/54a40119e4b0fbd5ffb33811/t/602683d3604f5b6a2f77b1e6/1613136853953/CG001+Asthma+v2.pdf)).  \n   - Call emergency services (911/999) and arrange rapid transfer to an emergency department; do not leave the patient unattended ([First Aid for Asthma – Lung.org](https://lung.org/getmedia/8d0df46c-de43-46a9-b884-21aca244acee/first-aid-for-asthma.pdf)).  \n\n**Summary:** When a rescue inhaler fails, give oxygen, repeat/continuous nebulised SABA, add ipratropium, start systemic steroids, then progress to IV magnesium, IV salbutamol, or aminophylline, and use IM adrenaline plus emergency transfer for life‑threatening attacks.",
    "final_verdict": "CAUTION",
    "reasoning": "Your symptoms (tight chest, loud wheezing) are classic signs of an acute asthma flare-up, likely triggered by the dust in the attic. Since your rescue inhaler is not providing its usual relief, this indicates your airway narrowing is significant and potentially severe. Antihistamines, while helpful for allergy symptoms like throat scratchiness, **do not work on acute bronchospasm** and should not be relied upon for immediate relief. Waiting for the antihistamine to kick in is risky when your reliever medication is ineffective. The primary concern is the failure of your rescue inhaler, which requires prompt escalation of care according to asthma guidelines.",
    "next_suggestion": [
        "Sit upright immediately and use your rescue inhaler again, perhaps using a spacer if available, as technique can sometimes be an issue.",
        "If the wheezing/tightness does not significantly ease after the second dose of your rescue inhaler, **you must seek emergency medical help immediately (call 911 or your local emergency number)**, as this meets the criteria for when a rescue inhaler is failing.",
        "After the acute episode is managed, follow up with your clinician to review your asthma action plan and discuss environmental control strategies (like HEPA filtration) to minimize future exposure to triggers like dust/mites."
    ],
    "conversation_summary": "User experienced acute asthma exacerbation (tight chest, loud wheezing, ineffective rescue inhaler) triggered by environmental exposure (dust cleaning). User profile indicates pre-existing conditions: Exercise-induced asthma, Chronic Sinusitis, tight chest, wheezing, allergies to dust/mites/dander/mold. **Core Logic:** Ineffective SABA (rescue inhaler) during acute attack necessitates immediate escalation, irrespective of concurrent antihistamine use, because antihistamines do not provide rapid bronchodilation. **Key Fact Retrieval:** Web search findings explicitly state that if a reliever inhaler isn't easing symptoms, seek emergency medical help immediately [7], [3]. Antihistamines do not treat acute bronchospasm [Healthline]. **Actionable Advice:** Prioritize emergency escalation due to rescue inhaler failure."
}

---

## Agent Workflow

1. If an image is provided, the system runs OCR and extracts product label information.
2. The agent classifies intent and determines whether external research is needed.
3. Research may optionally be incorporated into the reasoning stage.
4. The agent synthesizes a final verdict and explanation.
5. The frontend presents the plan, reasoning, verdict, and suggested follow-ups.

---

## Safety Principles

- Conservative decision bias when uncertainty exists
- Avoids speculative or unsupported medical claims
- Encourages informed and cautious decision-making
- Prefers evidence-aligned reasoning when research is applied

---

## Roadmap

- Streaming agent responses
- Web-grounded research integration
- Ingredient allergen knowledge base expansion
- Long-term user health profile memory
- Containerized deployment support

---

## Contributing

Contributions are welcome. Please maintain code quality, clarity, and documentation consistency.

---

## License

MIT License.

---

## Acknowledgment

This project explores AI-assisted consumer health decision workflows and safety-aware label interpretation.
