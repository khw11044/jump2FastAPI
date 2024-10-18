# 점프 투 FastAPI 따라하기

Hello API 만들기

가장 먼저 할 일은 FastAPI에 Hello API를 만드는 것이다. 파이참 에디터를 열고 다음과 같은 main.py 파일을 작성하자.

[파일명: projects/myapi/main.py]

```python

from fastapi import FastAPI

app = FastAPI()


@app.get("/hello")
def hello():
    return {"message": "안녕하세요 파이보"}
```

이제 작성한 프로그램을 실행해 보자. FastAPI로 작성한 프로그램을 실행하기 위해서는 FastAPI 프로그램을 구동할 서버가 필요하다. 먼저 파이참에서 터미널을 실행한 후 다음과 같이 uvicorn을 설치하자.

유비콘(Uvicorn)은 비동기 호출을 지원하는 파이썬용 웹 서버이다.

[터미널에서 실행]

```bash
(myapi) c:/projects/myapi> pip install "uvicorn[standard]"
```

그리고 다음과 같이 FastAPI 서버를 실행하자.

[터미널에서 실행]


```bash
(myapi) c:/projects/myapi> uvicorn main:app --reload
```

아래와 같이 2개의 터미널에서 실행되어야 한다.

![image](https://github.com/user-attachments/assets/4e403979-c5c3-4783-b856-f3bdd11356da)

그러면 현재 PC에 8000번 포트로 FastAPI서버가 구동된다.

> main:app에서 main은 main.py 파일을 의미하고 app은 main.py의 app 객체를 의미한다. --reload 옵션은 프로그램이 변경되면 서버 재시작 없이 그 내용을 반영하라는 의미이다.

FastAPI가 실행되면 FastAPI로 작성한 API는 /docs URL을 호출하여 테스트해 볼 수 있다. 브라우저에서 다음 URL을 실행해 보자.

> http://127.0.0.1:8000/docs

그러면 다음과 같은 화면이 나타난다. (이 화면이 바로 FastAPI가 자랑하는 테스트 가능한 API 문서이다. 이 화면은 React로 만들어졌다.)

![image](https://github.com/user-attachments/assets/52ba4858-a2de-4399-a572-6c0c63ea2189)