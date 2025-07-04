---
title: "[Clairy] Trouble Shooting"
excerpt: "AI 문서 정리 웹앱 Clairy 초기 세팅과 기술 스택 정리"
name: AI-chemist97
writer: AI-chemist97
categories: [Portfolio,Clairy]
tags:
  - [Clairy, FastAPI, NextJS, 개발환경, AI, 문서정리]
# published: false
toc: true
toc_sticky: true
date: 2025-07-04 03:00:00 +0900
last_modified_at: 2025-07-04 04:00:00 +0900
---

# 🐍 가상환경 실행 중 pip와 python 오류 정리

PostgreSQL 연결용 패키지를 설치하려고 `pip install`을 했는데 갑자기 아래와 같은 에러가 발생했다.

```bash
did not find executable at 'C:\Users\사용자명\AppData\Local\Programs\Python\Python313\python.exe'
: ??? ??? ?? ? ????.
````

`pip`는 원래 경로에 잘 있었고, 컴퓨터도 똑같은데 왜 이런 일이 생기는 건지 이해가 안 됐다.
방금 전까지 잘 됐고, 컴퓨터 끄고 운동 다녀온 게 전부였다.

---

## 🔍 문제 탐색

StackOverflow를 뒤져보니 가상환경 문제일 수 있다는 의견을 발견했다.
👉 [참고한 글](https://stackoverflow.com/questions/74449331/my-program-cant-seem-to-find-the-python-exe-in-my-computer)

그래서 기존 `venv`를 삭제하고 `python -m venv venv`로 다시 만들려고 했는데,
에러는 그대로였다.

그래서 `where python`, `where pip`를 해보니:

```bash
C:\Users\사용자명\AppData\Local\Microsoft\WindowsApps\python.exe
C:\Users\사용자명\AppData\Local\Programs\Python\Python313\Scripts\pip.exe
```

`pip`는 보이는데 `python`은 뭔가 이상하다 싶어서, 실제 경로에 들어가 봤다.
그랬더니 `Python313` 폴더에 `python.exe`가 **사라져 있음**.
폴더 구조만 남아 있고, 실행 파일이 없음.

---

## ⚠️ 문제의 핵심

`pip.exe`는 내부적으로 `python.exe`를 참조해서 실행되는데,
지금 `pip`가 의존하는 경로에 `python.exe`가 없으니 작동이 안 되는 것.

게다가 `where python`으로 나오는 경로는:

```bash
C:\Users\사용자명\AppData\Local\Microsoft\WindowsApps\python.exe
```

이건 진짜 Python이 아니라 **Windows에서 제공하는 Microsoft Store용 더미 실행파일**이다.
→ Python이 설치되어 있지 않아도 `python` 명령이 통하도록 해주는 일종의 껍데기.

---

## 🤔 왜 이런 일이 발생했을까?

가능성 있는 원인들을 정리해봤다:

1. **Python 3.13의 안정성 문제**

   * 최신 버전이라 발생할 수 있는 이슈
   * 그래도 써보고 싶어서 설치했는데, 재현되면 3.12로 내려갈 예정

2. **Windows 업데이트로 인한 경로 꼬임**

   * 최근에 업데이트한 기억이 없어 가능성 낮음

3. **보안 소프트웨어의 오진 삭제**

   * `.exe` 파일이라 백신이 실수로 삭제했을 가능성이 가장 높음

4. **디스크 오류 또는 파일 시스템 손상**

   * 오래된 PC라 가능성 있음

5. **Python 설치 중단/충돌**

   * 설치 중 문제가 생겼을 가능성도 배제할 순 없음

6. **OneDrive 동기화 문제**

   * 내 경우엔 OneDrive 연동이 문제가 되진 않았음

---

## ✅ 해결 방법

StackOverflow에서 본 조언 중 하나는 **짧고 단순한 경로**에서 Python과 가상환경을 사용하는 것.
그래서 `OneDrive` 아래 말고, 더 안전한 경로에서 가상환경을 만들었더니 문제 해결됨!

---

## 📝 정리

다음에 비슷한 문제가 생기면 아래 순서로 점검하면 된다:

1. `where python`, `where pip`로 경로 확인
2. `pip`가 참조하는 `python.exe`가 실제로 존재하는지 직접 확인
3. `WindowsApps` 경로는 진짜 Python이 아닌 더미일 수 있으니 주의
4. 필요한 경우 Python 재설치 (권장: Python 3.12 안정버전)
5. 가상환경은 `OneDrive` 같은 경로보다 `D:\dev\project` 같은 간단한 경로에서 생성

---

시간 꽤 잡아먹었지만, 다음부터는 빠르게 해결할 수 있을 듯

블로그 글 다 쓰고 봤는데 너무 구려서 gpt한테 고치라고 했더니 퀄이 좋아짐 고치기 전 글도 아래에 첨부함

---
# 가상환경 실행 중 pip와 python 관련 오류
가상환경 venv를 실행하고 postgresql db를 연결하기 위해 패키지를 추가로 다운받으려고 하던 중 pip install 이 먹히지 않고 자꾸 아래 같은 류의 에러가 떴다.
```bash
did not find executable at 'C:\Users\사용자명\AppData\Local\Programs\Python\Python313\python.exe'
: ??? ??? ?? ? ????.
```
자꾸 없다는데 일단 pip는 확실히 원래 경로에 있었고 이전에 실행했던 컴퓨터에 변하지 않았기 때문에 이 컴퓨터로만 개발을 진행해서 방금전에 끄고 운동갔다 다시와서 켠걸 빼고는 다른게 없었다.

https://stackoverflow.com/questions/74449331/my-program-cant-seem-to-find-the-python-exe-in-my-computer
stackoverflow에서 가상환경 문제같으니 다시 설치해~ 라는 뉘앙스의 글을 발견했고
가상환경을 삭제했더니
`python -m venv venv`로 가상환경을 만들려고 하자마자 위에 처럼 또 찾을 수 없다고 떴다.

컴퓨터를 껐다가 켜보고 하던 중 where python을 해보니 일단 있긴하고 pip도 둘다 있는데 왜 안뜨지

혹시나 해서 저기서 찾을 수 없다는 경로로 가봤다. 원래 저기 설치되어 있었는데 python.exe가 없었다.

where pip를 해보니 경로가 2개가 나왔고

이런 경우는 python 설치가 꼬여있다는데 기존에 잘 되던게 안되는게 이해를 할 수 없었다. 심지어 같은 컴퓨터
pip.exe는 내부적으로 python.exe를 참조해서 실행되는데 지금 where python을 통해 찾아진 경로의 python은 
```bash
C:\Users\사용자명\AppData\Local\Microsoft\WindowsApps\python.exe  
```
이거 였는데 이건 진짜 python이 아니고 windows에서 제공하는 가짜 실행 파일이다. python이 설치되지 않아도 `python`명령을 인식할 수 있게 하려고 더미 파일을 둔 Microsoft store 용 python 설치 유도용 가짜이다.

인터넷을 찾아보니 보통 이런데는 여러가지 원인이 있다고 한다.
1. **python3.13이 안정화되지않아 발생**(근데 3.13쓰고 싶으니 이번에 3.13받고 문제 재 발생시 3.12로 사실 3.13이랑 12랑 설명만 봤을 때는 뭐 성능 최적화 문법 확정 이런거처럼 이렇게 작은 프로젝트에 영향을 미칠만한건 없는 거 같은데 그래도 새걸 써보고 싶었다.)
2. **windows 업데이트 후 경로 꼬임**(근데 이건 가능성이 적은게 업데이트를 한 적 없다.)
3. **보안소프트웨어의 오진 삭제**->가능성이 높다고 보는게 python.exe의 .exe가 보안프로그램의 오진으로 경리 삭제되는 경우가 있다.
4. 디스크 오류나 파일 시스템 손상->오래된 컴퓨터라 그럴수도...
5. python 설치 중단 또는 충돌 -> 가능성 적음
6. onedrive동기화이슈 ->아닐듯

--> 해당 문제 방지를 위해 좀더 간단한 경로에서 python을 쓰라는 권유를 받았다. 암튼 이렇게 했더니 문제 해결!
다음에는 이런 문제 발생시 해결하는 시간을 훨씬 줄일 수 있다.

