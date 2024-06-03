# Knowledge Graph Prompting for Multi-Document Question Answering

키워드: KGQA
URL: https://arxiv.org/pdf/2308.11730
사람: 김민지
연도: 2024
학회: AAAI

### **Motivation**

- **MDQA의 어려운 점**
    - 여러 다른 문서를 참조해 답을 내야 함.
    - 하나의 문서는 여러 구조를 가진 데이터의 집합임.

### **Contribution**

- **Knowledge Graph Prompting**
    1. KG 생성법 제시
    2. 질문 종류(Content-based/Structure-based)에 따라 다른 KG traversal agent 제안
        - KG안 노이즈는 피하고, 관련 있는 neighbor을 찾기 위해!

---

### **1. KG 생성법 제시**

<aside>
⭐ 노드 $v_i$: 각 passage
Feature $\mathcal{X}_i$: passage의 text
엣지: passage 노드의 lexical/semantic 유사도로 생성함.

</aside>

- TD-IDF KG 생성법 (lexical similarity)
    - 각 문서에 TD-IDF keyword extraction 수행 + 각 문서의 제목을 keword space $\mathcal{W}$로 함.
    - 공통된 keyword를 가진 두 개의 passage를 연결 (lexical similarity)
- KNN-ST KG 생성법 (semantic similarity)
    - Sentence transformers를 이용하여서 passage embedding $X_i$를 생성하고, KNN 그래프를 만들어서 유사도 계산
- KNN-MDR KG 생성법
    - MDR (Multi-hop Dense Retrieval)
    - 기존의 supporting facts를 바탕으로 다음으로 올 supporting facts를 예측하도록 sentence encoder를 훈련시킴.
    - 마찬가지로 KNN 그래프를 만들어서 두 passage 사이의 유사도를 알아냄.
- TAGME
    - 각 passage에서 Wikipedia entity를 추출하여 공통된 Wikipedia entity를 가진 두 passage를 연결함.

![Untitled](Knowledge%20Graph%20Prompting%20for%20Multi-Document%20Quest%20ffcf08b5154a429da9e1aef90942aad6/Untitled.png)

- Extract-PDF ([https://developer.adobe.com/document-services/docs/overview/pdf-extract-api/](https://developer.adobe.com/document-services/docs/overview/pdf-extract-api/))
    - 문서의 구조를 추출하여 구조를 담은 노드를 그래프에 추가함.
    - 이 문서에서는 page와 table 구조만 고려함.
        - table 노드의 feature는 table markdown임.
            
            ![Untitled](Knowledge%20Graph%20Prompting%20for%20Multi-Document%20Quest%20ffcf08b5154a429da9e1aef90942aad6/Untitled%201.png)
            
            - Markdown table의 단점: ChatGPT의 input token limitation으로 너무 긴 table은 LLM의 input으로 넣기에 힘듦. ➡️ ***미래 연구에 table을 SQL prompt나 grid graph로 넣는 것에 대해 고려해볼 수 있음.***
        - page노드의 feature는 page number임.
        - page노드와 그 page에 있는 sentence/table노드를 연결함.
            
            ![Untitled](Knowledge%20Graph%20Prompting%20for%20Multi-Document%20Quest%20ffcf08b5154a429da9e1aef90942aad6/Untitled%202.png)
            

### **2. 질문 종류에 따라 다른 KG traversal agent 제안**

1. Content-based Question
    
    ![Untitled](Knowledge%20Graph%20Prompting%20for%20Multi-Document%20Quest%20ffcf08b5154a429da9e1aef90942aad6/Untitled%203.png)
    
    ![Untitled](Knowledge%20Graph%20Prompting%20for%20Multi-Document%20Quest%20ffcf08b5154a429da9e1aef90942aad6/Untitled%204.png)
    
2. Structure-based Question
    
    ![Untitled](Knowledge%20Graph%20Prompting%20for%20Multi-Document%20Quest%20ffcf08b5154a429da9e1aef90942aad6/Untitled%205.png)
    
    ![Untitled](Knowledge%20Graph%20Prompting%20for%20Multi-Document%20Quest%20ffcf08b5154a429da9e1aef90942aad6/Untitled%206.png)
