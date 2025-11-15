graph TB
    subgraph "👥 Users"
        SB[SuperBuilders<br/>Browser]
        EXEC[Executives<br/>Browser]
    end
    
    subgraph "System 1: AI Manager"
        UI[Web Application<br/>━━━━━━━━━<br/>UI Platform<br/>TBD: Airtable or ToolJet<br/>Port: 3000]
        
        WORKFLOW[n8n<br/>━━━━━━━━━<br/>Workflow Engine<br/>Node.js<br/>Port: 5678]
        
        API[AI Manager API<br/>━━━━━━━━━<br/>FastAPI + LangGraph<br/>Python 3.11<br/>Port: 8000]
    end
    
    subgraph "System 2: AI Quality Platform"
        OBS[Observability Platform<br/>━━━━━━━━━<br/>TBD: LangSmith or Langfuse<br/>Port: 3001]
    end
    
    subgraph "System 3: Data Platform"
        NEON[(Neon PostgreSQL<br/>━━━━━━━━━<br/>Structured Data<br/>+ pgvector)]
        
        S3[(AWS S3<br/>━━━━━━━━━<br/>Documents<br/>Policies/SOPs)]
        
        REDIS[(Upstash Redis<br/>━━━━━━━━━<br/>Cache<br/>Sessions)]
    end
    
    subgraph "External Systems"
        WS[WorkSmart API]
        BL[BrainLift API]
        METRICS[Metrics API]
        LLM[OpenAI/Claude]
        SES[AWS SES]
    end
    
    subgraph "Development Tools (Not Deployed)"
        STUDIO[LangGraph Studio<br/>localhost:8123]
        SMITH[LangSmith<br/>SaaS]
    end
    
    %% User interactions
    SB -->|HTTPS| UI
    EXEC -->|HTTPS| UI
    
    %% Container interactions
    UI -->|REST API| API
    UI -->|REST API| WORKFLOW
    UI -->|View/Embed| OBS
    
    WORKFLOW -->|Webhook| API
    WORKFLOW -->|SMTP| SES
    
    API -->|SQL| NEON
    API -->|S3 API| S3
    API -->|Redis| REDIS
    API -->|SDK| OBS
    
    OBS -->|SQL| NEON
    
    %% External integrations
    API -->|REST| WS
    API -->|REST| BL
    API -->|REST| METRICS
    API -->|REST| LLM
    
    %% Development
    STUDIO -.->|Local dev| API
    SMITH -.->|Trace analysis| API
    
    %% Styling
    style UI fill:#4db6ac,stroke:#00796b,stroke-width:3px,color:#fff
    style WORKFLOW fill:#66bb6a,stroke:#2e7d32,stroke-width:3px,color:#fff
    style API fill:#4db6ac,stroke:#00796b,stroke-width:3px,color:#fff
    
    style OBS fill:#ffb74d,stroke:#ef6c00,stroke-width:3px,color:#fff
    
    style NEON fill:#9575cd,stroke:#4527a0,stroke-width:3px,color:#fff
    style S3 fill:#7e57c2,stroke:#311b92,stroke-width:3px,color:#fff
    style REDIS fill:#9575cd,stroke:#4527a0,stroke-width:3px,color:#fff
    
    style WS fill:#78909c,stroke:#37474f,stroke-width:2px,color:#fff
    style BL fill:#607d8b,stroke:#263238,stroke-width:2px,color:#fff
    style METRICS fill:#78909c,stroke:#37474f,stroke-width:2px,color:#fff
    style LLM fill:#546e7a,stroke:#263238,stroke-width:2px,color:#fff
    style SES fill:#607d8b,stroke:#263238,stroke-width:2px,color:#fff
    
    style STUDIO fill:#e0e0e0,stroke:#616161,stroke-width:2px,color:#000
    style SMITH fill:#e0e0e0,stroke:#616161,stroke-width:2px,color:#000
    
    style SB fill:#64b5f6,stroke:#1976d2,stroke-width:3px,color:#fff
    style EXEC fill:#1976d2,stroke:#0d47a1,stroke-width:3px,color:#fff
