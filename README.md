<details>
<summary>ENG (English Version)</summary>

# Model2 Architecture for Efficient Development

### 1. Model1 vs Model2 Evolution
Model1 uses JSP for both logic and presentation (1,000+ lines, maintenance nightmare). Model2 separates Controller (Servlet) + Model (DAO/VO) + View (JSP), reducing JSP to 170 lines. Benefits: clear separation, reusability, team collaboration.

### 2. MVC Pattern Foundation
**Model:** DAO (data access), VO (data objects). **View:** JSP with EL/JSTL. **Controller:** Single servlet (MemberController) handles all requests via `request.getPathInfo()` (e.g., `/member/listMembers.do`).

### 3. Front Controller Pattern
Single entry point servlet (`@WebServlet("/member/*")`) routes via path info: `listMembers.do`, `addMember.do`, `modMember.do`, `delMember.do`. Each action sets `nextPage` and forwards to appropriate JSP.

### 4. CRUD Operations Implementation
**List:** `memberDAO.listMembers()` → `request.setAttribute("membersList", list)` → `listMembers.jsp`. **Add:** Form → `addMember.do` → `memberDAO.addMember(vo)` → list. **Modify:** `modMemberForm.do?id=cha2` → find → form → `modMember.do` → update. **Delete:** `delMember.do?id=ki` → delete.

### 5. Board System Architecture
**tboard Table:** Hierarchical (articleNO, parentNO for replies). **Controller:** BoardController → BoardService → BoardDAO. **Hierarchical Query:** `CONNECT BY PRIOR articleNO=parentNO` with LEVEL for indentation.

### 6. File Upload Handling
Uses Apache Commons FileUpload. Files saved to `/C:/image/` (temp → permanent by articleNO). `ServletFileUpload.parseRequest()` extracts form fields + files. JavaScript preview: `FileReader.readAsDataURL()`.

### 7. Article View & Download
`viewArticle.do?articleNO=7` → `selectArticle(7)` → `viewArticle.jsp`. Download: `download.do?articleNO=7&imageFileName=duke.png` → streams file with `Content-Disposition: attachment`.

### 8. Modify & Image Replacement
`viewArticle.jsp` → enable form → `modArticle.do` → update + move/replace image (`FileUtils.moveFileToDirectory()` + `oldFile.delete()`). Dynamic SQL: conditional `imageFileName?` clause.

### 9. Hierarchical Delete (Replies)
`removeArticle.do?articleNO=8` → `SELECT articleNO START WITH articleNO=8 CONNECT BY PRIOR articleNO=parentNO` → delete subtree + directory (`FileUtils.deleteDirectory()`).

### 10. Pagination Implementation
**Parameters:** `section=1&pageNum=2` (100 articles/page). **Query:** `ROWNUM between (section-1)*100+1 and section*100`. **Navigation:** Previous/Next + page links. Total count: `select count(articleNO)`.

### 11. Reply System
`replyForm.do?articleNO=2` → session.setAttribute("parentNO", 2) → `replyForm.jsp` → `addReply.do` → insert with parentNO. Indentation via LEVEL in hierarchical query.

### 12. Service Layer Benefits
Decouples Controller from DAO. Single responsibility: `BoardService.listArticles(pagingMap)` returns `Map` with `articlesList`, `totArticles`, `section`, `pageNum`. Enables unit testing.

### 13. Key Design Patterns Used
**Front Controller:** Single servlet entry. **Command Pattern:** `getPathInfo()` dispatches commands. **MVC:** Clear Model-View-Controller separation. **Data Access Object (DAO):** Centralized DB operations.

### 14. Database Optimization
**Hierarchical Query:** `START WITH parentNO=0 CONNECT BY PRIOR articleNO=parentNO ORDER SIBLINGS BY articleNO DESC`. **Pagination:** ROWNUM filtering. **PreparedStatement:** Prevents SQL injection.

### 15. Production Features
- Session-based reply tracking
- Image preview/upload/replacement
- AJAX-like JavaScript forms
- Dynamic SQL for conditional updates
- Directory cleanup on delete
- Comprehensive error handling with try-catch

</details>

<details>
<summary>KOR (한국어 버전)</summary>

# Model2 방식으로 효율적으로 개발하기

