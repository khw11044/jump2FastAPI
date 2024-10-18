# 점프 투 FastAPI 따라하기

```bash
(myapi) C:\venvs\myapi\Scripts>
```

## 1. FastAPI 개발 환경 준비 

### 가상 환경에서 FastAPI 설치하기

```bash
(myapi) C:\venvs\myapi\Scripts> pip install fastapi
```

### pip 최신 버전으로 설치하기

```bash
(myapi) C:\venvs\myapi\Scripts> python -m pip install --upgrade pip
```

## 2. Svelte 개발 환경 준비 

### Node.js 설치하기

Svelte를 사용하려면 Node.js가 필요하다.

Node.js는 자바스크립트로 애플리케이션을 개발할 수 있게 해 주는 도구이다.

다음의 사이트에서 Node.js를 다운받아 설치하자. 설치과정은 기본 설정을 그대로 두고 <Next> 버튼을 눌러 진행하면 되므로 생략한다.

Node.js 공식 사이트 : https://nodejs.org


Node.js를 설치하면 노드 패키지 매니저(npm)도 함께 설치된다. Svelte는 npm 명령을 통해 설치할 수 있다.

> npm은 Node.js를 위한 패키지 매니저로 필요한 패키지들을 다운로드 할 때 사용하는 도구이다. 파이썬의 pip과 비슷한 역할을 한다.

먼저 명령창을 열고 다음과 같이 myapi를 입력하여 myapi 가상환경으로 진입하자.

```bash
(myapi) c:/projects/myapi> npm create vite@latest frontend -- --template svelte
```

frontend 디렉터리가 생성되었다.

그리고 다음과 같이 frontend 디렉터리로 이동한 후에 Svelte 애플리케이션을 설치하자.

```bash
(myapi) c:/projects/myapi> cd frontend
(myapi) c:/projects/myapi/frontend> npm install

```

### 2.1 Svelte for VSCode 설치하기

VSCode는 Svelte 에디팅을 위한 확장 프로그램을 지원한다. 다음 URL을 통해 Svelte for VSCode 확장 프로그램을 설치하자.

![image](https://github.com/user-attachments/assets/cba45a6f-d657-4918-a021-8bc3cd139585)


### 2.2 자바스크립트 타입 체크 설정 끄기

스벨트는 자바스크립트의 타입을 체크하는 것이 디폴트로 설정되어 있다. 
하지만 파이보 프로젝트는 타입스크립트(Typescript)를 사용하지 않기 때문에 해당 설정을 끄고 진행하도록 한다.

VSCode에서 jsconfig.json 파일을 열고 다음과 같이 수정하자.

[파일명: projects/myapi/frontend/jsconfig.json]

compilerOptions의 checkJs에 설정된 값을 true에서 false로 변경한다.

![image](https://github.com/user-attachments/assets/c76b2bd5-e483-4d96-88e2-afd19a5403c2)


### 2.3 Svelte 서버 실행하기

이제 VSCode에서 다음과 같이 터미널을 열고 npm run dev 명령을 실행해보자.

[VSCode 터미널]


```bash
(myapi) c:/projects/myapi/frontend> npm run dev
```

![image](https://github.com/user-attachments/assets/110d2c45-7c01-4d5e-8ee0-3f37498dfdd1)

그러면 Svelte 서버가 로컬 PC에서 실행된다. 브라우저를 열고 http://127.0.0.1:5173/ 주소를 입력해 보자. 그러면 다음과 같은 화면이 나타날 것이다.

프론트엔드 서버가 필요한 이유?
Svelte로 작성한 코드를 실시간으로 테스트 해 보려면 Svelte로 작성한 코드를 브라우저가 인식할 수 있는 HTML, CSS, Javascript로 실시간 변환하는 기능이 필요하다. 이러한 실시간 변환을 위해 Node.js 서버가 필요하다. 물론 Svelte로 작성한 코드를 빌드하면 순수 HTML과 CSS, Javascript가 만들어지므로 FastAPI 서버에 정적 파일 형태로 배포하면 된다. 즉, 운영단계에서는 Node.js 서버가 필요없다.


