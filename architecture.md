graph TD
    %% Define Styles
    classDef source fill:#1e3a8a,stroke:#fff,stroke-width:2px,color:#fff,rx:5px,ry:5px;
    classDef ingest fill:#047857,stroke:#fff,stroke-width:2px,color:#fff,rx:5px,ry:5px;
    classDef process fill:#b45309,stroke:#fff,stroke-width:2px,color:#fff,rx:5px,ry:5px;
    classDef llm fill:#4c1d95,stroke:#fff,stroke-width:2px,color:#fff,rx:5px,ry:5px;
    classDef output fill:#be123c,stroke:#fff,stroke-width:2px,color:#fff,rx:5px,ry:5px;

    %% Layer 1: Data Sources (The Silos)
    subgraph Siloed CCB Data Sources
        C[Cards: Dispute Logs]:::source
        A[Auto: Underwriting Notes]:::source
        H[Home: Appraisals]:::source
        B[Business: Merchant Disputes]:::source
    end

    %% Layer 2: Ingestion & Processing
    subgraph Ingestion & Processing Layer
        I[Unified OCR & Parsing API]:::ingest
        PII[PII Masking & Hashing]:::ingest
        CH[Semantic Chunking & Metadata Tagging]:::ingest
    end

    %% Layer 3: Vectorization & Storage
    subgraph Long-Term Memory
        EMB[Embedding Model]:::process
        VDB[(Enterprise Vector DB <br> FAISS / Pinecone)]:::process
    end

    %% Layer 4: Retrieval & Generation (The Brain)
    subgraph Agentic Execution Layer
        Q([User Query / Automated Alerting]):::llm
        RAG[Semantic Retrieval Engine]:::llm
        LLM{Internal Secure LLM <br> Azure OpenAI / Mistral}:::llm
        VAL[Deterministic Guardrails <br> SQL Cross-Reference]:::llm
    end

    %% Layer 5: Output
    subgraph Business Intelligence
        DASH[CCB Risk Intelligence Dashboard]:::output
        ALERT[Early-Warning Default Alerts]:::output
    end

    %% Data Flow
    C --> I
    A --> I
    H --> I
    B --> I
    
    I --> PII
    PII --> CH
    CH --> EMB
    EMB --> VDB
    
    Q --> RAG
    VDB -. "Similar Vectors" .-> RAG
    RAG --> LLM
    LLM --> VAL
    
    VAL -- "Hallucination Blocked" --> RAG
    VAL -- "Data Verified" --> DASH
    VAL --> ALERT