### 1. Model1 vs Model2 진화
Model1은 JSP에서 로직+화면 혼재(1,000줄 이상, 유지보수 악몽). Model2는 Controller(Servlet) + Model(DAO/VO) + View(JSP) 분리, JSP 170줄로 축소. 장점: 명확한 분리, 재사용성, 팀 협업.

### 2. MVC 패턴 기초
**Model:** DAO(데이터 접근), VO(데이터 객체). **View:** EL/JSTL JSP. **Controller:** 단일 서블릿(MemberController)이 `request.getPathInfo()`로 모든 요청 처리(`/member/listMembers.do`).

### 3. Front Controller 패턴
단일 진입점 서블릿(`@WebServlet("/member/*")`)이 경로 정보로 라우팅: `listMembers.do`, `addMember.do`, `modMember.do`, `delMember.do`. 각 액션마다 `nextPage` 설정 후 JSP 포워딩.

### 4. CRUD 연산 구현
**목록:** `memberDAO.listMembers()` → `request.setAttribute("membersList", list)` → `listMembers.jsp`. **추가:** 폼 → `addMember.do` → `memberDAO.addMember(vo)` → 목록. **수정:** `modMemberForm.do?id=cha2` → 조회 → 폼 → `modMember.do` → 업데이트. **삭제:** `delMember.do?id=ki` → 삭제.

### 5. 게시판 시스템 구조
**tboard 테이블:** 계층형(articleNO, parentNO로 답변). **Controller:** BoardController → BoardService → BoardDAO. **계층 쿼리:** `CONNECT BY PRIOR articleNO=parentNO` + LEVEL로 들여쓰기.

### 6. 파일 업로드 처리
Apache Commons FileUpload 사용. `/C:/image/`에 저장(temp → articleNO별 영구). `ServletFileUpload.parseRequest()`로 폼 필드+파일 추출. JavaScript 미리보기: `FileReader.readAsDataURL()`.

### 7. 게시물 조회 & 다운로드
`viewArticle.do?articleNO=7` → `selectArticle(7)` → `viewArticle.jsp`. 다운로드: `download.do?articleNO=7&imageFileName=duke.png` → `Content-Disposition: attachment`로 스트림.

### 8. 수정 & 이미지 교체
`viewArticle.jsp` → 폼 활성화 → `modArticle.do` → 업데이트 + 이미지 이동/교체(`FileUtils.moveFileToDirectory()` + `oldFile.delete()`). 동적 SQL: 조건부 `imageFileName?` 절.

### 9. 계층 삭제(답변 포함)
`removeArticle.do?articleNO=8` → `SELECT articleNO START WITH articleNO=8 CONNECT BY PRIOR articleNO=parentNO` → 서브트리 삭제 + 디렉토리(`FileUtils.deleteDirectory()`).

### 10. 페이지네이션 구현
**파라미터:** `section=1&pageNum=2`(100개/페이지). **쿼리:** `ROWNUM between (section-1)*100+1 and section*100`. **이동:** 이전/다음 + 페이지 링크. 전체 개수: `select count(articleNO)`.

### 11. 답변 시스템
`replyForm.do?articleNO=2` → `session.setAttribute("parentNO", 2)` → `replyForm.jsp` → `addReply.do` → parentNO로 삽입. 계층 쿼리의 LEVEL로 들여쓰기.

### 12. Service 레이어 장점
Controller와 DAO 분리. 단일 책임: `BoardService.listArticles(pagingMap)`이 `articlesList`, `totArticles`, `section`, `pageNum` 포함 `Map` 반환. 단위 테스트 가능.

### 13. 사용된 핵심 디자인 패턴
**Front Controller:** 단일 서블릿 진입. **Command 패턴:** `getPathInfo()`로 명령 분배. **MVC:** 명확한 Model-View-Controller 분리. **DAO:** DB 연산 중앙화.

### 14. 데이터베이스 최적화
**계층 쿼리:** `START WITH parentNO=0 CONNECT BY PRIOR articleNO=parentNO ORDER SIBLINGS BY articleNO DESC`. **페이지네이션:** ROWNUM 필터링. **PreparedStatement:** SQL 인젝션 방지.

### 15. 프로덕션 기능
- 세션 기반 답변 추적
- 이미지 미리보기/업로드/교체
- AJAX-like JavaScript 폼
- 조건부 업데이트를 위한 동적 SQL
- 삭제 시 디렉토리 정리
- try-catch로 포괄적 에러 처리

</details>
