```mermaid
flowchart TD
  U["User Prompt"] --> R["Orch LLM (Router)"]

  subgraph RET["Retrieval / Index"]
    F["FAISS"] --- H["HNSW"]
    H --- Q["Flat 32-bit"]
  end

  R --> E["Eval (Check relevance)"]
  E -->|Relevant| KB["Knowledge Base: Data1, Data2, ... DataN"]
  E -.->|If needed| W["Web API"]

  KB --> A["Augment context"]
  W --> A
  A --> L["LLM"]

  L --> C["Semantic Cache"]
  C <--> J["LLM Eval Judge"]

  %% Bypass loop around Semantic Cache:
  %% top path (LLM -> Judge)
  L -.-> UP[" "]
  UP -.-> J

  %% bottom path (Judge -> LLM)
  J -.-> DOWN[" "]
  DOWN -.-> L

  J --> S["Solution"]
  S --> UI["UI"]
```
