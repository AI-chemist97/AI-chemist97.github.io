---
title: "[gwabang]Spring 폴더 구조와 Entity"
excerpt: "git"
name: AI-chemist97
writer: AI-chemist97
categories: [Portfolio,gwabang]
tags:
  - [gwabang]

# published: true
toc: true
toc_sticky: true

date: 2025-05-12 20:43:00 +0900
last_modified_at: 2025-05-12 20:43:00 +0900
# --- 아래 부터 content
---
# Spring 개발 폴더 구조
개발을 진행하면서 entity를 내가 짜서 넣겠다고 했다.
근데 과거에 개발을 급하게 진행하면서 해서 기억이 잘 나지않아 하나씩 다시 짜면서 정리해 두려고 한다.

우선 erd 테이블 종류별로 폴더를 생성한다

![과방 erd 이미지](../assets/img/250512/gwabang-erd.png)

일단 여기 있는 총 5종류의 이름의 폴더를 생성한다
원래 보다싶이 User 테이블인데 생각해보니 저번 개발에서도 User가 이미 자바 표준 라이브러리에 User 클래스가 있어서 잘못쓰면 충돌이 날 수 있다고 들었던 것 같아 팀원에게 제안하여 Member로 바꾸기로 했다.
해당 코딩은 바꾸는 와중에 하고 있어서 이미지는 우선 User지만 Member 테이블로 생각하고 제작한다.
member, article, comment, articlelike,commentlike, category....
이렇게 대충 폴더를 만들어 주고 member 부터 시작한다
보이는 그대로 일단 만들어주고 join같은 관계성은 다 만들고 다음 글에 수정하겠다.

onetomany manytoone...
화이팅
파일 구조는 최종 깃에서 확인 가능하다.
우선 member 의 경우 entity 폴더 내부에 Member.java
를 생성해야한다.

```java

@Getter
@Builder
@AllArgsConstructor
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Entity
@Table(name = "member")
public class Member {

};
```
이런식으로 시작해야한다
위에 달린건 어노테이션 annotation이라고 하는데 클래스에 코드를 자동 생성하거나 매핑을 도와준다
@Getter는 모든 필드에 대해 getter 메서드를 자동으로 생성해주고
@Builder는 객체를 만들때 new 대신 ClassName.builder().field().build() 형태로 사용할 수 있게 한다.(어렵다)



[github - gwabang](https://github.com/AI-chemist97/gwabang)

private이라 안보일수도있음