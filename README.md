<details>
<summary>ENG (English Version)</summary>

# Web Application Structure & Deployment

### 1. Web Application Concept
- **Definition:** Combines static web (HTML/CSS/JS for UI/events) + JSP/Servlet (Java classes for logic).
- **Services:** E-commerce, portals, job sites, etc. via dynamic content generation.
- **Architecture:** Server-side Java processing + client-side rendering.

### 2. Standard Directory Structure
- **Root Folder:** Web application name/context.
- **WEB-INF/:** External access blocked (configuration/classes/libraries).
- **WEB-INF/classes/:** Compiled Java classes.
- **WEB-INF/lib/:** JAR libraries/dependencies.
- **WEB-INF/web.xml:** Deployment descriptor (routing, filters, listener configuration).

### 3. Project Organization
- **Purpose-Based Folders:** html/, jsp/, css/, images/, js/, src/, bin/, config/.
- **Resource Classification:** Static (HTML/CSS/images) vs dynamic (Java source/compiled classes/libraries/config).
- **Separation of Concerns:** Clear distinction enables maintainability.

### 4. Web Application Registration (Deployment)
- **Method 1 (Auto-Deploy):** Place web app folder under `<TOMCAT>/webapps/` → Tomcat auto-detects.
- **Method 2 (Context Configuration):** Edit `server.xml`, add `<Context>` element manually.
  - **path:** Context path (web app identifier in URLs).
  - **docBase:** Actual web app location (typically parent of WEB-INF).
  - **reloadable:** Source change reflection mode (true = auto-reload).

### 5. Browser Request Flow
- **URL Format:** `http://localhost:port/context-path/resource`
- **Context Path:** Tomcat's recognized "web app name or alias."
- **404 Error:** Unregistered context path returns HTTP 404.

### 6. Eclipse Integration
- **Dynamic Web Project:** Auto-creates WEB-INF, lib, web.xml structure.
- **Servers Tab:** Connect Tomcat → context auto-registers in server.xml.
- **Convenience:** Run/rerun operations automated; manual server.xml editing avoided.
- **Workflow:** Understand manual steps first, then leverage IDE automation.

### 7. Production Deployment
- **WAR Export:** Package web app as WAR (Web Archive) file.
- **Tomcat Deployment:** Tomcat extracts WAR → unpacks to web app folder → executes.
- **Advantage:** Single file deployment; Tomcat handles extraction automatically.

### 8. Character Encoding Setup
- **Problem:** Korean character corruption/encoding issues during practice.
- **Solution:** Change Eclipse default encoding to UTF-8.
- **Files:** HTML/JSP file encoding settings → UTF-8 (prevents garbled text).

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# 웹 애플리케이션 구조 및 배포

### 1. 웹 애플리케이션 개념
- **정의:** 정적 웹(HTML/CSS/JS로 화면/이벤트) + JSP/서블릿(자바 클래스로 로직) 결합.
- **서비스 예시:** 쇼핑몰, 포털, 구인구직 등 동적 콘텐츠 제공.
- **구조:** 서버 측 자바 처리 + 클라이언트 측 렌더링.

### 2. 표준 디렉터리 구조
- **루트 폴더:** 웹 애플리케이션 이름/컨텍스트.
- **WEB-INF/:** 외부 직접 접근 불가(설정/클래스/라이브러리 보관).
- **WEB-INF/classes/:** 컴파일된 자바 클래스.
- **WEB-INF/lib/:** JAR 라이브러리/의존성.
- **WEB-INF/web.xml:** 배치 지시자(라우팅, 필터, 리스너 설정).

### 3. 프로젝트 조직
- **목적별 폴더:** html/, jsp/, css/, images/, js/, src/, bin/, config/.
- **리소스 분류:** 정적(HTML/CSS/이미지) vs 동적(자바 소스/컴파일 클래스/라이브러리/설정).
- **관심사 분리:** 명확한 구분으로 유지보수성 향상.

### 4. 웹 애플리케이션 등록(배포)
- **방식 1 (자동 배포):** `<TOMCAT>/webapps/` 아래에 웹앱 폴더 배치 → 톰캣 자동 감지.
- **방식 2 (컨텍스트 설정):** `server.xml` 편집, `<Context>` 요소 직접 추가.
  - **path:** 컨텍스트 경로(URL에서 웹앱 식별자).
  - **docBase:** 실제 웹앱 위치(보통 WEB-INF 상위 폴더).
  - **reloadable:** 소스 변경 반영 방식(true = 자동 리로드).

### 5. 브라우저 요청 흐름
- **URL 형식:** `http://localhost:포트/컨텍스트경로/리소스`
- **컨텍스트 경로:** 톰캣이 인식하는 "웹앱 이름 또는 별칭."
- **404 오류:** 등록되지 않은 컨텍스트 경로 → HTTP 404 반환.

### 6. Eclipse 통합
- **Dynamic Web Project:** WEB-INF, lib, web.xml 자동 생성.
- **Servers 탭:** 톰캣 연결 → server.xml에 컨텍스트 자동 등록.
- **편의성:** 실행/재실행 자동화; 수작업 server.xml 편집 불필요.
- **학습 흐름:** 수작업 이해 후 IDE 자동화로 진행.

### 7. 운영 서버 배포
- **WAR 내보내기:** 웹앱을 WAR(Web Archive) 파일로 패킹.
- **톰캣 배포:** 톰캣이 WAR 압축 해제 → 웹앱 폴더로 풀어 실행.
- **장점:** 단일 파일 배포; 톰캣이 자동 압축 해제.

### 8. 문자 인코딩 설정
- **문제:** 한글 깨짐/인코딩 오류 (실습 중 발생).
- **해결:** Eclipse 기본 인코딩을 UTF-8로 변경.
- **설정 대상:** HTML/JSP 파일 인코딩 → UTF-8 (글자 깨짐 방지).

</details>
