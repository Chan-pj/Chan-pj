# 허찬 | Backend · Database · AI Service

데이터가 어떻게 수집되고, 저장되고, 조회되는지를 설계하는 개발자를 지향합니다.
스키마 설계부터 데이터 적재 파이프라인, SQL 기반 조회, 서비스 API, 그리고 LLM · RAG를 서비스에 연결하는 과정까지 데이터가 흐르는 전 과정을 직접 구현해 왔습니다.

<br>

## Certifications

| 자격증 | 발급 기관 |
|---|---|
| SQLD (SQL 개발자) | 한국데이터베이스진흥센 |
| ADsP (데이터분석 준전문가) | 한국데이터베이스진흥원 |
| 정보처리산업기사 | 한국산업인력공단 |
| 리눅스마스터 2급 | 한국정보통신진흥협회 |
| 컴퓨터활용능력 2급 | 대한상공회의소 |
| 전산회계 1급 | 한국세무사회 |

<br>

## Tech Stack

| 분류 | 기술 |
|---|---|
| Database | MySQL / MariaDB, SQL Server, SQLite, ChromaDB (Vector DB), H2 |
| Language | SQL, Java, Python, JavaScript |
| Backend | Spring Boot, Spring Data JPA, Spring JDBC, Flask, Thymeleaf |
| AI · Data | LLM · RAG (임베딩 기반 벡터 검색), PyTorch, 공공데이터 API 수집 · 적재 |
| Mobile | Capacitor (Android) |
| Test | JUnit 5, MockMvc |
| Infra / Tool | Linux, Docker, Docker Compose, GitHub Actions, Gradle, Maven, Git / GitHub, Postman |

<br>

## Database Experience

**데이터 모델링**
- 서비스 요구사항을 바탕으로 7개(EcoLink) · 10개(WelBot) · 9개(CollegeSystem) 테이블의 스키마를 설계하고 PK · FK로 엔티티 간 관계를 정의
- 마스터 테이블(복지 서비스 목록)과 1:N 상세 테이블(신청 방법 · 문의처 · 근거 법령 등)로 분리해 중복 없이 관리
- 시계열 센서 데이터 조회 패턴에 맞춰 `(can_id, log_time)` 복합 인덱스 설계
- 상품마다 다른 상세 정보를 항목명 / 항목값 행으로 저장해 테이블 변경 없이 확장되도록 설계 (ShoppingMall)

**데이터 수집 · 적재**
- 공공데이터포털 API로 중앙부처 · 지자체 복지 정책 **5,011건**을 수집해 MySQL에 적재하는 파이프라인 구현
- `ON DUPLICATE KEY UPDATE` · `INSERT IGNORE`로 재수집 시 중복 없이 갱신되는 멱등 적재 구조 설계
- 적재된 데이터를 `seed.sql`로 덤프해 API 키 없이도 동일한 데이터로 환경을 재현할 수 있도록 구성

**데이터 정합성 · 운영**
- 애플리케이션 엔티티와 실제 DB 스키마의 불일치(테이블명 · 누락 테이블)를 찾아 스키마를 재정의하고, 기동 시 스키마 검증(`ddl-auto=validate`)으로 정합성 확인
- `UNIQUE` · `CHECK` 제약과 애플리케이션 검사를 함께 써서 수강 중복과 시간표 충돌을 DB · 코드 두 단계에서 차단 (CollegeSystem)
- Docker Compose 초기화 스크립트로 스키마 생성과 샘플 데이터 적재를 자동화
- 주간 배치(APScheduler)로 벡터 인덱스를 재구축해 원천 데이터 변경을 검색에 반영
- 문자셋 `utf8mb4` 통일로 한글 데이터 저장 · 검색 문제 예방

<br>

## Projects

### [복복이 (WelBot)](https://github.com/Chan-pj/Chatbot) — RAG 기반 복지 정책 안내 AI 챗봇
`MySQL` `ChromaDB` `Python` `Flask` `Docker` · 2인 팀 프로젝트 · 백엔드 · DB · RAG 담당

공공데이터 5,011건의 복지 정책을 벡터 검색하고, LLM이 사용자 상황과 거주 지역에 맞는 정책을 카드 형태로 추천하는 챗봇

