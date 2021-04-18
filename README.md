# [Babel] ES6 문법 코드 -> ES5 문법 코드로 변환하기


## 1. index.html / index.js 파일 생성 

- `index.html`
- `index.js` : backtick(`) 사용(ES6문법)으로 IE에서는 오류 발생하도록 작성했습니다.

- 크롬 / IE에서 동작 확인 (IE에서는 오류 발생) 

여기까지 git branch : https://github.com/sosohanya/sample-babel/tree/step01-start 


## 2. npm init 으로 package.json 파일 생성 (생략가능)  

- Babel 설치 전 사전작업. 
- 이 부분은 **필요**에 따라 진행하지 않아도 됩니다. 

### 2.1 npm init 명령어 입력으로 package.json 파일 생성 

#### 방법 1. 명령프롬프트에서 패키지 명, 버전, 설명 등을 하나씩 설정  
```shell
npm init
```

#### 방법 2. 명령프롬프트에서 기본값으로 한번에 생성 (저는 아직 package.json의 세부 내용은 모르므로 그냥 기본으로 생성합니다.)
```shell
npm init -y 
```

### 2.2 위 명령어 실행 후 
- `package.json` 파일이 생성됨

### !참고! package.json 파일 생성하는 이유는?
- library version과 의존성(denpendency) 관리를 위함입니다.
    - 아래 내용을 보면 library들은 버전이 있고, 각 library들이 의존하는(호출/사용해야하는) 다른 library와 버전 범위(^)가 호환되어야 합니다. 
    ```json
    {
        "devDependencies": {
            "@babel/cli": "^7.13.14",
            "@babel/core": "^7.13.15",
            "@babel/preset-env": "^7.13.15",
            "babel-loader": "^8.2.2",
            "webpack": "^5.32.0",
            "webpack-cli": "^4.6.0"
        },
        "dependencies": {
            "@babel/polyfill": "^7.12.1"
        }
    }
    ```

- 차후 다른 환경에서 `npm install` 명령어를 통해 동일하게 프로젝트를 진행 가능합니다. 
    - 본인이 만든 library 배포하거나, 다른 환경에서 작업 또는 다른 사람과 함께 작업을 할 경우, 환경마다 또는 작업하는 사람에게 가서 각 명령어들을 직접 입력하여 (버전까지 맞춰서) 설치하고 하는 작업들 일일이 할 수 없으니, `npm install` 명령어 하나로 package.json에 있는 devDependencies/dependencies 목록에 있는 것들을 npm이 자동으로 다운로드/설치가 가능합니다.

- package.json의 `"scripts"` 항목 설정에 따라, 명령어를 단축화 할 수 있습니다. 
