# 허찬 | Backend Developer

데이터가 흐르는 구조를 설계하고, 직접 만든 서비스의 문제를 끝까지 추적해 개선하는 백엔드 개발자를 지향합니다.
Java · Spring Boot 기반 REST API 개발과 Python 기반 RAG 서비스 개발 경험이 있습니다.

<br>

## Tech Stack

| 분류 | 기술 |
|---|---|
| Language | Java, Python, SQL, JavaScript |
| Backend | Spring Boot, Spring Data JPA, Flask |
| Database | MySQL / MariaDB, ChromaDB (Vector DB) |
| AI / Data | RAG, Sentence-Transformers, LLM API (Groq) |
| Test | JUnit 5, MockMvc, H2 |
| Infra / Tool | Docker, Docker Compose, Gradle, Git / GitHub, Postman |

<br>

## Projects

### [EcoLink BinGo](https://github.com/Chan-pj/Eco-Link) — IoT 스마트 쓰레기통 수거 최적화 시스템
`Java 17` `Spring Boot` `Spring Data JPA` `MariaDB` `Docker` · 4인 팀 프로젝트 · 백엔드 담당

초음파 센서로 쓰레기통 적재량을 수집하고, AI 예측과 경로 최적화로 수거가 필요한 쓰레기통만 효율적으로 수거하도록 돕는 서비스

- Controller · Service · Repository 레이어드 구조로 6개 도메인의 REST API 설계 및 구현
- JPA 연관관계 매핑과 DB 스키마 설계, 지연 로딩 프록시 직렬화 오류(500) 해결
- 응답 DTO 분리로 비밀번호 노출 차단, `@RestControllerAdvice` 기반 404 예외 처리 통일
- MockMvc 통합 테스트 작성, Docker Compose로 DB · 샘플 데이터 실행 환경 구성

<br>

### [복복이 (WelBot)](https://github.com/Chan-pj/Chatbot) — RAG 기반 복지 정책 안내 AI 챗봇
`Python` `Flask` `MySQL` `ChromaDB` `Groq API` `Docker` · 2인 팀 프로젝트 · 백엔드 · RAG 담당

공공데이터 5,011건의 복지 정책을 벡터 검색하고, LLM이 사용자 상황과 거주 지역에 맞는 정책을 카드 형태로 추천하는 챗봇

- 공공데이터 API 수집 → MySQL 적재 → 한국어 임베딩 → ChromaDB 검색 → LLM 답변까지 RAG 파이프라인 구현
- 시군구 · 시도 · 전국 그룹별 검색과 관련도 필터로 지역 맞춤 추천 및 전국 정책 누락 문제 해결
- 추론 토큰 소진으로 JSON 응답이 잘리던 문제를 원인 분석 후 해결
- 검색 결과 수와 프롬프트를 조정해 질문당 토큰 약 20% 절감