- 공공데이터 API 수집 → MySQL 적재 → 한국어 임베딩 → ChromaDB 검색 → LLM 답변까지 데이터 파이프라인 구현
- 복지 서비스 마스터 · 상세 · 회원 · 대화 기록 등 10개 테이블 스키마 설계
- 시군구 · 시도 · 전국 그룹별 검색과 관련도 필터로 지역 맞춤 추천 및 전국 정책 누락 문제 해결
- 검색 결과 수와 프롬프트를 조정해 질문당 토큰 약 20% 절감

<br>

### [EcoLink BinGo](https://github.com/Chan-pj/Eco-Link) — IoT 스마트 쓰레기통 수거 최적화 시스템
`MariaDB` `Spring Data JPA` `Spring Boot` `Docker` · 4인 팀 프로젝트 · 백엔드 · DB 설계 담당

초음파 센서로 쓰레기통 적재량을 수집하고, AI 예측과 경로 최적화로 수거가 필요한 쓰레기통만 효율적으로 수거하도록 돕는 서비스

- 쓰레기통 · 센서 로그 · 상태 이력 · 수거 경로 · 수거 이력 · 작업자 등 7개 테이블 스키마 및 ERD 설계
- 센서 시계열 데이터 저장 구조와 조회용 복합 인덱스 설계, 최근 48시간 기준 샘플 데이터 생성 스크립트 작성
- JPA 연관관계 매핑과 Controller · Service · Repository 구조의 REST API 구현
- 응답 DTO 분리로 비밀번호 노출 차단, 예외 처리 통일, MockMvc 통합 테스트 작성

<br>

### [TraderGame](https://github.com/Chan-pj/TraderGame-Mobile-) — 턴제 주식투자 시뮬레이션 게임 · [▶ 바로 플레이](https://chan-pj.github.io/TraderGame-Mobile-/)
`JavaScript` `SQLite` `Capacitor` `GitHub Actions` · 개인 프로젝트

가상 종목을 사고팔아 목표 순자산을 달성하는 로그라이크 투자 게임. 웹 브라우저와 Android 앱에서 같은 코드로 실행

- 시장 국면 · 뉴스 출처별 신뢰도 · 상장폐지 · 임대료 · 대출 등 게임 규칙과 상태 관리 로직 구현
- 전략별 2,500판 이상의 몬테카를로 시뮬레이션으로 난이도별 은퇴율을 측정해 밸런스 조정
- 게임 상태 · 플레이 기록 · 업적을 SQLite로 저장 (Android: capacitor-sqlite, 웹: sql.js)
- GitHub Actions로 GitHub Pages 자동 배포, Capacitor로 Android 앱 패키징

<br>

### [학사관리 시스템 (CollegeSystem)](https://github.com/Chan-pj/CollegeSystem) — 수강신청 · 성적 · 시간표 관리 웹 서비스
`SQL Server` `Spring Data JPA` `Spring Boot` `Thymeleaf` · 개인 프로젝트

- 학과 · 교수 · 학생 · 과목 · 수강 · 성적 · 시간표 등 9개 테이블 스키마 설계
- DB 시퀀스로 코드 체계(학번 20260001~, 교수 1001~, 과목 5001~) 자동 부여
- 강의실 · 담당교수 · 학생 수강 시간의 교시 구간 충돌 검사와 학생별 요일 × 교시 시간표 구현
- 시간표를 과목 단위로 재설계해 과목 시간이 바뀌어도 학생 데이터 수정이 필요 없도록 개선

<br>

### [ShoppingMall](https://github.com/Chan-pj/ShoppingMall) — 전화번호 인증 기반 쇼핑몰 웹 서비스
`SQL Server` `Spring JDBC` `Spring Boot` `Thymeleaf` · 개인 프로젝트

- 6자리 인증번호 · 3분 만료 · 이전 번호 자동 비활성화 방식의 회원가입 · 로그인 구현
- 상위코드 저장 규칙으로 대 · 중 · 소 3단계 카테고리를 조인 없이 조회하도록 설계
- `JdbcTemplate`으로 SQL을 직접 작성한 Controller · Service · Repository 구조
