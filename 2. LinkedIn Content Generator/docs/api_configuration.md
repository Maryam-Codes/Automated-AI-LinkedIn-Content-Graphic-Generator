# 🔌 API & Integration Specifications

This document details the API schemas, payload configurations, and data structures utilized in the **Automated AI LinkedIn Content & Graphic Generator** workflow.

---

## 1. Tavily AI Web Search API (`POST https://api.tavily.com/search`)

Tavily is used to retrieve real-time, grounded web research on the target topic.

### Request Headers & Payload
```http
POST https://api.tavily.com/search
Authorization: Bearer tvly-dev-xxxxxxxxxxxxxxxxxxxxxxxx
Content-Type: application/json
```

```json
{
  "query": "Search the web on AI Agents in B2B Automation",
  "auto_parameters": false,
  "topic": "general",
  "search_depth": "basic",
  "chunks_per_source": 3,
  "max_results": 3,
  "include_answer": true,
  "include_raw_content": false,
  "include_images": false
}
```

### Response Mapping in n8n (LangChain Input)
```javascript
Article 1: {{ $json.results[0].content }}
Article 2: {{ $json.results[1].content }}
Article 3: {{ $json.results[2].content }}
```

---

## 2. OpenAI Image Generation Node Specifications

The image generator node uses DALL-E 3 (`gpt-image-1-mini`) to render 1024x1024 LinkedIn graphics based on the engineered prompt.

### Node Parameters
* **Resource:** `image`
* **Model ID:** `dall-e-3` / `gpt-image-1-mini`
* **Prompt Input Expression:** `={{ $json.output }}`
* **Image Ratio:** `1:1 Square (1024x1024)`

---

## 3. Google Sheets Content Queue Schema

The Google Sheets node uses standard OAuth2 API v4 to filter `To-do` items and write back completed posts.

### Read Filter Query (Node: `Grab The Topic`)
* **Lookup Column:** `Status`
* **Lookup Value:** `To-do`
* **Return Option:** `returnFirstMatch`

### Write Back Payload (Node: `Add Content to sheets`)
* **Matching Column:** `Topic`
* **Updated Values:**
  * `Status` = `Done`
  * `Content` = `={{ $('AI Content Writer').item.json.output }}`
