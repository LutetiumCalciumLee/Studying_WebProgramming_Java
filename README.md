<details>
<summary>ENG (English Version)</summary>

# Program Evolution

### 1. Lecture Objectives
- **Purpose:** Introduces Java web technology motivation and overall roadmap.
- **Goal:** Explains why web-based programs dominate over client PC models.

### 2. Client PC Programs
- **Definition:** Installed on laptops/desktops (Word, Excel); fully local execution.
- **Example:** Java currency converter with if-else branching for hardcoded rates.
- **Limitation:** Adding GBP/EUR requires source modification and client reinstallation.

### 3. Client PC Limitations
- **Update Burden:** Compile/build → redistribute to all clients (like app updates).
- **Security Risk:** DB credentials exposed in client code/binary; easy reverse engineering.
- **Scalability Issue:** N clients × N changes = massive maintenance overhead.

### 4. Client-Server Model
- **Architecture:** Thin client (UI/input) + thick server (core logic/DB).
- **Currency Example:** Client sends KRW/currency → server computes → returns result.
- **Advantages:** Server-only updates for logic changes; better security (data server-side).

### 5. Web-Based (JSP) Model
- **Server-Centric:** Browser requests URL → server generates HTML → no client install.
- **Currency Flow:** Browser form submit → server processes → HTML response with result.
- **Zero Client Burden:** No installs/updates; all logic/UI on server.

### 6. Evolution Summary
- **Client PC → Client-Server → Web:** Centralized control, security, zero deployment pain.
- **Key Benefits:** Real-time updates, secure data handling, universal browser access.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 프로그램 발전 과정

### 1. 강의 목적
- **목표:** Java 웹 기술을 배워야 하는 이유와 전체 학습 흐름 제시.
- **배경:** 웹 기반 프로그램이 클라이언트 PC 모델을 대체한 이유 이해.

### 2. 클라이언트 PC 프로그램
- **정의:** 노트북/데스크톱에 설치되어 실행(Word, Excel). 완전 로컬 실행.
- **예시:** Java 환율 계산기(원화 입력 → 통화 선택 → if-else로 환율 계산).
- **구조:** 선택된 통화에 따라 분기하여 환율 계산 결과 출력.

### 3. 클라이언트 PC 단점
- **단점 1 (업데이트):** 파운드/유로 추가 시 소스코드 수정 → 컴파일/빌드 → 모든 클라이언트 PC에 재설치(앱 업데이트와 유사).
- **단점 2 (보안):** 클라이언트에서 실행되므로 DB 접속 정보 같은 민감 정보가 코드/바이너리에 노출되어 보안 취약.

### 4. 클라이언트-서버 모델
- **구조:** 클라이언트(사용자 PC 입력/송수신) + 서버(핵심 로직/DB).
- **환율 예시:** 클라이언트가 원화/통화 선택 값 전송 → 서버가 환율 정보 적용하여 계산 → 결과 반환.
- **장점:** 로직 변경 시 서버만 수정(클라이언트 보통 작업 불필요); 보안 우수(민감 정보 서버 보관).
- **한계:** 화면(UI) 기능 변경 시 클라이언트 프로그램 수정/배포 필요.

### 5. 웹 기반(JSP) 모델
- **특징:** 화면 처리(UI)와 데이터 처리를 모두 서버에서 수행.
- **사용 방식:** 브라우저 주소창에 URL 입력 → 서버가 해당 요청에 맞는 HTML 생성 → 브라우저로 전송.
- **환율 흐름:** 브라우저가 계산기 화면 수신 → 원화/통화 입력 후 "변환" 버튼 클릭 → 정보가 서버로 전송 → 서버가 환율 변환 로직 수행 → 결과를 브라우저로 표시.
- **클라이언트 부담 제거:** 브라우저는 핵심 기능 수행 안 함; 사용자는 설치/업그레이드 작업 거의 없음.
- **유지보수 용이:** 파운드/유로 추가 시 서버 로직과 HTML을 함께 수정하면 됨.
- **보안:** 모든 기능이 서버에서 처리되므로 보안 측면 유리.

### 6. 발전 과정 요약
- **변화:** 클라이언트 PC → 클라이언트-서버 → 웹 기반으로 진화.
- **핵심 이유:** 중앙집중식 관리, 보안 강화, 배포 부담 제거, 실시간 업데이트 가능.

</details>
