---
title: "[gwabang]Spring 폴더 구조와 Entity2"
excerpt: "git"
name: AI-chemist97
writer: AI-chemist97
categories: [Portfolio,gwabang]
tags:
  - [gwabang]

published: false
toc: true
toc_sticky: true

date: 2025-05-15 20:43:00 +0900
last_modified_at: 2025-05-15 20:43:00 +0900
# --- 아래 부터 content
---
# Spring 개발 폴더 구조2
그렇게 다른 파일들도 생성해서 틀을 만들어줬고, 이제는 거기에 joinColumn이라는 annotation을 달아서 관계를 이어줄거다.

게시글과 댓글 그리고 좋아요 같은 것들은 테이블 끼리 서로 연결(관계) 되어 있는데 자바에서는 JPA 엔티티 필드에 어노테이션(@)을 붙여서 표현
## 1:N 관계
Article 과 Comment는 1:N 관계
하나의 게시글이 여러개의 댓글을 가짐
```java
@Entity
public class Article {

    @Id
    @GeneratedValue
    private Long id;

    private String title;

    @OneToMany(mappedBy = "article")
    private List<Comment> comments = new ArrayList<>();
}

```
여기서는 객체가 Article이기 때문에 Article 기준으로는 게시글 한개에 댓글 여러개
->  그래서 OneToMany다







[github - gwabang](https://github.com/AI-chemist97/gwabang)

private이라 안보일수도있음