---
title: "RAG-AI-agent (Personal Assistant)"
excerpt: "A fully automated RAG AI agent that ingests new Google Drive files, generates embeddings, stores them in Pinecone, and uses an AI Agent with memory and retrieval to answer chat queries using OpenAI models for accurate, context‑aware responses.<br/><img src='/images/rag-ai-agent-full-workflow.png'>"
collection: portfolio
---

## Rag AI Agent.

![aiagent](https://github.com/NowshadRuhan/RAG-AI-agent/blob/main/rag-ai-agent-full-workflow.png?raw=true)

This workflow automates a complete RAG-powered AI agent pipeline. When a new file is added to Google Drive, it is automatically downloaded, split into chunks, embedded, and stored in Pinecone as a vector database. On the chat side, incoming user messages trigger an AI Agent that retrieves relevant context from Pinecone, combines it with OpenAI’s chat model and memory, and generates accurate, context‑aware responses. The system seamlessly connects document ingestion, vector storage, retrieval, and conversational reasoning into one unified intelligent assistant.

## Explain in below: step by step

1. File arrives in Google Drive; Trigger:
   - A new file is created/added in a specific Google Drive folder.
   - This event starts the workflow automatically (no manual action needed).
2. Download the file; Action:
   - The workflow downloads the newly added file from Google Drive.
   - At this point you have the raw document content available for processing.
3. Split the document into chunks; Action:
   - The file content is passed to a text splitter (e.g., recursive character splitter).
   - The document is broken into smaller, overlapping chunks so each piece fits into the context window and remains semantically meaningful.
4. Create embeddings for each chunk; Action:
   - Each text chunk is sent to an embedding model (e.g., OpenAI embeddings).
   - The model converts each chunk into a vector (a list of numbers) that captures its semantic meaning.
5. Store vectors in Pinecone; Action:
   - Each embedding, along with its original text and metadata (e.g., file name, chunk index), is stored in a Pinecone index.
   - Pinecone becomes your vector database, optimized for fast similarity search over all chunks.

#### Explain in above. (Rag data processing)

![aiagent](https://github.com/NowshadRuhan/RAG-AI-agent/blob/main/rag-data-processing.png?raw=true)

6. Wait for a chat message; Trigger:
   - On the “chat side,” a user sends a message (e.g., via a chat UI, API, or messaging app).
   - This incoming message triggers the AI Agent workflow
7. Embed the user’s query; Action:
   - The user’s question is embedded using the same embedding model.
   - This produces a query vector in the same space as the stored document vectors.
8. Retrieve relevant context from Pinecone; Action:
   - The query vector is sent to Pinecone for similarity search.
   - Pinecone returns the most relevant chunks (top‑k matches) from your stored documents.
   - These chunks form the retrieval context.
9. Combine context, memory, and chat model; Action:
   - The AI Agent takes:
     - the user’s question
     - the retrieved chunks from Pinecone
     - any stored conversation memory (previous messages, state)
   - It builds a prompt that includes both the question and the relevant context.
   - This prompt is sent to an OpenAI chat model (e.g., GPT‑style model).
10. Generate the final answer; Action:

- The chat model uses the retrieved context plus conversation history to generate a grounded, context‑aware response.
- The AI Agent may also update its memory with the new interaction.

11. Send the response back to the user; Action:

- The generated answer is returned to the chat channel (web app, messaging app, etc.).
- From the user’s perspective, they just asked a question and got a smart, document‑aware reply.

#### Explain in above.(Rag ai-agent)

![aiagent](https://github.com/NowshadRuhan/RAG-AI-agent/blob/main/rag-ai-agent.png?raw=true)

## AI Agent: workflow video: Watch the video

<!-- Basic YouTube embed -->
<iframe
  width="560"
  height="315"
  src="https://www.youtube.com/embed/a-7ZzCNJ-xk"
  title="YouTube video player"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
  referrerpolicy="strict-origin-when-cross-origin"
  allowfullscreen>
</iframe>
