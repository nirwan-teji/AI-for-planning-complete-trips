# AI Travel Planner Agent
[![Ask DeepWiki](https://devin.ai/assets/askdeepwiki.png)](https://deepwiki.com/nirwan-teji/AI-for-planning-complete-trips)

This repository contains an advanced AI-powered travel agent designed to help users plan complete trips worldwide. Using an agentic workflow built with LangGraph, the application leverages a suite of specialized tools to provide comprehensive and detailed travel plans, including itineraries, cost breakdowns, and real-time information.

The application features a user-friendly web interface built with Streamlit and a robust backend powered by FastAPI.

## Features

- **Comprehensive Itineraries:** Generates detailed day-by-day travel plans, offering both popular tourist routes and off-beat suggestions.
- **Real-time Information:** Fetches live data for:
    - **Weather:** Current conditions and forecasts via OpenWeatherMap.
    - **Places:** Attractions, restaurants, activities, and transportation modes using Google Places API with a Tavily search fallback.
- **Expense Planning:**
    - Calculates total estimated hotel costs.
    - Provides a complete trip expense breakdown.
    - Estimates an approximate daily budget.
- **Currency Conversion:** Converts costs between different currencies using the ExchangeRate-API.
- **Dual LLM Support:** Easily switch between different language model providers (Groq and OpenAI) by modifying the configuration.
- **Agentic Architecture:** Utilizes LangGraph to create a stateful, multi-tool agent that can reason and decide which tool to use to answer a user's query.

## Architecture

The application is composed of a frontend, a backend, and an AI agent core:

-   **Frontend (`streamlit_app.py`):** A web interface built with Streamlit that allows users to interact with the travel agent in a chat-like format.
-   **Backend (`main.py`):** A FastAPI server that exposes an API endpoint (`/query`) to receive user requests from the frontend.
-   **Agent (`agent/agentic_workflow.py`):** The core of the application. It uses LangGraph to define a workflow (a state graph) where the AI agent can iteratively use a set of tools to gather information and formulate a complete response.
-   **LLM Integration (`utils/model_loader.py`):** A flexible loader to instantiate language models from either Groq or OpenAI, based on the `config.yaml` file.
-   **Tools (`tools/`):** A collection of specialized functions that the agent can invoke:
    -   `place_search_tool.py`: Searches for attractions, restaurants, etc.
    -   `weather_info_tool.py`: Fetches weather data.
    -   `expense_calculator_tool.py`: Performs cost calculations.
    -   `currency_conversion_tool.py`: Handles currency conversions.
-   **Utilities (`utils/`):** Wrapper classes and helper functions that interact with external APIs (Google Places, OpenWeatherMap, etc.).

## Setup and Installation

Follow these steps to set up and run the project locally.

### 1. Clone the Repository
```bash
git clone https://github.com/nirwan-teji/AI-for-planning-complete-trips.git
cd AI-for-planning-complete-trips
```

### 2. Create a Virtual Environment
```bash
python -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables
Create a file named `.env` in the root directory of the project and add the following API keys.

```
GROQ_API_KEY="your_groq_api_key"
OPENAI_API_KEY="your_openai_api_key"
GPLACES_API_KEY="your_google_places_api_key"
TAVILY_API_KEY="your_tavily_api_key"
OPENWEATHERMAP_API_KEY="your_openweathermap_api_key"
EXCHANGE_RATE_API_KEY="your_exchangerate-api.com_v6_api_key"
```

**Note:** You can obtain these keys from their respective service providers. While both `GROQ_API_KEY` and `OPENAI_API_KEY` are listed, you only need the one corresponding to the LLM provider you choose to use.

## Usage

You need to run the backend and frontend servers in two separate terminals.

### 1. Start the Backend Server
In your first terminal, run the FastAPI application:
```bash
uvicorn main:app --reload --port 8000
```
The backend will be available at `http://localhost:8000`.

### 2. Start the Frontend Application
In your second terminal, run the Streamlit application:
```bash
streamlit run streamlit_app.py
```
The application will open in your web browser, typically at `http://localhost:8501`. You can now start planning your trips by typing your requests into the chat interface.

### Example Query
```
Plan a trip to Goa for 5 days
```

## Project Structure
```
├── agent/                # LangGraph agentic workflow
├── config/               # Configuration files (e.g., config.yaml for LLM choice)
├── prompt_library/       # System prompts for the AI agent
├── tools/                # Tools for weather, places, calculation, etc.
├── utils/                # Utility functions and API wrappers
├── main.py               # FastAPI backend entry point
├── streamlit_app.py      # Streamlit frontend application
├── requirements.txt      # Python dependencies
└── setup.py              # Project setup script
