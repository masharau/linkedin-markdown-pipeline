
# Gmail-to-LinkedIn Automated Content Pipeline

![Gmail](https://img.shields.io/badge/Gmail-Source-EA4335?style=flat-square) ![Gemini AI](https://img.shields.io/badge/Gemini-AI_Skill-blue?style=flat-square) ![Google Drive](https://img.shields.io/badge/Google_Drive-Buffer-green?style=flat-square) ![Make.com](https://img.shields.io/badge/Make-Automation-blueviolet?style=flat-square) ![LinkedIn](https://img.shields.io/badge/LinkedIn-API-0077B5?style=flat-square)

An automated, secure pipeline for publishing analytical Markdown (`.md`) content to LinkedIn. This architecture uses labeled Gmail emails as the data source, an AI-driven "Human-in-the-Loop" drafting process in a chat environment, and a robust low-code distribution pipeline (via Make.com). 

Crucially, it bypasses Google Drive's default binary `.docx` conversion to preserve clean UTF-8 text, Markdown formatting, and emojis.

## 🎯 How It Works: The Two-Phase Architecture

To maintain strict editorial control over sensitive content (e.g., civic education news, human rights updates), the pipeline is strictly divided into a manual pre-trigger phase (involving Gmail and AI) and an automated publishing phase (Make.com).

### Phase 1: The AI Workspace Workflow (Pre-Trigger)
*This happens entirely outside of Make.com, utilizing the Google Workspace integration within the AI chat interface (e.g., Google Gemini).*

1. **The Input (Gmail):** Relevant incoming news, materials, or rulings are marked with a specific label in the editor's Gmail inbox.
2. **AI Extraction & Drafting:** The human editor prompts the AI chat to retrieve the latest labeled emails. The AI (using the instructions from `content_generator_skill.md`) evaluates the text and generates a structured LinkedIn post in the chat.
3. **Iterative Approval:** The editor reviews the draft directly in the chat, requesting changes if necessary.
4. **Export & The Bridge:** Once approved, the editor exports the text as a downloadable `.md` file and **manually uploads it into the `LinkedIn_Staging` folder on Google Drive**. 

*Uploading this file is the physical action that triggers the automation.*

### Phase 2: The Automated Distribution (Make.com)
*This is the scheduled automation that takes over once the file is in Google Drive.*

1. **Trigger (Google Drive — Watch Files):** Monitors the `LinkedIn_Staging` folder for new `.md` files. **Critical Setting:** Processing is strictly limited to **1 file per run** (`Maximum number of results = 1`) to prevent post clustering.
2. **Extraction (Google Drive — Download a File):** Grabs the exact `.md` file detected by the trigger. **Critical Setting:** MS Word conversion is strictly **disabled** (format selected as `TXT` / `Plain Text`) to prevent Google Docs from corrupting the Markdown formatting or injecting page breaks.
3. **Publication (LinkedIn Pages — Create a Text Post):** Receives the clean UTF-8 text variable from the downloaded file and publishes it to the organization's page.
4. **Archiving (Google Drive — Move a File):** Moves the successfully published `.md` file to the `LinkedIn_Archive` folder to ensure it is never published twice.

## 📁 Repository Contents

* `README.md` — This documentation.
* `blueprint.json` — The exported Make.com scenario for 1-click import.
* `skills/content_generator_skill.md` — The core AI system prompt/skill. It contains the exact instructions, filtering rules, and Markdown templates used in Phase 1 to generate the posts from emails.

## 🚀 Quick Setup Guide

1. **Prepare the Source:** Create a specific label in your Gmail (e.g., `LinkedIn_News`) to mark incoming materials.
2. **Prepare the Buffer:** Create two folders in your Google Drive: `LinkedIn_Staging` and `LinkedIn_Archive`.
3. **Deploy the AI Skill:** Copy the contents of `skills/content_generator_skill.md` into your AI assistant's system instructions or custom GPT/Gemini skill builder. Ensure your AI has Workspace (Gmail) access enabled.
4. **Deploy the Automation:** Import the `blueprint.json` file into your Make.com account.
5. **Authenticate & Verify:** Connect your Google Drive and LinkedIn accounts in Make.com. Ensure the "Download a File" module has document conversion turned OFF.
6. **Test the Loop:** Mark an email with your Gmail label, ask your AI to draft a post based on that label, approve the draft, download the `.md` file, drop it into `LinkedIn_Staging`, and run the Make.com scenario.
