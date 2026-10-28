# Human-in-the-Loop: AI to LinkedIn Content Pipeline

![Gemini AI](https://img.shields.io/badge/Gemini-AI_Skill-blue?style=flat-square) ![Google Drive](https://img.shields.io/badge/Google_Drive-Buffer-green?style=flat-square) ![Make.com](https://img.shields.io/badge/Make-Automation-blueviolet?style=flat-square) ![LinkedIn](https://img.shields.io/badge/LinkedIn-API-0077B5?style=flat-square)

An automated, secure pipeline for publishing analytical Markdown (`.md`) content to LinkedIn. This architecture bridges the gap between an AI-driven "Human-in-the-Loop" drafting process (in a chat environment) and a robust low-code distribution pipeline (Make.com). 

Crucially, it bypasses Google Drive's default binary `.docx` conversion to preserve clean UTF-8 text, Markdown formatting, and emojis.

## 🎯 How It Works: The Two-Phase Architecture

To maintain strict editorial control over sensitive content (e.g., civic education news, human rights updates), the pipeline is strictly divided into a manual pre-trigger phase and an automated publishing phase.

### Phase 1: The Manual AI Workflow (Pre-Trigger)
*This happens entirely outside of Make.com, within your AI chat interface (e.g., Google Gemini).*

1. **The Input:** The human editor pastes a raw news link, legal ruling, or text into the AI chat.
2. **AI Filtering & Drafting:** The AI (using the instructions from `content_generator_skill.md`) evaluates the text for regional relevance. If it passes, the AI generates a structured LinkedIn post in the chat.
3. **Iterative Approval:** The editor reviews the draft directly in the chat, requesting changes if necessary.
4. **Export & The Bridge:** Once approved, the editor commands the AI to export the text. The AI provides a downloadable `.md` file. The editor downloads this file and **manually uploads (drags and drops) it into the `LinkedIn_Staging` folder on Google Drive**. 

*Uploading this file is the physical action that triggers the automation.*

### Phase 2: The Automated Pipeline (Make.com)
*This is the scheduled automation that takes over once the file is in Google Drive.*

1. **Trigger (Google Drive — Watch Files):** Monitors the `LinkedIn_Staging` folder for new `.md` files. **Critical Setting:** Processing is strictly limited to **1 file per run** (`Maximum number of results = 1`) to prevent post clustering.
2. **Extraction (Google Drive — Download a File):** Grabs the exact `.md` file detected by the trigger. **Critical Setting:** MS Word conversion is strictly **disabled** (format selected as `TXT` / `Plain Text`) to prevent Google Docs from corrupting the Markdown formatting or injecting page breaks.
3. **Publication (LinkedIn Pages — Create a Text Post):** Receives the clean UTF-8 text variable from the downloaded file and publishes it to the organization's page.
4. **Archiving (Google Drive — Move a File):** Moves the successfully published `.md` file to the `LinkedIn_Archive` folder to ensure it is never published twice.

## 📁 Repository Contents

* `README.md` — This documentation.
* `blueprint.json` — The exported Make.com scenario for 1-click import.
* `skills/content_generator_skill.md` — The core AI system prompt/skill. It contains the exact instructions, filtering rules, and Markdown templates used in Phase 1 to generate the posts.

## 🚀 Quick Setup Guide

1. **Prepare the Buffer:** Create two folders in your Google Drive: `LinkedIn_Staging` and `LinkedIn_Archive`.
2. **Deploy the AI Skill:** Copy the contents of `skills/content_generator_skill.md` into your AI assistant's system instructions or custom GPT/Gemini skill builder.
3. **Deploy the Automation:** Import the `blueprint.json` file into your Make.com account.
4. **Authenticate & Verify:** Connect your Google Drive and LinkedIn accounts in Make.com. Ensure the "Download a File" module has document conversion turned OFF.
5. **Set Schedule:** Set the Make.com scenario schedule (e.g., twice a day at 07:30 and 16:00).
6. **Test the Loop:** Drop a news item into your AI chat, approve the generated draft, download the `.md` file, drop it into `LinkedIn_Staging`, and run the Make.com scenario.
