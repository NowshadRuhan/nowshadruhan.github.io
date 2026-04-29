---
title: "AI-agent-for-extract-transform-HackerNews-readable-data-and-send-whatsapp"
excerpt: "An automated n8n workflow that fetches top HackerNews stories, extracts raw data, and uses an AI agent to generate clean, readable summaries. The processed insights are sent directly to WhatsApp via the WhatsApp Cloud API, creating a seamless, automated tech‑news delivery system.<br/><img src='/images/project-3.png'>"
collection: portfolio
---

This project implements an automated n8n workflow that acts as an AI‑powered news assistant for HackerNews. It fetches the latest or top‑ranked stories from the HackerNews API, extracts the raw JSON data, and uses an AI agent to transform it into clean, readable summaries optimized for quick consumption. The workflow removes noise, highlights key insights, and restructures the content into a human‑friendly format. Once processed, the AI‑generated summaries are delivered directly to the user through WhatsApp using the WhatsApp Cloud API. This creates a seamless end‑to‑end pipeline where HackerNews updates are automatically collected, analyzed, summarized, and pushed to WhatsApp—turning n8n into a personal tech‑news delivery system that runs on autopilot.


1. Set up WhatsApp Cloud API
   - Meta Developer Portal → Create App → Add WhatsApp → Configure
   - Create a Meta Developer account and app
   - Enable WhatsApp and get Phone Number ID and WhatsApp Business Account ID
   - Generate a permanent or long‑lived access token
   - Add your phone as a test recipient in the WhatsApp section

2. Create a new n8n workflow
   - n8n → New Workflow
   - Open your n8n instance
   - Click New Workflow and give it a name like HackerNews AI WhatsApp Assistant
   - Save the empty workflow as a starting point

3. Add a trigger for automation
   - Add Node → Cron
   - Add a Cron node as the first node
   - Configure it to run on your preferred schedule (e.g., every hour or once per day)
   - This ensures the workflow runs automatically without manual execution

4. Fetch stories with the HackerNews node
   - Add Node → Hacker News
   - Add the Hacker News node and connect it after the Cron node
   - Choose the operation (e.g., Get Top Stories or Get New Stories)
   - Optionally limit the number of stories (e.g., top 5–10)
   - Confirm the output includes fields like title, url, score, and time

5. Prepare data for the AI agent
   - Add Node → Item Lists / IF (optional)
   - Optionally add an Item Lists node to keep only the first N items
   - Or add an IF node to filter by score, keywords, or type
   - Pass only the items you want summarized to the AI node

6. Call the AI agent to summarize stories
   - An LLM transforms raw HackerNews JSON into short, readable summaries.
   - Add Node → HTTP Request
   - Add an HTTP Request node after the HackerNews (or filter) node
   - Set Method to POST and URL to your LLM endpoint (e.g., OpenAI chat completions)
   - Add headers: Authorization: Bearer YOUR_API_KEY, Content-Type: application/json
   - In the JSON body, build a prompt that includes title, url, and text/story fields
   - Instruct the model to output concise, WhatsApp‑friendly summaries

7. Extract the AI‑generated summary
   - Add Node → Set
   - Add a Set node after the AI HTTP Request
   - Map the model’s response field (e.g., choices[0].message.content) into a new field like summary
   - Keep original fields like title and url for context in the final message

8. Format one or multiple summaries for WhatsApp
   - Add Node → Item Lists (Aggregate) or Set
   - Build a template like: 1. *Title*\nSummary\nLink
   - Store the final text in a field such as whatsappMessage

9. Send summaries via WhatsApp Cloud API
   - This step delivers the AI‑processed HackerNews summaries directly to your WhatsApp.
   - Add Node → HTTP Request
   - Add another HTTP Request node for WhatsApp and connect it after the formatting step
   - Set Method to POST and URL to https://graph.facebook.com/v20.0/YOUR_PHONE_NUMBER_ID/messages
   - Add headers: Authorization: Bearer YOUR_WHATSAPP_TOKEN, Content-Type: application/json
   - In the JSON body, set to to your phone number and text.body to {{$json["whatsappMessage"]}} or the per‑item message

10. Test, refine, and enable the workflow
   - n8n → Execute Workflow / Activate
   - Run the workflow once manually to confirm each node behaves as expected
   - Check that HackerNews stories are fetched and AI summaries look clean and readable
   - Confirm WhatsApp messages arrive correctly formatted on your phone
   - Adjust schedule, number of stories, and prompt style as needed
   - Activate the workflow so it runs automatically on your chosen schedule



## AI Agent. 
![aiagent](https://github.com/NowshadRuhan/AI-agent-for-extract-transform-HackerNews-readable-data-and-send-whatsapp/blob/main/Screenshot%202026-04-30%20at%2012.07.11%E2%80%AFAM.png?raw=true) 