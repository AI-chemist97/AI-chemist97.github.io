---
title: "컨벤션"
excerpt: "코딩 컨벤션"
name: AI-chemist97
writer: AI-chemist97
categories: [base]
tags:
  - [Git Blog, jekyll theme, Github, Git, markdown, ssafy, AI-chemist97]

toc: true
toc_sticky: true

date: 2024-08-08 19:23:00 +0900
last_modified_at: 2024-08-08 19:23:00 +0900
# --- 아래 부터 content
---

- 들어가며
  이번 프로젝트 때 git 커밋 과정에서 협업을 위해서는 컨벤션을 지키는 게 서로에게 좋다.
  라는 이야기를 들었다. 컨벤션이 뭐냐고 옆사람에게 조용히 물어보자 자기도 모르지만 못 물어보고 있다고 해서 끝나고 같이 물어봤다.

# 백엔드 메소드 컨벤션

- controller, service method name : create, get, update, delete
  - Controller : 웹 MVC의 컨트롤러 역할
  - Service : 핵심 비즈니스 로직 구현
  - Repository : 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리
  - Domain : 비즈니스 도메인 객체, (ex) 회원, 주문, 쿠폰 등등 주로 데이터베이스에 저장하고 관리됨
    [I-one](https://it-recording.tistory.com/31)
- 메소드 순서는 crud (create (생성), Read (읽기), Update (갱신), Delete(삭제))
- read의 경우 일반적인 조회 메소드를 우선

- IntelliJ 기준 naver 컨벤션을 이용한다.
  [개발바닥곰발바닥](https://bestinu.tistory.com/64)

# 변수명 컨벤션

Member member
List<Member> members
2개 이상의 객체가 필요하다면 논의 후 결정

# 처음 시작할 때 할 것

- (Corca-실무에서 사용되는 git flow 사용법)[https://medium.com/corca/%EC%8B%A4%EB%AC%B4%EC%97%90%EC%84%9C-%EC%82%AC%EC%9A%A9%EB%90%98%EB%8A%94-git-flow-%EC%82%AC%EC%9A%A9%EB%B2%95-aka-app%EA%B0%9C%EB%B0%9C%EC%97%90-%EC%93%B0%EC%9D%B4%EB%8A%94-git-flow-%EB%B8%8C%EB%9E%9C%EC%B9%AD-%EC%A0%84%EB%9E%B5-9e860d7ce771]

  1. git clone 주소
  2. git flow init
  3. 설정 전부 엔터 눌러서 넘어가기
     ## 기능 파서 시작하기 (feature)
     - develop branch에서 시작
  4. git pull (가장 최신 develop branch 받기) (ex) git pull origin develop
  5. git flow feature start {지라 티켓 번호 숫자}-{티켓 제목}
     -> (ex) git flow feature start 28-스키마-생성
     -> feature/{티켓번호}-{티켓제목} 이런 형식으로 브랜치 자동 생성됨
  6. 작업 진행
  7. Commit

  ```
  Feat: {제목}
  {본문}
  티켓: {티켓번호}

  (ex)
  Feat: 스키마 생성
  작성된 ERD를 베이스로 PostgreSQL에 스키마를 생성했습니다.
  티켓: S11P12A603-28
  ```

  8. 완료 후 feature 브랜치에서 push 진행 git push origin feature/{티켓번호}-{티켓제목}
  9. MergeRequest 작성
     a. 필히 담당자 지정 후 코드리뷰 받을 것
  10. 코드리뷰 받은 후 머지 진행
  11. 기존 feature 브랜치 삭제할 것
  12. 4~11 반복

  ## 릴리즈

  ## 커밋 컨벤션

  [컨벤션 커밋츠 1.0.0](https://www.conventionalcommits.org/ko/v1.0.0/)
  커밋메시지 생성 CLI 툴 (Command line Interface 명령 줄 인터페이스 또는 명령어 인터페이스는 텍스트 터미널을 통해 사용자와 컴퓨터가 상호 작용하는 방식을 의미)

  - OpenCommit : ai 를 활용하여 의미있는 커밋 메시지를 자동 생성하는 도구, 개발자들의 커밋 메시지 작성시간을 줄이고 일관성있는 커밋메시지를 유지하게 함

  - Ai-Commit : 메시지를 입력안해도 문구를 자동으로 생성해서 제안

  ## 브랜치 컨벤션

  Git-flow
  [우아한기술블로그-나동호](https://techblog.woowahan.com/2553/)
  git flow 를 git bash에서 지원하고 있음.
  [해어린-쉽게이해하는GitFlow](https://shinsunyoung.tistory.com/127)

  ### merge request convention

  이슈 : jira 이슈
  개요 : 간단한 설명
  변경사항 : 실제 변경된 내용 정리
  테스트 방법 : test 메소드, api 요청과 응답 등 세부사항 정리

  예시

  ```
  ## 이슈
  ```

Jira 이슈 : [S11P12A603-31](https://ssafy.atlassian.net/browse/S11P12A603-136)

## 개요

이 Merge Request는 사용자 로그인 기능을 추가합니다.

## 변경 사항

- 로그인 API 엔드포인트 추가 (`/api/login`)
- 사용자 인증을 위한 JWT 토큰 발급 기능 구현
- 로그인 관련 유닛 테스트 추가
- 로그인 화면 UI 업데이트

## 테스트 방법

1. 백엔드 서버를 실행합니다.
2. 다음 명령어로 로그인 API를 테스트합니다:
   ```bash
   curl -X POST https://your-backend-url/api/login -d '{"username": "testuser", "password": "testpass"}'
   ```

## 브랜치 관련 명령어

1. 새 브랜치 만들고 이동

```
bash코드 복사
git checkout -b 새브랜치이름
```

2. 변경 사항 커밋

```
bash코드 복사
git add .
git commit -m '커밋 메시지'
```

3. 새 브랜치를 원격 저장소에 푸시

```
bash코드 복사
git push origin 새브랜치이름
```

4. Pull Request(Merge Request) 생성 및 머지
   GitHub, GitLab 등의 웹 인터페이스에서 새 브랜치에 대한 Pull Request(Merge Request)를 생성하고, [develop] 브랜치에 병합합니다.

5. 로컬 브랜치로 돌아가기 및 업데이트

```
bash코드 복사
git checkout develop
git pull origin develop
```

6. 로컬 브랜치 삭제

```
bash코드 복사
git branch -d 새브랜치이름
```

7. 원격 브랜치 삭제

```
bash코드 복사
git push origin --delete 새브랜치이름
```

예제

1. 새 브랜치 만들고 이동

```
bash코드 복사
git checkout -b feature/new-feature
```

2. 변경 사항 커밋

```
bash코드 복사
git add .
git commit -m "Feat: Add new feature"
```

3. 새 브랜치를 원격 저장소에 푸시

```
bash코드 복사
git push origin feature/new-feature
```

4. Pull Request(Merge Request) 생성 및 머지
   GitHub, GitLab 등의 웹 인터페이스에서 [feature/new-feature] 브랜치에 대한 Pull Request(Merge Request)를 생성하고, [develop] 브랜치에 병합합니다.

5. 로컬 브랜치로 돌아가기 및 업데이트

```
bash코드 복사
git checkout develop
git pull origin develop
```

6. 로컬 브랜치 삭제

```
bash코드 복사
git branch -d feature/new-feature
```

7. 원격 브랜치 삭제

```
git push origin --delete feature/new-feature
```
