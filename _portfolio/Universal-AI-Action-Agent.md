---
title: "Universal-AI-Action-Agent"
excerpt: "An AI‑powered multi‑action assistant built in n8n that sends emails, fetches real‑time news, and creates meeting invitations from natural language commands. The AI Agent interprets each message, selects the right tool, and executes tasks automatically through Gmail, News API, and Google Calendar.<br/><img src='/images/email-ai-agent.png'>"
collection: portfolio
---

This project implements a fully autonomous AI Agent workflow in n8n that transforms simple chat messages into real actions. When a user sends a message, the AI Agent analyzes the intent using the Gemini Chat Model and determines whether the user wants to send an email, retrieve the latest news, or schedule a meeting. The agent then activates the appropriate tool: Gmail for composing and sending emails, News API for fetching up‑to‑date headlines across any category, and Google Calendar for generating meeting invitations with precise details. A built‑in memory module helps the agent maintain context across interactions, enabling more natural and intelligent conversations. The final result is returned to the user through the chat interface, creating a smooth, end‑to‑end automation system that behaves like a personal AI assistant capable of handling communication, information retrieval, and scheduling tasks effortlessly.

## How It Works

1. **Chat Trigger Node**
   - *Receives incoming user messages and passes the text into the workflow for processing.*

2. **AI Agent Node**
   - *Uses an LLM to interpret the user’s intent, decide whether the request is about sending an email, fetching news, or creating a meeting, and then selects the correct tool automatically.*

3. **Gmail / News API / Google Calendar Nodes**
   - *Executes the required action: sends emails through Gmail, retrieves real‑time news via the News API, or creates meeting invitations in Google Calendar.*

4. **Response Node**
   - *Sends a confirmation or result back to the user, completing the conversational loop.*

## AI Agent. 
![aiagent](https://github.com/NowshadRuhan/Universal-AI-Action-Agent/blob/main/email-ai-agent.png?raw=true) 

## AI Agent: workflow video: Watch the video

<!-- Basic YouTube embed -->
<iframe
  width="560"
  height="315"
  src="https://www.youtube.com/embed/WT2Rl-X_QvY"
  title="YouTube video player"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
  referrerpolicy="strict-origin-when-cross-origin"
  allowfullscreen>
</iframe>
