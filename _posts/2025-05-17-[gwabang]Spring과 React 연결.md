---
title: "[gwabang]Spring과 React 연결"
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
# Spring과 React 연결
사실 연결하려고 고민을 많이 했는데 그냥 
둘이 실행하면된다
java spring은 그냥 실행하면 되고

react 프로젝트를 시작해보겠다
* 프로젝트 설명
    * 전체프로젝트이름: `gwabang`
    * 프론트엔드 디렉토리 이름: `frontend`
    * spring boot, react, tailwindcss 쓸 것


```java
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
            .allowedOrigins("http://localhost:3000") // 프론트 포트
            .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
            .allowCredentials(true);
    }
}
```
주소만 잘 입력해주면 되는거였다.
React는 기본적으로 localhost:3000, Spring은 localhost:8080이니까 crossorigin으로 허용해 줘야한다.

그리고 이제 프론트에서 연결해주면 됨
```jsx
await axios.post(
  '요청보낼 주소',    // URL
  {
    key1: value1,     // Body - 보낼 데이터
    key2: value2,
  },
  {
    headers: {
      Authorization: 'Bearer access_token',  // (선택) 인증이나 기타 헤더
    }
  }
)


```

## React 실행


[github - gwabang](https://github.com/AI-chemist97/gwabang)

private이라 안보일수도있음