# ⚙️ Step-by-Step Installation & Setup Guide

This guide provides complete instructions to set up, configure, and execute the **Automated AI LinkedIn Content & Graphic Generator** workflow.

---

## 📌 Prerequisites

Before starting, ensure you have:
1. An active [n8n](https://n8n.io) instance (v1.x or higher, with LangChain nodes enabled).
2. A free API key from [Tavily AI Web Search](https://tavily.com).
3. An [OpenAI Platform](https://platform.openai.com) account with API key (`gpt-4o-mini` and DALL-E 3 / `gpt-image-1-mini` access).
4. A Google Cloud account for Google Sheets OAuth authorization.
5. A LinkedIn Personal Profile or Company Page with admin permissions for API posting.

---

## 🚀 Step 1: Import the n8n Workflow

1. Open your n8n Dashboard.
2. Click **Workflows -> Import from File**.
3. Select `workflow.json` (or `LinkedIn Content Generation.json`).
4. The workflow will render on the canvas.

---

## 🔑 Step 2: Configure Credentials & Nodes

### 1. Tavily Search API Node
1. Open the **Search the WEB** node.
2. Under `Header Parameters`, find the `Authorization` header.
3. Replace `Bearer YOUR_TAVILY_API_KEY` with your actual Tavily API Key (`Bearer tvly-...`).

### 2. OpenAI Credentials
1. In n8n, create an **OpenAI API** credential using your secret key (`sk-...`).
2. Connect this credential to:
   - **OpenAI Chat Model** (attached to `AI Content Writer`).
   - **OpenAI Chat Model1** (attached to `AI Image Prompt Generator`).
   - **Generate an image** node.

### 3. Google Sheets OAuth Credential
1. Create a Google Sheet named `LinkedIn Content Topics`.
2. Add the following columns in Row 1:
   ```text
   Topic | Status | Content
   ```
3. Add a row under `Topic` (e.g. `AI Agents in Customer Service`) and set `Status` to `To-do`.
4. In n8n, open the **Grab The Topic** and **Add Content to sheets** nodes.
5. Select your Google Sheets credential and point to your spreadsheet ID.

### 4. LinkedIn OAuth Credential
1. Create an n8n credential for **LinkedIn OAuth2 API**.
2. Connect your LinkedIn Account.
3. In the **Create a post** node, select your LinkedIn Organization or Personal Profile.

---

## 🧪 Step 3: Test & Run

1. Open your Google Sheet and add a topic with `Status = To-do`.
2. In n8n, click **Execute Workflow**.
3. Observe:
   - Tavily fetches 3 live web articles.
   - AI Content Writer crafts the post copy.
   - AI Image Prompt Generator drafts the DALL-E prompt.
   - OpenAI renders the 1:1 graphic.
   - The post + image is published directly to your LinkedIn account!
   - Google Sheet status updates to `Done` with full post content archived.
