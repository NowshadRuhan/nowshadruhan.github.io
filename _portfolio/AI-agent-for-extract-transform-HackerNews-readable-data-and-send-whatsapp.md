---
title: "AI-agent-for-extract-transform-HackerNews-readable-data-and-send-whatsapp"
excerpt: "An automated n8n workflow that fetches top HackerNews stories, extracts raw data, and uses an AI agent to generate clean, readable summaries. The processed insights are sent directly to WhatsApp via the WhatsApp Cloud API, creating a seamless, automated tech‑news delivery system.<br/><img src='/images/project-3.png'>"
collection: portfolio
---

This project implements an automated n8n workflow that acts as an AI‑powered news assistant for HackerNews. It fetches the latest or top‑ranked stories from the HackerNews API, extracts the raw JSON data, and uses an AI agent to transform it into clean, readable summaries optimized for quick consumption. The workflow removes noise, highlights key insights, and restructures the content into a human‑friendly format. Once processed, the AI‑generated summaries are delivered directly to the user through WhatsApp using the WhatsApp Cloud API. This creates a seamless end‑to‑end pipeline where HackerNews updates are automatically collected, analyzed, summarized, and pushed to WhatsApp—turning n8n into a personal tech‑news delivery system that runs on autopilot.


1. **Set up WhatsApp Cloud API**
   - _Meta Developer Portal → Create App → Add WhatsApp → Configure_
   - _Create a Meta Developer account and app_
   - _Enable WhatsApp and get Phone Number ID and WhatsApp Business Account ID_
   - _Generate a permanent or long‑lived access token_
   - _Add your phone as a test recipient in the WhatsApp section_

2. **Create a new n8n workflow**
   - _n8n → New Workflow_
   - _Open your n8n instance_
   - _Click New Workflow and give it a name like HackerNews AI WhatsApp Assistant_
   - _Save the empty workflow as a starting point_

3. **Add a trigger for automation**
   - _Add Node → Cron_
   - _Add a Cron node as the first node_
   - _Configure it to run on your preferred schedule (e.g., every hour or once per day)_
   - _This ensures the workflow runs automatically without manual execution_

4. **Fetch stories with the HackerNews node**
   - _Add Node → Hacker News_
   - _Add the Hacker News node and connect it after the Cron node_
   - _Choose the operation (e.g., Get Top Stories or Get New Stories)_
   - _Optionally limit the number of stories (e.g., top 5–10)_
   - _Confirm the output includes fields like title, url, score, and time_

5. **Prepare data for the AI agent**
   - _Add Node → Item Lists / IF (optional)_
   - _Optionally add an Item Lists node to keep only the first N items_
   - _Or add an IF node to filter by score, keywords, or type_
   - _Pass only the items you want summarized to the AI node_

6. **Call the AI agent to summarize stories**
   - _An LLM transforms raw HackerNews JSON into short, readable summaries._
   - _Add Node → HTTP Request_
   - _Add an HTTP Request node after the HackerNews (or filter) node_
   - _Set Method to POST and URL to your LLM endpoint (e.g., OpenAI chat completions)_
   - _Add headers: Authorization: Bearer YOUR_API_KEY, Content-Type: application/json_
   - _In the JSON body, build a prompt that includes title, url, and text/story fields_
   - _Instruct the model to output concise, WhatsApp‑friendly summaries_

7. **Extract the AI‑generated summary**
   - _Add Node → Set_
   - _Add a Set node after the AI HTTP Request_
   - _Map the model’s response field (e.g., choices[0].message.content) into a new field like summary_
   - _Keep original fields like title and url for context in the final message_

8. **Format one or multiple summaries for WhatsApp**
   - _Add Node → Item Lists (Aggregate) or Set_
   - _Build a template like: 1. *Title*\nSummary\nLink_
   - _Store the final text in a field such as whatsappMessage_

9. **Send summaries via WhatsApp Cloud API**
   - _This step delivers the AI‑processed HackerNews summaries directly to your WhatsApp._
   - _Add Node → HTTP Request_
   - _Add another HTTP Request node for WhatsApp and connect it after the formatting step_
   - _Set Method to POST and URL to https://graph.facebook.com/v20.0/YOUR_PHONE_NUMBER_ID/messages_
   - _Add headers: Authorization: Bearer YOUR_WHATSAPP_TOKEN, Content-Type: application/json_
   - _In the JSON body, set to to your phone number and text.body to {{$json["whatsappMessage"]}} or the per‑item message_

10. **Test, refine, and enable the workflow**
   - _n8n → Execute Workflow / Activate_
   - _Run the workflow once manually to confirm each node behaves as expected_
   - _Check that HackerNews stories are fetched and AI summaries look clean and readable_
   - _Confirm WhatsApp messages arrive correctly formatted on your phone_
   - _Adjust schedule, number of stories, and prompt style as needed_
   - _Activate the workflow so it runs automatically on your chosen schedule_



## AI Agent. 
![aiagent](https://github.com/NowshadRuhan/AI-agent-for-extract-transform-HackerNews-readable-data-and-send-whatsapp/blob/main/Screenshot%202026-04-30%20at%2012.07.11%E2%80%AFAM.png?raw=true) 