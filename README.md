# PUP Enrollment Buddy  
*A Langflow + Gemini + AstraDB RAG Project*

## Overview

This project is a **personalized AI Enrollment Assistant** built during the **Langflow Workshop**.

The objective was to create a system that can answer questions about **PUP enrollment** using official admission documents — while remembering the student it is interacting with.

Before this workshop, I had **no experience using Langflow or Astra DB**. This project became my first hands-on exposure to building a full **Retrieval-Augmented Generation (RAG)** system using a visual workflow tool and a vector database.

Instead of building a basic chatbot, this project implements a structured AI system that:

- Personalizes responses using the student’s name
- Stores and retrieves conversation history
- Searches official enrollment documents before answering
- Reduces hallucinated or fabricated information

This repository contains the **Langflow workflow file** used to build the system.

---

# Why This Project Matters

Enrollment season can be overwhelming for students. Common questions include:

- What documents are required?
- Who is eligible for PUPCET?
- When is the enrollment deadline?
- Where should requirements be submitted?

Instead of manually searching through long PDF files or waiting for responses from offices, this assistant retrieves relevant information directly from official documents and generates grounded responses.

The goal is not just automation — it is **responsible AI usage**, where answers are tied to real sources.

---

# What This Project Does

## 1. Personalizes Conversations

The system asks for the student’s name and uses it as:

- A **session ID**
- A **memory identifier**
- A **dynamic variable inside the prompt**

Each student gets their own conversation history.

---

## 2. Uses Memory

The **Message History node** stores:

- Previous questions
- Past responses
- The student identity

This makes conversations **continuous instead of isolated interactions**.

---

## 3. Uses Google Gemini as the Language Model

**Model used**

```
gemini-2.5-flash-lite
```

**Temperature**

```
0.1
```

Low temperature reduces randomness and improves factual consistency.

---

## 4. Implements Retrieval-Augmented Generation (RAG)

Instead of answering freely, the system:

1. Searches Astra DB for relevant document chunks  
2. Retrieves the most relevant sections  
3. Injects them into the prompt as context  
4. Generates answers grounded in that context  

This significantly **reduces hallucinations** and improves reliability.

---

# System Architecture

## Core Flow

```
Text Input (Student Name)
        ↓
Chat Input
        ↓
Message History
        ↓
Prompt Template
        ↓
Google Gemini LLM
        ↓
Chat Output
```

## RAG Extension

```
Chat Input → Astra DB (Search Query)
Astra DB → Parser → Prompt Template ({context})
```

---

# What I Learned

## Learning Langflow

Before this project, I had **never used Langflow**.

Through building this system, I learned:

- How visual workflow builders represent AI pipelines
- How prompts, memory, embeddings, and models connect together
- How to configure nodes properly and connect data types
- How to debug broken connections between components
- How system design matters more than just model choice

Langflow helped me understand **AI architecture visually instead of only through code**. It made the structure of RAG systems clearer and easier to reason about.

---

## Learning Astra DB

I also had **no prior experience with Astra DB or vector databases**.

From this project, I learned:

- What embeddings actually are
- How vector similarity search works
- Why chunk size and overlap matter
- How documents are converted into embeddings
- How retrieval improves model grounding
- The importance of embedding dimensions matching the model

I learned that Astra DB does not just store text — it stores **numerical representations of meaning**. That realization changed how I think about AI systems.

---

## Broader Technical Growth

This project helped me understand:

- The difference between a simple chatbot and a grounded RAG system
- Why hallucination happens
- How to control model output using context
- How memory improves user experience
- How real-world AI systems separate data from logic

More importantly, I learned that **responsible AI requires controlling the source of truth**.

---

# Repository Contents

This repository contains:

```
PUP Enrollment Buddy.json
```

The official **PUP Admission PDF** is **NOT included** in this repository.

You must upload your own copy of the **official PUP Admission Guidelines PDF** when running the project.

---

# How To Use This Project (Step-by-Step)

## Step 1 — Import the Workflow

1. Log in to **Astra Portal (DataStax)**
2. Open **Langflow**
3. Click **Import**
4. Upload:

```
PUP Enrollment Buddy.json
```

---

## Step 2 — Add Required API Keys

You will need:

- Google AI Studio API Key
- Astra DB Application Token

In Langflow:

1. Open the **Google Language Model node**
2. Paste your **Google API Key**
3. Open the **Astra DB node**
4. Paste your **Astra Application Token**

---

## Step 3 — Upload the PUP Admission PDF

Since the PDF is not included:

1. Locate the **Read File node** in the workflow
2. Upload your copy of the **official PUP Admission Guidelines PDF**
3. Ensure it connects to:

- Split Text node
- Google Embeddings node
- Astra DB Ingest node

This step converts the PDF into **embeddings** and stores them inside **Astra DB**.

---

## Step 4 — Run Document Ingestion

Before asking questions:

1. Run the flow once to ingest the document
2. Confirm that **Astra DB contains stored embeddings**

Configuration used:

**Collection name**

```
requirements_vector
```

**Embedding model**

```
models/gemini-embedding-001
```

**Embedding dimension**

```
768
```

If the database is empty, the assistant will **not retrieve context**.

---

## Step 5 — Test in Playground

Open **Playground** and try asking:

- What are the requirements for enrollment?
- Who is eligible for PUPCET?
- What documents are required for freshmen?
- When is the enrollment deadline?

The assistant will:

1. Search Astra DB  
2. Retrieve relevant document chunks  
3. Inject context into the prompt  
4. Generate a grounded response  

---

# Limitations

This system has clear boundaries:

- It only knows what is inside the uploaded PDF
- If information is missing from the document, it cannot answer
- It does not connect to live PUP databases
- It does not replace official confirmation from the Registrar
- If the document says **“TBA”**, the assistant will respond with **“To Be Announced”**
- Incorrect embedding setup will affect retrieval quality

This design prioritizes **grounded responses over creativity**.

---

# Acknowledgment

Special thanks to **Mr. Joshua Mistal** for guiding the **Langflow Workshop** and providing the foundational structure for this project.

The walkthrough on **RAG architecture, embeddings, and system design** helped clarify not just how to build the system, but **why each component is necessary**.

---

# Author

**Ayen**  
Langflow Workshop Participant

This project represents my **first hands-on implementation of a Langflow-based RAG system using Astra DB.**
