## Hi there 👋

<!--
**nomad4u/nomad4u** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

```mermaid
graph TB
    subgraph Client_Layer ["Frontend: Streamlit"]
        User(["사용자"]) --> UI["입력 폼 & 히스토리 뷰어"]
        UI --> SS["Session State: 검색 이력 저장"]
    end

    subgraph Backend_Layer ["API: FastAPI"]
        UI --> API["POST /api/v1/gm-report"]
        API --> Validator{"Pydantic <br/>입력 검증"}
    end

    subgraph LangGraph_Agent_Framework ["Multi-Agent Orchestration"]
        Validator --> Supervisor["<b>GM Supervisor (단장)</b><br/>상태 분석 및 라우팅 판단"]
        
        %% Checkpointer
        Supervisor -.-> Memory[("MemorySaver<br/>Checkpointer")]

        %% Nodes
        Supervisor -->|"1. 명단 필요"| DataNode["<b>Data Agent</b><br/>ReAct: CSV 능동 검색"]
        Supervisor -->|"2. 지표 분석"| SaberNode["<b>Saber Agent</b><br/>Pydantic: 세이버 분석"]
        Supervisor -->|"3. 정성 평가"| ScoutNode["<b>Scout Agent</b><br/>ReAct: RAG 능동 검색"]
        Supervisor -->|"4. 예산 검토"| FinanceNode["<b>Finance Agent</b><br/>Pydantic: 가성비 평가"]

        %% Returns to Supervisor
        DataNode --> Supervisor
        SaberNode --> Supervisor
        ScoutNode --> Supervisor
        FinanceNode --> Supervisor

        %% Decision Logic
        Supervisor -->|"REJECT: 조건 미달"| DataNode
        Supervisor -->|"APPROVE: 최종 승인"| Finish(["FINISH: 최종 리포트 생성"])
    end

    subgraph Data_Layer ["Knowledge Base"]
        DataNode --- CSV[("KBO 정형 데이터<br/>CSV Files")]
        ScoutNode --- FAISS[("FAISS Vector DB<br/>Scouting Reports")]
    end

    %% Styling
    style Supervisor fill:#f96,stroke:#333,stroke-width:2px
    style Finish fill:#00c853,stroke:#333,color:#fff
    style Memory fill:#e1f5fe,stroke:#01579b
```
