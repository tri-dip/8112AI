# 8112.AI - Frontend Client

![Status](https://img.shields.io/badge/Status-Beta-blue)
![Framework](https://img.shields.io/badge/Framework-React_18-61DAFB)
![Language](https://img.shields.io/badge/Language-TypeScript-3178C6)
![Styling](https://img.shields.io/badge/Styling-Tailwind_CSS-06B6D4)

The **8112.AI Frontend** is a modern, responsive web interface for the Consumer Health Decision Engine. Built with React and TypeScript, it provides a fluid user experience for scanning food labels, interacting with the AI agent, and visualizing health verdicts through complex animations.

## ✨ Features

* **Cyberpunk/Dark Mode UI:** A sleek, immersive interface designed with Tailwind CSS and custom scrollbars.
* **Visual Label Scanning:** Integrated file upload handling to send food packaging images to the backend.
* **Transparent Thinking Engine:** Real-time animated status indicators (`Thinking`, `Researching`, `Analyzing`) powered by **Framer Motion**.
* **Dynamic Verdict Badges:** Visual "Safe", "Caution", and "Avoid" result cards with icon integration.
* **Structured Data Rendering:** parses complex JSON responses to display Ingredients and Nutrition Facts cleanly.
* **Interactive Chat:** A complete chat interface with streaming-like updates and follow-up suggestion buttons.

## 🛠️ Tech Stack

* **Core:** [React 18](https://react.dev/), [TypeScript](https://www.typescriptlang.org/)
* **Build Tool:** [Vite](https://vitejs.dev/)
* **Styling:** [Tailwind CSS](https://tailwindcss.com/)
* **Animations:** [Framer Motion](https://www.framer.com/motion/) (AnimatePresence, layout transitions)
* **Icons:** [Lucide React](https://lucide.dev/)
* **HTTP Client:** Native Fetch API

## 🚀 Getting Started

### Prerequisites

* Node.js (v18 or higher)
* npm or yarn
* The **8112.AI Backend** running on port `8000` (see Backend README).

### Installation

1.  **Clone the repository** (if you haven't already):
    ```bash
    git clone [https://github.com/your-username/8112-ai-frontend.git](https://github.com/your-username/8112-ai-frontend.git)
    cd 8112-ai-frontend
    ```

2.  **Install Dependencies:**
    ```bash
    npm install
    ```

    *Note: Ensure the specific UI libraries are installed:*
    ```bash
    npm install framer-motion lucide-react clsx tailwind-merge
    ```

3.  **Configure Tailwind:**
    Ensure your `tailwind.config.js` is set up to scan your file paths:
    ```javascript
    /** @type {import('tailwindcss').Config} */
    export default {
      content: [
        "./index.html",
        "./src/**/*.{js,ts,jsx,tsx}",
      ],
      theme: {
        extend: {},
      },
      plugins: [
        require('@tailwindcss/typography'), // Optional: for prose content
      ],
    }
    ```

### Running the Application

Start the development server:

```bash
npm run dev