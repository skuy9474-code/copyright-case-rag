# 저작권 판례 RAG

저작권 판례를 검색하고 근거 기반 답변을 생성하는 RAG 실습 프로젝트입니다.  
Baseline의 검색 실패 원인을 분석한 뒤, 검색 파이프라인을 개선하고 같은 평가 질문으로 결과를 비교했습니다.

## 문제 정의

Baseline은 Dense Similarity Top-3만 사용했습니다. 이 방식은 정확한 법률 용어가 중요한 질문에서 정답 판례를 놓칠 수 있고, 고정 길이 Chunk가 판결 이유와 번호 문단을 나누는 문제가 있었습니다.

## 적용한 방법

- 판례 구조 기반 Chunking
- BM25와 Dense MMR을 결합한 Hybrid Retrieval
- Reciprocal Rank Fusion(RRF)
- Evidence Verification과 1회 재검색

Baseline과 Advanced 모델은 같은 판례 데이터, 임베딩 모델, LLM, 평가 질문을 사용했습니다. 검색 파이프라인만 변경했습니다.

## 평가 결과

평가 질문은 총 6개입니다.

| 지표 | Baseline | Advanced | 차이 |
|---|---:|---:|---:|
| Hit@3 | 0.500 | 0.500 | +0.000 |
| MRR@3 | 0.417 | 0.500 | +0.083 |
| Correctness | 3.833 | 4.333 | +0.500 |
| Groundedness | 4.833 | 4.667 | -0.167 |

검색 순위와 답변 정확성은 개선됐지만 Groundedness는 0.167 낮아졌습니다. 재검색 과정이 새로운 잡음을 만들었는지 질문별 분석이 필요합니다.

## 실행 방법

1. `Copyright_RAG_Advanced.ipynb`를 Google Colab에서 엽니다.
2. Colab Secrets에 `OPENAI_API_KEY`를 등록합니다.
3. 아래 두 파일을 업로드합니다.
   - `Copyright_case_law_Supreme.xlsx`
   - `Copyright_RAG_Evaluation.xlsx`
4. 위에서부터 셀을 순서대로 실행합니다.

## 데이터 공개 범위

판례 데이터와 평가용 Excel 파일은 이 저장소에 포함하지 않았습니다. 데이터의 이용 조건을 확인한 뒤 본인이 사용할 수 있는 파일을 직접 업로드해야 합니다.

## 한계

- 평가 질문이 6개이므로 전체 저작권 질의 성능으로 일반화할 수 없습니다.
- Correctness와 Groundedness 평가는 LLM Judge 결과입니다.
- API 비용과 모델 버전에 따라 재실행 결과가 달라질 수 있습니다.

