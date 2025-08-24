# 🤖 Multi-Agent Research Workflow

This diagram illustrates the orchestrated workflow of specialized AI agents working together to conduct high-quality research using AutoGen and LangChain tools.

## Workflow Overview

```mermaid
flowchart TD
    %% Input and Setup
    A[📋 Task Query Input] --> B[🔧 Initialize Components]
    B --> C[🤖 Create Agent Team]
    
    %% Agent Setup
    C --> D[🧭 Planner Agent]
    C --> E[🔍 Researcher Agent] 
    C --> F[🧪 Critic Agent]
    C --> G[✍️ Editor Agent]
    C --> H[👨‍💼 Supervisor Agent]
    
    %% Tools Setup
    E --> I[🔍 Wiki Search Tool]
    E --> J[🌐 DuckDuckGo Search Tool]
    G --> K[💾 Save Report Tool]
    
    %% Workflow Start
    A --> L[🚀 Start SelectorGroupChat]
    L --> M{📝 Agent Selection}
    
    %% Planning Phase
    M -->|First| D
    D --> N[📋 Create Research Plan]
    N --> O[🔄 Send to Critic for Review]
    O --> F
    F --> P{✅ Plan Approved?}
    P -->|No| Q[📝 Revision Feedback]
    Q --> D
    P -->|Yes| R[✅ PLAN_COMPLETE]
    
    %% Research Phase
    R --> M
    M -->|Next| E
    E --> S[🔍 Execute Research]
    S --> T[📊 Use Search Tools]
    T --> I
    T --> J
    I --> U[📄 Gather Wikipedia Data]
    J --> V[🌐 Gather Web Data]
    U --> W[📝 Compile Research Draft]
    V --> W
    
    %% Review Phase
    W --> X[🔄 Send to Critic]
    X --> F
    F --> Y[🧪 Analyze Content]
    Y --> Z{📋 Content Quality Check}
    Z -->|Issues Found| AA[📝 Detailed Feedback]
    AA --> BB[🔄 Back to Researcher]
    BB --> E
    Z -->|Approved| CC[✅ APPROVED Signal]
    
    %% Editing Phase
    CC --> M
    M -->|Final| G
    G --> DD[✍️ Format & Structure]
    DD --> EE[📚 Add Citations]
    EE --> FF[💾 Save Final Report]
    FF --> K
    K --> GG[📄 Generate Timestamped File]
    GG --> HH[🏁 Add TERMINATE]
    
    %% Termination & Output
    HH --> II{🔚 Termination Check}
    II -->|TERMINATE Found| JJ[✅ Extract Final Report]
    II -->|Max Messages| KK[⚠️ Force Stop]
    II -->|Continue| M
    
    %% State Management
    JJ --> LL[💾 Save Team State]
    KK --> LL
    LL --> MM[📊 Return Results]
    
    %% Error Handling
    subgraph "🛡️ Error Handling"
        NN[❌ Exception Caught]
        OO[💾 Save State for Recovery]
        PP[🔄 Resume Option Available]
    end
    
    %% Supervisor Oversight
    H -.->|Monitors| M
    H -.->|Oversees| N
    H -.->|Validates| W
    H -.->|Approves| FF
    
    %% Error Flow
    S -.->|Error| NN
    Y -.->|Error| NN
    DD -.->|Error| NN
    NN --> OO
    OO --> PP
    
    %% Styling
    classDef agentNode fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef toolNode fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef processNode fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    classDef decisionNode fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef terminalNode fill:#ffebee,stroke:#b71c1c,stroke-width:2px
    
    class D,E,F,G,H agentNode
    class I,J,K toolNode
    class N,S,Y,DD,EE processNode
    class M,P,Z,II decisionNode
    class JJ,MM terminalNode
```

## Workflow Phases

### 🧭 **Phase 1: Planning**
- **Planner Agent** creates detailed research strategy
- **Critic Agent** reviews and provides feedback
- Iterative refinement until `PLAN_COMPLETE`

### 🔍 **Phase 2: Research**
- **Researcher Agent** executes approved plan
- Uses Wikipedia and DuckDuckGo search tools
- Compiles comprehensive research draft

### 🧪 **Phase 3: Quality Assurance**
- **Critic Agent** analyzes research for accuracy and completeness
- Provides detailed feedback or `APPROVED` signal
- Ensures source citations and logical structure

### ✍️ **Phase 4: Editorial**
- **Editor Agent** formats and structures content
- Adds proper citations and professional styling
- Saves final report with timestamp
- Signals `TERMINATE` for completion

### 👨‍💼 **Phase 5: Orchestration**
- **Supervisor Agent** provides oversight throughout
- **SelectorGroupChat** manages agent selection
- State management for recovery and debugging

## Key Features

- 🛡️ **Error Recovery**: Automatic state saving and resume capability
- 🔄 **Iterative Refinement**: Multiple review cycles ensure quality
- 📊 **Comprehensive Logging**: Full execution trail with structured logs
- 🎯 **Termination Safety**: Multiple termination conditions prevent infinite loops
- 🔧 **Tool Integration**: Seamless LangChain tool usage for data gathering