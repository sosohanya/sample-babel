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

여기까지 git branch : https://github.com/sosohanya/sample-babel/tree/step02-npm-init


## 3. Babel 설치 

### 3.1 Install packages  

공식 문서 : https://babeljs.io/docs/en/usage  
```shell
npm install --save-dev @babel/core @babel/cli @babel/preset-env
```

### 3.2 위 명령어 실행후 
- `/node_modules` 폴더가 생성됨
- `package-lock.json` 파일이 생성됨
- `package.json` 파일이 수정됨

### 3.3 babel.config.json 파일 생성 및 설정

#### 3.3.1 `babel.config.json` 파일 생성
#### 3.3.2 아래 내용 입력 

```json
{
  "presets": ["@babel/preset-env"]
}
```

프리셋 관련 좋은 설명 블로그 URL : https://jeonghwan-kim.github.io/series/2019/12/22/frontend-dev-env-babel.html#4-%ED%94%84%EB%A6%AC%EC%85%8B 



#### !참고! `--save-dev`로 설치하는 이유
Babel은 브라우저가 이해할 수 있는 코드로 변환(build?)해 주는 내용이라, 개발 부분에만 있으면 됩니다. 

#### !참고! 
- @babel/core : Babel의 핵심 기능 
- @babel/cli : terminal에서 Babel을 사용할 수 있게 해주는 도구
- @babel/preset-env : 최신 Javascript 구문을 대상 환경에 맞는 구문으로 변환해주는 도구

공식 문서 중 `npm install --save @babel/polyfill` 의 경우, 7.4.0 버전부터 deprecate 되었다고 설명되어있습니다. (참고 : https://babeljs.io/docs/en/usage#polyfill ) 

#### !참고! git에 올릴 예정이라면
- `/node_modules` 폴더내의 엄청나게 많은 파일들이 git 변경사항으로 표시됨
- `/node_modules` 폴더 내용은 `npm install`로 언제든 생성가능하므로 `gitignore`에 추가 

여기까지 git branch : https://github.com/sosohanya/sample-babel/tree/step03-babel-install


## 4. Babel 실행 

### 4.1 Babel 실행 
```shell
./node_modules/.bin/babel index.js --out-dir lib
```

또는 (npm 5.2.0 버전 이상이라면 아래 명령어도 사용가능)
```shell
npx babel index.js -d lib
```

### 4.2 위 명령어 실행 후 
- `/lib` 폴더가 생성되었고, 하위에 `index.js` 파일이 만들어졌습니다. 
    - `/lib/index.js` 파일 내용이 ES5문법으로 변경 된것을 확인할수 있습니다. 

### 4.3 index.html 에서 lib/index.js 경로로 변경 

```html
<script src="lib/index.js"></script> <!-- index.js -> lib/index.js -->
```
- 크롬 / IE에서 동작 확인     


### !참고! `babel.config.json`이 없는 상태에서 위 명령어 실행했을 경우 
- `/lib/index.js` 파일이 만들어 지는것은 동일하지만, `/lib/index.js` 파일 내용이 변환전과 동일한것을 확인할 수 있다. 


### !참고! git에 올릴 예정이라면
- `/lib` 폴더 내용은 babel 명령어 실행시 언제든 생성가능하므로 `gitignore`에 추가 (여기서는 결과물도 올릴겸 그냥 git으로 올릴예정)


여기까지 git branch : https://github.com/sosohanya/sample-babel/tree/step04-npx-babel 

# 덧. `package.json` 파일의 `"scripts"` 설정으로 Babel 명령어 단축하기 

기본 내용 
```json
{
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
    }
}
```

* `npm test` : "scripts" 개체의 미리 지정된 속성 "test"에 지정된 명령을 실행 
```shell
npm test
```
명령어를 실행하면 명령 프롬프트창에 명령어와, "Error: no test specified"가 찍히는 것을 확인할 수 있다. 


"scripts"에 속성 추가해보기 
```json
{
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "sample": "echo \"npm sample 명령어를 입력하면 이 메시지가 출력됩니다.\"",
        "start": "npm run build",
        "build": "npx babel index.js -d lib"
    }
}
```

* `sample` : 사용자 지정 속성 "sample"에 지정된 명령을 실행 (사용자 지정 속성은 `npm run <user defined>` )
```shell
npm run sample
```

* `build` : 사용자 지정 속성 "build"에 지정된 명령을 실행 
```shell
npm run build
```

`npx babel index.js -d lib` 명령어를 직접 실행한것고 동일한 역할 

* `start` : "scripts" 개체의 미리 지정된 속성 "start"에 지정된 명령을 실행 
```shell
npm start 
```

"start" -> "build"를 호출하므로 `npx babel index.js -d lib` 명령어를 직접 실행한것고 동일한 역할 