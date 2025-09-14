---
title: "[Steplytics]환경 세팅"
excerpt: "git"
name: AI-chemist97
writer: AI-chemist97
categories: [Portfolio,Steplytics]
tags:
  - [Steplytics]

# published: 
toc: true
toc_sticky: true

date: 2025-07-29 06:43:00 +0900
last_modified_at: 2025-07-29 06:43:00 +0900
# --- 아래 부터 content
---
# 사용 기술


---
# 가상환경 만들기
```bash
python -m venv venv
```



* git ignore를 만들어줘야 후에 깃에 프로젝트를 올릴 필요가없는 가상환경은 올라가지 않는다.
   * 용량이 너무 크고 git은 소스코드만 추적하는 곳인데, 이런 큰 바이너리 파일(사람이 읽을 수 없는 0과 1로 이루어진 파일)을 올리면 속도가 느려지고 용량도 낭비된다. requirements.txt 파일로 누구든지 그냥 만들수있음. `pip install -r requirements.txt`를 입력하여 requirements.txt 내 패키지와 버젼에 맞춰 설치한다.
==> 패키지를 삭제하려면 `pip uninstall 삭제하고싶은패키지이름`
어떤 버젼을 다운 받을 수있는지 보고싶다면?




[github - Steplytics](https://github.com/AI-chemist97/Steplytics)
