# Kokoro Video Automation System — AI Agent Build & Deployment Guide

This repository serves as a **System Build and Instruction Guide** designed specifically for **AI Coding Agents** (such as Antigravity, Devin, Cursor, etc.) and developer teams to easily clone, configure, link, and deploy the entire true-crime video automation system.

The system is a fully automated video pipeline that generates true crime script CSVs, renders AI visuals (using Flux.1 Schnell on GPU), synthesizes voices (using Kokoro TTS), burns captions, and compiles them into finished YouTube videos.

---

## 🛠️ System Architecture

The project consists of 4 main components:
1. **Script Generator Skill (`truecrimescript`)**: Instructs the AI agent to write fact-checked, high-retention CSV scripts.
2. **Frontend UI (Vercel)**: A sleek glassmorphic dashboard for parameter configuration and video downloads.
3. **Backend API (Hugging Face Space)**: A Node/Express server that acts as a secure trigger, compiling notebooks and talking to the Kaggle CLI.
4. **Execution Pipeline (Kaggle GPU)**: A Tesla T4 GPU-enabled environment that runs the Jupyter notebook to do the heavy rendering.

---

## 🤖 Instructions for the AI Agent

> [!IMPORTANT]
> **AI Agent Action Plan:** Read and execute the following steps to rebuild and link this project on the user's workspace.

### Step 1: Clone the Project Codebase
The entire codebase is structured as a monorepo. Clone it to the local workspace:
```bash
git clone https://github.com/Airpyk-98/truecrimescripts.git
```
The repository contains:
*   `kaggle-trigger-app/` - The backend API, the frontend assets, and the base Jupyter notebook.
*   `kaggle-trigger-app/public/` - The Vercel frontend dashboard.
*   `kaggle-trigger-app/kokoro-tts-automation.ipynb` - The Kaggle T4 GPU notebook.

### Step 2: Configure Local Credentials
Create a file named `tokens.txt` inside the `kaggle-trigger-app/` directory and populate it with the user's secret keys:
```text
========================================================================
                     PROJECT CREDENTIALS & TOKENS
========================================================================

--- Kaggle Credentials ---
Username: <USER_KAGGLE_USERNAME>
API Key / Token (Access Token): <USER_KAGGLE_API_KEY>

--- Hugging Face Credentials ---
Hugging Face Token: <USER_HF_WRITE_TOKEN>

--- GitHub Credentials ---
GitHub Token: <USER_GITHUB_TOKEN>

--- Vercel Credentials ---
Vercel CLI Token (Access Token): <USER_VERCEL_TOKEN>
Vercel Team Scope: <USER_VERCEL_TEAM_ID>
Vercel Project ID: <USER_VERCEL_PROJECT_ID>
========================================================================
```

### Step 3: Set Up the Script Generator Skill
To write script CSV files matching the required format:
1. Locate the `truecrimescript` skill instructions (usually saved as `C:\Users\DELL\.gemini\config\skills\truecrimescript\SKILL.md` or locally in this repo as `truecrimescript.md`).
2. Verify that your system prompts incorporate the `truecrimescript` skill format, column headers (`Serial number, image prompt, video prompt, voice over prompt`), and Ray William Johnson's narrative voice.
3. Trigger script generation by asking for a case (e.g. *"generate a true crime script for [Crime Case] in CSV format"*).

### Step 4: Link the Frontend to the Backend
1. Open `kaggle-trigger-app/public/app.js`.
2. Locate the default backend configuration URL or input box.
3. Link the frontend to point to the backend's direct URL (e.g., `https://<hf-username>-truecrime-video-generator.hf.space` or `http://localhost:7860` for local dev).

### Step 5: Link the Backend to Kaggle GPU
1. In `kaggle-trigger-app/server.js`, confirm the paths point to the local base notebook (`kokoro-tts-automation.ipynb`).
2. The backend server uses the Kaggle API CLI. Confirm the server automatically writes credentials into the `KAGGLE_CONFIG_DIR` during executions, keeping credentials containerized.
3. The server uses the `NvidiaTeslaT4` accelerator flag when pushing the notebook:
   ```bash
   kaggle kernels push -p "./temp_job_id" --accelerator NvidiaTeslaT4
   ```

### Step 6: Exclude Secrets from Tracking
To prevent exposing credentials:
1. Ensure `tokens.txt` is added to `kaggle-trigger-app/.gitignore`.
2. Ensure `tokens.txt` is added to the `ignore_patterns` list in `kaggle-trigger-app/upload.py`.

### Step 7: Push and Deploy
*   **Backend (Hugging Face Spaces):** Run `python upload.py` from the `kaggle-trigger-app/` folder to upload the code to the Hugging Face Space.
*   **Frontend (Vercel):** Set up Vercel CLI and push using:
   ```bash
   vercel --prod --token <VERCEL_TOKEN> --scope <TEAM_SCOPE>
   ```

---

## 🔑 Credential Setup Guide (For Humans)

Here is where to find the tokens and keys required to populate your `tokens.txt` file:

### 1. Kaggle API Credentials
1. Log in to [Kaggle.com](https://www.kaggle.com).
2. Go to your **Profile** (top right) -> **Settings**.
3. Scroll down to the **API** section and click **Create New Token**.
4. A file named `kaggle.json` will download. Open it to extract your `username` and `key`.

### 2. Hugging Face Access Token
1. Log in to [HuggingFace.co](https://huggingface.co).
2. Go to your **Settings** -> **Access Tokens**.
3. Click **Create New Token** (or **New Token**).
4. Set the name, choose the **Write** role, and click **Generate**. Copy the token (`hf_...`).

### 3. Vercel Authentication Token
1. Log in to [Vercel.com](https://vercel.com).
2. Click your avatar (top right) -> **Account Settings** -> **Tokens**.
3. Click **Create** to generate a new token. Set scope/access as required and copy the token (`vcp_...`).
4. Find your Project ID and Team ID inside the `.vercel/project.json` file in your repository.

### 4. GitHub Developer Token
1. Log in to [GitHub.com](https://github.com).
2. Go to **Settings** (top right) -> **Developer Settings** (bottom left).
3. Select **Personal Access Tokens** -> **Tokens (classic)**.
4. Click **Generate new token (classic)**, check the `repo` scope, and click generate. Copy the token (`gho_...` or `ghp_...`).

---

## 🔗 Project Reference Links
*   **Project Monorepo:** [https://github.com/Airpyk-98/truecrimescripts.git](https://github.com/Airpyk-98/truecrimescripts.git)
*   **Active Frontend (Vercel):** [https://public-wine-three-41.vercel.app](https://public-wine-three-41.vercel.app)
*   **Active Backend (Hugging Face Space):** [https://huggingface.co/spaces/Airpyk98/truecrime-video-generator](https://huggingface.co/spaces/Airpyk98/truecrime-video-generator)
*   **True Crime Skill Instruction Set:** [truecrimescript.md](file:///C:/Users/DELL/Documents/antigravity/epic-nobel/truecrimescript.md)
