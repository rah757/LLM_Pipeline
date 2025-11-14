## Overview of the Data Question Answering Pipeline

This system is designed to answer user questions about their data by combining metadata, retrieval augmented generation techniques, and large language models.

---

## Key Components

### 1. User Query Interface
The user submits a question such as "Ask about my data". This query is passed to the backend processing system.

### 2. Metadata Sources
The system checks several types of metadata stored in Azure Data Lake:
- **Table level metadata** such as column information, data statistics, and table headers.  
- **Workspace level metadata** such as catalog descriptions and story level summaries.  
- **Context files** that contain additional information like dataset background or common Q and A.  
- **Cache** that stores around fifty pre generated common answers to speed up responses.

### 3. SQL and RAG Layer
The query goes through SQL lookups and a RAG engine to decide:
- Whether the metadata is sufficient to answer the question.  
- Whether deeper reading of PDFs or tables is required.

### 4. PDF and Table Readers
If needed, the system reads:
- **PDF documents** for unstructured insights.  
- **Tables** for structured data.  
If the table is small, the system performs a vector search and reads the entire table.

### 5. Decision Logic
A size check determines how the data is processed:
- **Small tables** are fully vectorized and scanned.  
- **Large tables** rely on metadata and cached results.

### 6. LLM Response Generator
After retrieval, the combined information is passed into the LLM.  
The LLM generates the final answer to return to the user.

---

## How the Components Connect

1. The user query enters the system and is routed to the metadata layer.  
2. Metadata responses, cache results, and SQL lookups are collected.  
3. The RAG system determines if additional retrieval is required.  
4. If needed, the PDF reader or table reader fetches deeper content.  
5. All retrieved information is aggregated.  
6. The LLM composes the final answer and delivers it back to the user.

This creates an end to end intelligent question answering system that blends metadata, retrieval, and LLM reasoning to produce accurate responses about user data.
