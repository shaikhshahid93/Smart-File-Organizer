# Smart File Organizer

An AI-powered Google Drive file organizer I built using **n8n, Gemini, Supabase, and MCP/Claude**.

The idea was to automate a simple but repetitive problem: dealing with files that have unclear names or are sitting in the wrong folder.

![Smart File Organizer Workflow](smart-file-organizer.png)

## What It Does

When a new file is added to my Google Drive **Inbox**, the workflow:

1. Detects the new file.
2. Downloads it and identifies its file type.
3. Extracts or analyzes its content.
4. Uses Gemini to classify the document and suggest a meaningful filename.
5. Checks the classification confidence.
6. Creates or finds the required category/subject folder.
7. Handles duplicate files and filename conflicts.
8. Renames and moves the file into the correct location.
9. Logs the result in Supabase.

For example:

```text
XQER.pdf
    ↓
AI_NOTES.pdf
    ↓
Organized/College/Artificial Intelligence/
```

## Main Features

- Content-based file classification
- Automatic file renaming
- Support for PDF, images, text, CSV, JSON, XLSX, XLS and Google Docs
- Categories such as College, Work, Personal, Finance, Certificates, Projects and Other
- Confidence-based review for uncertain files
- Duplicate detection using file checksums
- Filename conflict handling
- Automatic category and subject folder creation
- Review folder for low-confidence, unsupported and duplicate files
- Backfill workflow for files already present in the Inbox
- Supabase logging through `file_organizer_index`

## Workflow Overview

```text
Google Drive Inbox
        ↓
   Detect New File
        ↓
    Download File
        ↓
   Check File Type
        ↓
Extract / Analyze Content
        ↓
 AI Document Classification
        ↓
   Confidence Check
      /          \
 High            Low
  /                 \
Organize           Review
  ↓
Rename + Move
  ↓
Supabase Log
```

There is also a separate duplicate-check path using the file checksum before the normal organization process.

## AI and MCP

I used **MCP with Claude** while building and interacting with this automation. The workflow itself uses **Gemini inside n8n** for document and image analysis and classification.

So, to run the imported workflow, the main runtime requirements are **n8n, Google Drive, Gemini, and Supabase**. A separate Claude MCP setup is not required just to run the workflow.

## Tech Stack

| Technology | Purpose |
|---|---|
| n8n | Workflow automation and orchestration |
| Google Drive | File storage and organization |
| Gemini | Document and image analysis/classification |
| Supabase | Logging and duplicate tracking |
| MCP | AI-to-workflow interaction |
| Claude | Used with MCP while building/interacting with the automation |
| JavaScript | Custom workflow logic |
| JSON | n8n workflow configuration |

## Setup

### Requirements

- An n8n instance
- Google Drive account
- Gemini API credentials
- Supabase project

### Steps

1. Download `Smart File Organizer.json`.
2. Import the workflow into n8n.
3. Connect your Google Drive, Gemini and Supabase credentials.
4. Update the Google Drive folder IDs to match your own Inbox and destination structure.
5. Create the `file_organizer_index` table in Supabase with the fields used by the workflow.
6. Test the workflow with a few sample files before running it on your main Inbox.

## Repository Structure

```text
Smart-File-Organizer/
├── Smart File Organizer.json
├── smart-file-organizer.png
└── README.md
```

## Example

A file named `XQER.pdf` containing AI notes can be analyzed, classified, renamed to something meaningful such as `AI_NOTES.pdf`, and moved into the appropriate folder automatically.

The goal of the project is straightforward: **turn a messy file Inbox into a structured folder system without manually sorting every file.**

## Future Improvements

- OCR for scanned documents
- More configurable categories
- Notifications for files sent to review
- Web dashboard for organizer history
- More advanced version and duplicate management
