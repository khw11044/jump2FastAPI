# 점프 투 FastAPI 따라하기

### Svelte 웹 페이지 만들기

이제 백엔드의 Hello API를 완성했으니 프론트엔드 영역에서 웹 페이지를 만들어 보자. 

**VSCode를 열고 projects/myapi/frontend/src/App.svelte 파일을 열어 디폴트로 작성된 내용을 모두 지우고 다음과 같이 작성하자.**

[파일명: frontend/src/App.svelte]

```html
<h1>Hello</h1>
```

![image](https://github.com/user-attachments/assets/73f9fc5c-d587-4085-b92a-9b449d3c3755)

그러면 브라우저에서 http://localhost:5173/ 호출시 다음과 같이 Hello 라는 문구가 화면 정중앙에 표시될 것이다.


화면 좌상단이 아닌 정중앙에 표시된 이유는 app.css 파일에 작성된 스타일 때문이다. 앞으로 이 곳에 정의된 스타일을 사용할 이유가 없으므로 app.css 파일의 내용도 모두 제거하자.

[파일명: frontend/src/app.css]

```html
/* 내용을 모두 지운다. */
```

그러면 이제 Hello가 정중앙이 아닌 좌상단에 표시되는 것을 확인할 수 있을 것이다.

### FastAPI 서버와 통신하기

이제 Svelte에 작성한 App.svelte 파일에서 Hello API를 호출하여 돌려받은 값을 화면에 출력해 보자. Hello API는 호출시 다음과 같은 json을 리턴한다.


```
{
  "message": "안녕하세요 파이보"
}
```

다음과 같이 App.svelte 파일을 수정하자.

[파일명: /frontend/src/App.svelte]

```html
<script>
  let message;

  fetch("http://127.0.0.1:8000/hello").then((response) => {
    response.json().then((json) => {
      message = json.message;
    });
  });
</script>

<h1>{message}</h1>

```

http://localhost:5173/ 확인 

![image](https://github.com/user-attachments/assets/38883ab8-f132-4d72-8e8f-e721012d4edf)

와우 

화면에는 undefined라고만 표시된다. 
오류가 발생했기 때문


프론트엔드에서 FastAPI 백엔드 서버로 호출이 불가능한 상황이다. 이 오류는 FastAPI에 CORS 예외 URL을 등록하여 해결할 수 있다.


### FastAPI의 main.py 파일을 수정하자.

[파일명: main.py]

```python
from fastapi import FastAPI
from starlette.middleware.cors import CORSMiddleware

app = FastAPI()

origins = [
    "http://localhost:5173"
]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)


@app.get("/hello")
def hello():
    return {"message": "안녕하세요 파이보"}

```

오 이제 제대로 나온다.

![image](https://github.com/user-attachments/assets/6807de1b-c161-444c-97e7-714914fdfd7e)
