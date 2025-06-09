---
title: "[gwabang]Spring 폴더 구조와 Entity2"
excerpt: "git"
name: AI-chemist97
writer: AI-chemist97
categories: [Portfolio,gwabang]
tags:
  - [gwabang]

# published: false
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

Comment와 Article은 반대로 N:1 의 관계
여러 댓글이 하나의 게시글을 참조
```java
@Entity
public class Comment {

    @Id
    @GeneratedValue
    private Long id;

    private String content;

    @ManyToOne
    @JoinColumn(name = "article_id")
    private Article article;
}
```
댓글 입장에서 댓글 여러개지만 게시글은 한개
관계 설정 어노테이션 달아줄때 순서가 본인to관계있는객체
요 순서로 가는것같다

@JoinColumn(name="컬럼명")으로 외래키(FK)지정
외래키는 다른 테이블의 기본키를 참조하는 키 데이터 무결성을 보장하는데 중요한 역할
다른 테이블의 행을 가리키는 값
(ex)member_id는 Member테이블의 id를 참조하는 외래키
이렇게 해주면 자동으로 멤버의 기본키(PK)인 id를 참조해서 알아서 외래키로 설정되어 member_id가 되는 것!




[github - gwabang](https://github.com/AI-chemist97/gwabang)

private이라 안보일수도있음