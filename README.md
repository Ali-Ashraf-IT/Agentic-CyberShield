# AI SOC Analyst

An Agentic AI SOC Analyst web application built with Streamlit, Python, and Large Language Models (LLM).

## Overview

This application acts as an automated Tier-3 SOC analyst. It ingests raw security logs, normalizes them, extracts Indicators of Compromise (IOCs), and uses an AI agent to perform an investigation, generate a timeline, assign a risk score, map techniques to MITRE ATT&CK, and create an Incident Response playbook.

**IMPORTANT:** The application prioritizes safety. All state-changing incident response actions (such as blocking an IP or isolating a host) are strictly generated as **recommendations requiring analyst approval**.

---

## 🚀 How to Run Locally

### 1. Requirements

Ensure you have Python 3.9+ installed.

### 2. Installation

1. Clone or download this project.
2. Open a terminal in the project directory.
3. Install the dependencies:
   ```bash
   pip install -r requirements.txt
   ```

### 3. API Configuration

This application connects to LLMs. By default, the `.env.example` shows how to connect to Groq, OpenRouter, or OpenAI.

1. Copy `.env.example` to `.env`:
   ```bash
   cp .env.example .env
   ```
2. Edit `.env` and add your API key. For Groq:
   ```env
   LLM_API_KEY=gsk_your_api_key_here
   LLM_BASE_URL=https://api.groq.com/openai/v1
   LLM_MODEL=llama-3.1-70b-versatile
   ```

*Note: You can also enter the API key directly in the Streamlit Sidebar when the app runs.*

### 4. Start the Application

```bash
streamlit run app.py
```

This will open your browser to the AI SOC Analyst dashboard.

---

## 📖 How to Test the Application

### 1. Sample Log Data

Copy the following synthetic log data and paste it into the **"Paste Raw Logs"** text area in the sidebar of the application:

```text
2026-09-10 10:01:21 Failed login user=administrator src_ip=185.10.10.20
2026-09-10 10:01:25 Failed login user=administrator src_ip=185.10.10.20
2026-09-10 10:01:31 Failed login user=administrator src_ip=185.10.10.20
2026-09-10 10:02:04 Successful login user=administrator src_ip=185.10.10.20
2026-09-10 10:02:19 Process=powershell.exe PID=4212 Parent=winword.exe
2026-09-10 10:02:21 Process=powershell.exe CommandLine="powershell -enc JABzAD0ATgBlAHcALQBPAGIAagBlAGMAdAAgAEkATwAuAE0AZQBtAG8AcgB5AFMAdAByAGUAYQBtACgAWwBDAG8AbgB2AGUAcgB0AF0AOgA6AEYAcgBvAG0AQgBhAHMAZQA2ADQAUwB0AHIAaQBuAGcAKAAiAEgA...=="
2026-09-10 10:03:10 Network connection src=10.10.10.15 dst=185.10.10.20 dst_port=443
```

### 2. Run Investigation
1. Ensure your API Key, Base URL, and Model are set in the sidebar.
2. Click **Start Investigation**.
3. Wait for the AI to parse the logs, extract IOCs, and generate the report.
4. Explore the tabs (Overview, Timeline, IOCs, MITRE ATT&CK, Response Playbook).

---

## 🌐 How to Deploy to GitHub & Streamlit Community Cloud

Follow these "baby steps" to deploy your app online for free:

### Phase 1: Upload to GitHub
1. Go to [GitHub.com](https://github.com) and sign in or create an account.
2. Click the **"+"** icon in the top right and select **"New repository"**.
3. Name it `ai-soc-analyst`. Leave it as Public (or Private) and do **not** initialize it with a README (since you already have one). Click **Create repository**.
4. In your local terminal (make sure you are inside your project folder), run:
   ```bash
   git init
   git add .
   git commit -m "Initial commit of AI SOC Analyst"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/ai-soc-analyst.git
   git push -u origin main
   ```
   *(Note: Ensure you do NOT commit your `.env` file! The `.gitignore` is already set up to ignore it).*

### Phase 2: Deploy to Streamlit Community Cloud
1. Go to [share.streamlit.io](https://share.streamlit.io/) and log in with your GitHub account.
2. Click **"New app"**.
3. Select your repository `ai-soc-analyst` and the branch `main`.
4. The **Main file path** should be `app.py`.
5. **CRITICAL STEP (Adding Secrets):** 
   - Click on **"Advanced settings..."** before deploying.
   - In the **Secrets** text area, add your API keys exactly as they appear in your `.env` file:
     ```toml
     LLM_API_KEY="gsk_your_groq_api_key_here"
     LLM_BASE_URL="https://api.groq.com/openai/v1"
     LLM_MODEL="llama-3.1-70b-versatile"
     ```
   - Click **Save**.
6. Click **Deploy!**
7. Wait a few minutes. Your AI SOC Analyst will now be live on a public URL!

---

## 🔒 Security & Architecture

* **No Hardcoded Secrets**: Passwords and API keys are strictly kept in the `.env` (local) or Streamlit Secrets (cloud).
* **Replaceable LLMs**: The architecture connects via `llm_client.py`, which is an OpenAI-compatible wrapper. This allows swapping Groq, OpenRouter, OpenAI, or Local LLMs (like vLLM/Ollama) by merely changing the `LLM_BASE_URL`.
* **Safe Actions**: The system is read-only by design and will only provide bash/powershell commands inside an incident playbook for the human analyst to review.
