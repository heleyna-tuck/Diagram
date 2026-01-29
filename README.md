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

  subgraph CACHE_ROW[""]
    direction LR
    C --- TTL["TTL (1) ... (1 week)"]
  end

  C <--> J["LLM Eval Judge"]

  subgraph JUDGE_ROW[""]
    direction LR
    J --- RG["RAGAS"]
  end

  J --> S["Solution"]
  S --> UI["UI"]
```
