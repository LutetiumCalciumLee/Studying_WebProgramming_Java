<details>
<summary>ENG (English Version)</summary>

# JSP Development Environment Setup

### 1. JDK Installation
- **Download:** Oracle official site → select version matching tutorial.
- **Installation:** Install JDK; verify installation paths (JDK/JRE folders created).
- **Verification:** Check folder structure in installation directory.

### 2. Environment Variables (Windows)
- **JAVA_HOME:** Set to JDK installation folder.
- **Path:** Add `%JAVA_HOME%\bin` to system PATH.
- **Test:** Run `javac -version` command to confirm proper setup.

### 3. Tomcat Installation
- **Download:** Get Tomcat (servlet container) from official site.
- **Configuration:** Set port number, manager account credentials during installation.
- **Verification:** Confirm installation folder structure and service status.

### 4. Eclipse Installation
- **Installer:** Use Eclipse Installer → select **Enterprise Java** package (for web development).
- **Workspace:** Specify workspace location.
- **Launch:** Verify IDE opens successfully.

### 5. Java EE API Documentation
- **Setup:** Find and bookmark Oracle Java EE API reference docs.
- **Usage:** Consult during development when class/method details unclear.
- **Habit:** Regular reference-checking reduces errors.

### 6. Visual Studio Code
- **Purpose:** Document viewing and light editing.
- **Installation:** Install VS Code and verify launch.
- **Quick Check:** Confirm execution without deep configuration.

### 7. Oracle Database (XE - Free Edition)
- **Installation:** Download and install Oracle XE.
- **Connection Test:** Verify access via SQL Plus.
- **User Setup:** Create practice user account and grant roles.
- **Service Optimization:** Change startup type from automatic to manual to avoid resource drain.

### 8. SQL Developer Setup
- **Format:** Compressed archive (not traditional installer).
- **Extraction & Launch:** Extract → run executable.
- **Connection Test:** Input host/port/SID/credentials → test connection.
- **Worksheet:** Open worksheet to confirm full readiness.
- **Tip:** Use English Windows account/paths to avoid I/O errors with Korean characters.

### 9. exERD Installation
- **Method:** Eclipse plugin installation for DB modeling.
- **Purpose:** ER Diagram (ERD) tool for database design.
- **Outcome:** Complete toolkit ready for JSP development.

### 10. Full Toolchain Summary
- **Stack:** JDK → Tomcat → Eclipse (Enterprise) → Oracle XE → SQL Developer → exERD.
- **Readiness:** All components installed and tested; ready for dynamic web/JSP practice.

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# JSP 개발 환경 세팅

### 1. JDK 설치
- **다운로드:** 오라클 공식 사이트 → 실습 버전에 맞는 JDK 선택.
- **설치:** JDK 설치 후 설치 경로(JDK/JRE 폴더) 확인.
- **검증:** 설치 디렉토리 폴더 구조 정상 여부 확인.

### 2. 환경변수 설정 (Windows)
- **JAVA_HOME:** JDK 설치 폴더로 지정.
- **Path:** `%JAVA_HOME%\bin` 시스템 PATH에 추가.
- **테스트:** `javac -version` 명령으로 정상 동작 확인.

### 3. Tomcat 설치
- **다운로드:** 톰캣(서블릿 컨테이너) 공식 사이트에서 다운로드.
- **설정:** 설치 중 포트 번호, 관리자 계정(매니저) 기본 설정.
- **검증:** 설치 폴더 구조 및 서비스 상태 확인.

### 4. Eclipse 설치
- **설치 도구:** Eclipse Installer 사용 → **Enterprise Java** 패키지 선택(웹 개발용).
- **워크스페이스:** 워크스페이스 위치 지정.
- **실행:** IDE 실행 확인.

### 5. Java EE API 문서(레퍼런스)
- **준비:** 오라클 Java EE API 문서 찾아 북마크(즐겨찾기).
- **사용:** 개발 중 클래스/메서드 세부 사항 확인 시 참고.
- **습관:** 정기적 문서 참고로 오류 감소.

### 6. Visual Studio Code
- **목적:** 문서 확인, 간단한 편집용 에디터.
- **설치:** VS Code 설치 후 실행 확인.
- **빠른 확인:** 깊은 설정 없이 실행 여부만 확인.

### 7. Oracle Database (XE - 교육용 무료판)
- **설치:** 오라클 XE 다운로드 및 설치.
- **접속 테스트:** SQL Plus로 접속 확인.
- **사용자 설정:** 실습용 사용자 계정 생성 및 권한(롤) 부여.
- **서비스 최적화:** 자동 실행으로 인한 리소스 낭비 방지; 시작 유형을 수동으로 변경.

### 8. SQL Developer 설치 및 DB 연결
- **형식:** 전통적 설치 형식이 아닌 압축 해제 후 실행.
- **추출 및 실행:** 압축 해제 → 실행 파일 실행.
- **연결 테스트:** 호스트/포트/SID/계정 정보로 접속 테스트.
- **워크시트:** 워크시트 실행으로 전체 준비 확인.
- **팁:** 윈도우 계정이 한글일 때 I/O 오류 발생 가능; 영문 계정/경로 사용 권장.

### 9. exERD 설치
- **방식:** Eclipse 플러그인 형태로 설치.
- **목적:** DB 모델링(ERD) 도구 확보.
- **완성:** JSP 본격 실습을 위한 완전한 도구 갖춤.

### 10. 전체 도구 체인 요약
- **스택:** JDK → Tomcat → Eclipse(Enterprise) → Oracle XE → SQL Developer → exERD.
- **준비 완료:** 모든 구성 요소 설치/테스트 완료; 동적 웹/JSP 실습 준비.

</details>
