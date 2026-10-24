# Drive-to-LinkedIn Automated Content Pipeline

![Make.com](https://img.shields.io/badge/Make-Automation-blue?style=flat-square) ![Google Drive](https://img.shields.io/badge/Google_Drive-Storage-green?style=flat-square) ![LinkedIn](https://img.shields.io/badge/LinkedIn-API-0077B5?style=flat-square)

A robust automation pipeline for seamlessly publishing Markdown (`.md`) content from Google Drive to LinkedIn via Make.com. It ensures the complete preservation of text structure, formatting, and UTF-8 characters (including emojis).

## 🎯 The Problem
Directly integrating Google Documents with external APIs causes two critical technical issues:
1. **Binary Conversion:** The Google Drive API defaults to converting text files into `.docx` format (a ZIP archive with a `PK` signature), which corrupts the encoding and sends machine garbage to the target API instead of plain text.
2. **Metadata Injections:** Google Docs' internal algorithms forcibly add page breaks and system headers during export.

**Solution:** This architecture exclusively processes "raw" **Markdown (`.md`)** files, completely bypassing the Google Docs conversion engine and preserving a clean UTF-8 data stream.

## 🛠️ Pipeline Architecture (Make.com)

The pipeline is built on a 4-module structure with scheduling support:

1. **Trigger (Google Drive — Watch Files / Search Files):**
   * Monitors the `LinkedIn_Staging` buffer folder.
   * **Critical Setting:** Processing is strictly limited to **1 file per run** (`Maximum number of results = 1`).
2. **Extraction (Google Drive — Download a File):**
   * Receives the dynamic `File ID` from the first module (`Enter manually` mode).
   * **Critical Setting:** MS Word conversion is disabled (format selected as `TXT` / `Plain Text`).
3. **Publication (LinkedIn Pages — Create an Organization Text Post):**
   * Receives the clean text variable.
   * Publishes the content to the organization page.
4. **Archiving (Google Drive — Move a File):**
   * Moves the successfully published `.md` file to the `LinkedIn_Archive` folder to prevent duplicate processing.

## 🚀 Quick Start & Setup

1. Create two folders in your Google Drive: `LinkedIn_Staging` and `LinkedIn_Archive`.
2. Import the `blueprint.json` file into your Make.com account.
3. Authenticate the Google Drive and LinkedIn modules.
4. Check the settings of the second module (Download a File): ensure conversion to `.docx` is disabled.
5. Set the scenario schedule (e.g., Mon-Fri, 07:30 and 16:00).
6. Place a test `.md` file into the `LinkedIn_Staging` folder and click **Run once**.