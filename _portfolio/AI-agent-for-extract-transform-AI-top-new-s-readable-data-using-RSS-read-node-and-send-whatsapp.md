---
title: "AI-agent-for-extract-transform-AI-top-news-readable-data-using-RSS-read-node-and-send-whatsapp"
excerpt: "An n8n workflow that reads top AI news from RSS feeds, uses an AI agent to extract and transform the raw articles into clear, readable summaries, and sends the final insights directly to WhatsApp. This creates an automated, real‑time AI news delivery system.
 <br/><img src='/images/main-pic.png'>"
collection: portfolio
---

This project creates a fully automated n8n workflow that builds your own AI agent to collect top news from your field,
 transform it into clear, readable summaries, and deliver it directly to your WhatsApp. 
 Using the RSS Read node, the workflow pulls the latest AI‑related articles from trusted sources, 
 then an AI agent extracts key insights, removes noise, and rewrites the content into concise,
  human‑friendly updates. Finally, the WhatsApp Cloud API sends the summarized news straight to your phone, 
turning n8n into a personal, real‑time AI news assistant that runs continuously and keeps you informed without manual effort.

## How It Works
1. **RSS Read Node**
   - _Fetches the latest AI‑related articles from selected RSS feeds and outputs structured news items._

2. **AI Agent Node**
   - _Uses an LLM to extract key insights, remove noise, and transform each article into a short, readable summary optimized for WhatsApp._

3. **Aggregate / Format Node**
   - _Combines multiple AI‑generated summaries into one clean digest or formats each item into a WhatsApp‑friendly message._

4. **WhatsApp Cloud API Node**
   - _Sends the final summarized news directly to your WhatsApp number as a real‑time update._

## AI Agent.

![aiagent](https://github.com/NowshadRuhan/AI-agent-for-extract-transform-AI-top-new-s-readable-data-using-RSS-read-node-and-send-whatsapp/blob/main/main-pic.png?raw=true) 

## AI Agent: workflow video: Watch the video

<!-- Basic YouTube embed -->
<iframe
  width="560"
  height="315"
  src="https://www.youtube.com/embed/56rl0BWcDtI"
  title="YouTube video player"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
  referrerpolicy="strict-origin-when-cross-origin"
  allowfullscreen>
</iframe>