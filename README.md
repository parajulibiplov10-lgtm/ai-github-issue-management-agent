# AI-Powered GitHub Issue Management Agent

An automated GitHub issue triage workflow built using **n8n**, **Google Gemini**, and the **GitHub API**.

## Overview

When a new GitHub issue is opened, this workflow uses an AI agent to analyze its title and description, assign a category and priority, and post a short explanation as a comment. It checks existing comments first to avoid repeating an AI classification.

## Features

- Detects newly opened issues with a GitHub trigger
- Categorizes issues as `bug`, `enhancement`, or `documentation`
- Assigns `priority: low`, `priority: medium`, or `priority: high`
- Uses Gemini to analyze issue context
- Retrieves existing comments before posting a new classification
- Updates GitHub labels and creates a classification comment
- Includes a check for duplicate classification comments

## Workflow

```text
GitHub Issue Opened
        |
        v
  Filter: opened
        |
        v
GET Existing Comments
        |
        v
     AI Agent <---- Google Gemini
        |
        +-- GitHub: Get Issue
        +-- HTTP: Get Comments
        +-- GitHub: Edit Labels
        +-- GitHub: Create Comment
```

## Setup

1. Import [`workflow.json`](workflow.json) into n8n.
2. Connect your own GitHub OAuth2 credentials to the GitHub nodes.
3. Configure the GitHub repository owner and repository name in the trigger and GitHub tool nodes. Replace the placeholders `YOUR_GITHUB_USERNAME` and `YOUR_TARGET_REPOSITORY`.
4. Connect your Google Gemini model credentials (or an available supported model connection).
5. Configure HTTP Header Auth for GitHub API GET requests; use a securely stored token with the required repository permissions. Do not put tokens directly into the workflow JSON.
6. Ensure the repository has the labels `bug`, `enhancement`, `documentation`, `priority: low`, `priority: medium`, and `priority: high`.
7. Activate the workflow and open a **new** test issue. Verify that labels and exactly one comment are added.

**Important:** Importing the workflow alone does not connect credentials or activate it. The exported workflow has been deactivated for safe sharing.

## Example

**Issue:** Login button does not respond after entering credentials.

**Expected classification:**

```text
Category: bug
Priority: High
Reason: Login is a core function and users cannot access their accounts.
```

This is an illustrative example; actual AI classifications may vary.

## Technologies

- n8n workflow automation
- Google Gemini language model
- GitHub Issues and GitHub API
- HTTP GET requests and JSON
- AI agent tool calling

## Notes and limitations

- This workflow classifies issues; it does **not** fix application bugs.
- AI classification can make mistakes; review labels when needed.
- Duplicate-comment protection is implemented through instructions and comment retrieval; it is not an atomic guarantee under simultaneous executions.
- The exported manual trigger/test branch is not connected to the live agent path.
- The workflow is designed for new issue events, not automatically for every edit or reopening.
- Repository details and credentials must be configured after import.

## Security

The published workflow omits credential bindings, webhook identifiers, and instance metadata. Do not commit API keys, GitHub access tokens, `.env` files, or private n8n exports containing secrets.

## Project status

Tested with sample GitHub issues, including bug and enhancement classification and posting comments. This repository provides the workflow configuration and setup guide; it does not include a standalone application.
