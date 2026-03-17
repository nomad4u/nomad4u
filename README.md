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
graph TD
    %% 외부 사용자 및 인터페이스
    User((사용자)) --> |"영입 조건 입력"| UI[Streamlit UI]
    UI --> |"API 요청"| Backend[FastAPI Backend]

    %% LangGraph 핵심 워크플로우
    subgraph "LangGraph Multi-Agent Workflow"
        Start((시작)) --> DataAgent[데이터 수집 에이전트\n(Pandas Tool)]
        
        %% 병렬 분석 처리
        DataAgent --> SaberAgent[세이버메트릭스 에이전트\n(데이터 심층 분석)]
        DataAgent --> ScouterAgent[스카우터 에이전트\n(RAG: 부상/멘탈 평가)]
        DataAgent --> FinanceAgent[재무 담당 에이전트\n(예산 및 가성비 검토)]
        
        SaberAgent --> GM_Review{단장(GM) 에이전트\n최종 종합 검토}
        ScouterAgent --> GM_Review
        FinanceAgent --> GM_Review
        
        %% 피드백 루프 (Conditional Edge)
        GM_Review -- "반려 (예산초과/부상위험 등)" --> DataAgent
        GM_Review -- "최종 승인" --> ReportGen[최종 리포트 생성 노드]
    end

    %% 데이터 저장소 연동
    CSV[(KBO 24-25 성적.csv)] -.-> |"정량 지표 검색"| DataAgent
    VDB[(Chroma Vector DB)] -.-> |"유사도 검색(RAG)"| ScouterAgent
    Text[(스카우팅/세이버 가이드.txt)] -.-> |"임베딩"| VDB

    %% 결과 출력
    Backend -.-> |"State Graph 실행"| Start
    ReportGen --> End((종료))
    End -.-> |"리포트 JSON 반환"| Backend
    Backend --> |"결과 렌더링"| UI
```
