# JSB – Q&A 게시판 (Thymeleaf + Spring Boot + JPA)

질문/답변 CRUD, 검색, 회원가입/로그인 기능을 제공하는 SSR 기반 게시판입니다.  
Spring Data JPA로 정렬·페이징을 지원합니다.

---

## 🔧 기술 스택

- **Language/Build**: Java 21, Gradle
- **Framework**: Spring Boot
- **Web (MVC)**: Spring Web, Thymeleaf + TailwindCSS 
- **Persistence**: Spring Data JPA (Hibernate)
- **Validation**: Jakarta Bean Validation (`spring-boot-starter-validation`)
- **Auth/Security**: Spring Security 6
- **DB**: MySql
- **Dev/Tooling**: Lombok, Spring Boot DevTools
- **Testing**: Spring Boot Test, Spring Security Test, JUnit Platform

---

## 📦 도메인 개요

### BaseEntity (`@MappedSuperclass`, Auditing)
| Field          | Type            | Notes |
|----------------|-----------------|-------|
| id             | long            | — |
| createdDate    | LocalDateTime   | — |
| modifiedDate   | LocalDateTime   | — |


### Member
| Field     | Type   | Notes |
|-----------|--------|-------|
| username  | String | — |
| password  | String | — |
| nickname  | String | — |

### Question
| Field      | Type         | Notes |
|------------|--------------|-------|
| subject    | String       | — |
| content    | String       | — |
| answerList | List<Answer> | — |
| author     | Member       | — |

### Answer
| Field   | Type    | Notes |
|---------|---------|-------|
| content | String  | — |
| question| Question| — |
| author  | Member  | — |

#### 관계(요약)
- **Member 1 : N Question**
- **Member 1 : N Answer**
- **Question 1 : N Answer** (질문 삭제 시 답변도 삭제: `cascade=REMOVE`)
-  **Answer 1 : N Comment** (답변 삭제 시 댓글도 삭제: `cascade=REMOVE`)
  

---
## 🔗 주요 URL (SSR)
### Question Routes
| Method | Path                       | Method 이름     | Note |
|-------:|----------------------------|-----------------|------|
| GET    | /questions/list            | showList        | 목록/검색(kwType, kw) |
| GET    | /questions/detail/{id}     | showDetail      | 상세 |
| GET    | /questions/create          | showCreate      | 작성 폼 |
| POST   | /questions/create          | create          | 🔒 로그인 필요, 유효성 검사 |
| GET    | /questions/update/{id}     | showUpdate      | 작성자 본인만(컨트롤러에서 체크) |
| POST   | /questions/update/{id}     | update          | 🔒 로그인 필요, 본인만 수정 |
| POST   | /questions/delete/{id}     | delete          | 🔒 로그인 필요, 본인만 삭제 |

### Answer Routes
| Method | Path                      | Method 이름 | Note |
|-------:|---------------------------|-------------|------|
| POST   | /answers/create           | create      | 🔒 로그인 필요, 등록 후 질문 상세로 리다이렉트 |
| POST   | /answers/update/{id}      | update      | 🔒 로그인 필요, 본인만 수정 |
| POST   | /answers/delete/{id}      | delete      | 🔒 로그인 필요, 본인만 삭제 |

### Member (Auth) Routes
| Method | Path    | Method 이름  | Note |
|-------:|---------|--------------|------|
| GET    | /login  | showLogin    | 로그인 폼, 실패 메시지 표시(WebAttributes) |
| POST   | /login   | —           | **Spring Security 처리**(loginProcessingUrl) |
| GET    | /signup | signup       | 회원가입 폼 |
| POST   | /signup | signup       | 회원 생성, 중복/비번확인 검증, `redirect:/login` |
| POST   | /logout  | —           | **Spring Security 처리**(로그아웃) |

---
## 👥 역할 및 담당
> 아래 표는 **주 담당(Primary Owner)** 기준입니다.  
> 실제로는 **팀 역량 향상을 위해 모든 팀원이 기본 기능 전체를 1회 이상 end-to-end로 구현**했으며,  
> **지금 표에 있는 모든 기능을 전원이 구현 가능**합니다.

| 역할 | 담당 기능 / 기여 |
|---|---|
| **유승인 (팀장)** | 추가 기능 구현, 총괄 |
| **심수민** | 답변 |
| **이해민** | 댓글 |
| **김지윤** | 질문, 카테고리 |
| **김정호** | 소셜로그인, 비밀번호 변경 |

### 팀 운영 메모
- **주 담당**: 해당 기능의 최종 품질 책임(최종 PR, 버그 핫픽스, 문서화)
- **공통 구현**: 스프린트 초반, 각자 모든 기본 기능을 독립 구현 → 코드 리뷰/리팩토링으로 컨벤션 통합
- **협업 흐름**: GitHub Flow(브랜치 → PR → 리뷰 → 머지), 공통 코드스타일/URL 규칙 준수

### 🚀 추가 기능
- 🖼️ 프로필 화면 구현하기 (회원 정보 확인 및 수정)



