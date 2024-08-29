# RAG를 활용한 LLM Application 개발 (feat. LangChain)

## 프로젝트 요약
- 인프런의 [RAG를 활용한 LLM Application 개발](https://inf.run/biyZk) 강의자료입니다
- 한국의 소득세법을 활용해서 RAG를 구성하고 `LangChain`의 `ChatOpenAI`클래스를 활용하여 LLM과 연결합니다.
- keyword 사전을 활용해서 retrieval 성능을 개선합니다

### 3.1 환경 설정과 LangChain의 ChatOpenAI를 활용한 검증
- **패키지 설치 및 환경 변수 설정**: `python-dotenv`와 `langchain-openai` 패키지를 설치하고 `.env` 파일을 통해 OpenAI API 키를 설정합니다.
- **LangChain의 ChatOpenAI 사용**: `ChatOpenAI` 클래스를 사용하여 OpenAI의 언어 모델을 통해 질문에 대한 답변을 생성합니다.
- **실제 예제**: "인프런에 어떤 강의가 있나요?"라는 질문에 대해 인프런의 강의 내용과 특징을 설명하는 답변을 생성합니다.

### 3.2 LangChain과 Chroma를 활용한 RAG 구성
- **데이터 생성 및 분할**: `RecursiveCharacterTextSplitter`를 사용하여 문서를 chunk로 분할하고, `Docx2txtLoader`를 통해 데이터를 로드합니다.
- **데이터 임베딩 및 저장**: OpenAI의 임베딩 모델을 사용하여 chunk를 벡터화하고, `Chroma`를 통해 벡터화된 데이터를 데이터베이스에 저장합니다.
- **질의 응답 생성**: 저장된 데이터를 유사도 검색을 통해 검색하고, `RetrievalQA` 체인을 사용하여 질문에 대한 답변을 생성합니다.

### 3.3 LangChain 없이 구성하는 RAG의 불편함
- **데이터 생성 및 분할**: `python-docx`와 `tiktoken`을 사용하여 문서를 chunk로 분할하고, 텍스트 데이터를 생성합니다.
- **데이터 임베딩 및 저장**: OpenAI의 임베딩 모델을 사용하여 chunk를 벡터화하고, `Chroma`를 통해 벡터화된 데이터를 데이터베이스에 저장합니다.
- **질의 응답 생성**: 저장된 데이터를 유사도 검색을 통해 검색하고, OpenAI의 언어 모델을 사용하여 질문에 대한 답변을 생성합니다.

### 3.4 LangChain을 활용한 Vector Database 변경 (Chroma ➡️ Pinecone)

- **데이터 생성 및 분할**: `Docx2txtLoader`와 `RecursiveCharacterTextSplitter`를 사용하여 문서를 로드하고 chunk로 분할합니다.
- **벡터 데이터베이스 변경**: 기존 Chroma 벡터 데이터베이스를 Pinecone으로 변경하여 문서 임베딩을 저장하고 검색합니다.
- **질의 응답 생성**: `RetrievalQA` 체인을 사용하여 저장된 벡터 데이터베이스에서 문서를 검색하고, OpenAI의 언어 모델을 통해 질문에 대한 답변을 생성합니다.

### 3.5 Retrieval 효율 개선을 위한 데이터 전처리

- **데이터 전처리 및 분할**: `Docx2txtLoader`와 `RecursiveCharacterTextSplitter`를 사용하여 문서를 로드하고 chunk로 분할합니다.
- **벡터 데이터베이스 설정**: Pinecone을 사용하여 벡터 데이터베이스를 설정하고, OpenAI 임베딩 모델을 통해 문서 임베딩을 저장합니다.
- **질의 응답 생성**: `RetrievalQA` 체인을 사용하여 저장된 벡터 데이터베이스에서 문서를 검색하고, OpenAI의 언어 모델을 통해 질문에 대한 답변을 생성합니다.


### 3.6 Retrieval 효율 개선을 위한 키워드 사전 활용방법

- **데이터 로드 및 분할**: `Docx2txtLoader`와 `RecursiveCharacterTextSplitter`를 사용하여 문서를 로드하고 chunk로 분할합니다.
- **벡터 데이터베이스 설정**: Pinecone을 사용하여 벡터 데이터베이스를 설정하고, OpenAI 임베딩 모델을 통해 문서 임베딩을 저장합니다.
- **질의 응답 생성**: `RetrievalQA` 체인을 사용하여 저장된 벡터 데이터베이스에서 문서를 검색하고, OpenAI의 언어 모델을 통해 질문에 대한 답변을 생성합니다.
- **사전 체인의 중요성**: `dictionary_chain`을 사용하여 사용자의 질문을 사전에 정의된 키워드로 수정함으로써, 더 정확한 검색 결과와 답변을 제공합니다.

### 5.1 LangSmith를 활용한 LLM Evaluation
- **데이터 생성 및 로드**: `langsmith`를 사용하여 소득세법 관련 질문-답변 쌍을 생성하고, 데이터를 로드합니다.
- **벡터 데이터베이스 설정**: Pinecone을 사용하여 벡터 데이터베이스를 설정하고, OpenAI 임베딩 모델을 통해 문서 임베딩을 저장합니다.
- **질의 응답 생성**: `RagBot` 클래스를 사용하여 저장된 벡터 데이터베이스에서 문서를 검색하고, OpenAI의 언어 모델을 통해 질문에 대한 답변을 생성합니다.
- **평가 및 검증**: `langsmith`의 평가 도구를 사용하여 생성된 답변의 정확성, 유용성, 그리고 환각 여부를 평가합니다.
