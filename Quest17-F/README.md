# Quest 17-F. 번들링과 빌드 시스템

## Introduction

- 이번 퀘스트에서는 현대적 웹 클라이언트 개발에 핵심적인 번들러와 빌드 시스템의 구조와 사용 방법에 대해 알아보겠습니다.

## Topics

- Webpack
- Bundling
  - Data URL
- Transpiling
  - Source Map
- Hot Module Replacement

## Resources

- [Webpack](https://webpack.js.org/)
- [Webpack 101: An introduction to Webpack](https://medium.com/hootsuite-engineering/webpack-101-an-introduction-to-webpack-3f59d21edeba)

## Checklist

- 여러 개로 나뉘어진 자바스크립트나 이미지, 컴포넌트 파일 등을 하나로 합치는 작업을 하는 것은 성능상에서 어떤 이점이 있을까요?

  **웹팩에서 해결하고자 하는 문제점**

  1. 자바스크립트 변수 유효 범위
  2. **브라우저별 HTTP 요청 숫자의 제약**

  - TCP 스펙에 따라 브라우저에서 서버로 한번에 보낼 수 있는 **HTTP 요청의 갯수에는 제약**이 있다.

    | 브라우저        | 최대 연결 횟수 |
    | --------------- | -------------- |
    | 익스플로러      | 7 2            |
    | 익스플로러      | 8 ~ 9 6        |
    | 익스플로러      | 10, 11 8, 13   |
    | 크롬            | 6              |
    | 사파리          | 6              |
    | 파이어폭스      | 6              |
    | 오페라          | 6              |
    | 안드로이드, iOS | 6              |

  - 그러므로, HTTP요청을 줄이는 것은 어플리케이션의 성능을 높여 줄수 있으며, 사용자가 사이트를 로딩하고 조작하는 시간을 앞당겨 줄 수 있다.

  > 참고 |
  > 클라이언트에서 서버에 HTTP 요청을 보내기 위해서는 먼저 TCP/IP가 연결되어야 한다.

  <br/>

  3. 사용하지 않는 코드의 관리
  4. Dynamic Loading & Lazy Loading 미지원

  - 레이지 로딩(Lazy Loading) : 초기 페이지 로딩 속도를 높이기 위해 나중에 필요한 자원들은 나중에 요청하는 방식
  - 웹팩의 Code Splitting 기능을 이용하여 원하는 모듈을 원하는 타이밍에 로딩할 수 있다.

  <br/>

  - 이미지를 Data URL로 바꾸어 번들링하는 것은 어떤 장점과 단점이 있을까요?

    ```html
    // image file // 파일을 특정위치에 저장, DB에 위치를 기록
    <img src="./1.png" />

    // data url .. 파일을 Data URL로 변환하여 DB에 저장
    <img src="data:image/png;base64, ....." />
    ```

    - Data URL : `data:` 접두어가 붙은 URL이며, 바이너리 파일을 Base64로 인코딩하여 ASCII 문자열 형식으로 변환한 것이다.

    - 형식 : `data:[<mediatype>][;base64],<data>`

    > 특징

    - 이미지 파일을 따로 두지 않고 하나의 html 파일에서 데이터를 불러올 수 있다. 하나의 html로 표현할 수 있다.
    - 파일의 크기가 커질수록 <data>영역이 길어진다.
    - disk cache가 불가능 하며, memory cache만 가능하다.

    > 단점

    - 바이너리 파일을 한 번 인코딩 했기 때문에, 이미지의 경우 화면에 출력하는데 비교적 오래걸린다.
    - 브라우저에 따라 URL의 길이제한이 있을수 있으며, 디코딩 처리가 오래걸리기 때문에 작은 파일의 경우에만 사용하는것이 권장된다.

     <br/>

- Source Map이란 무엇인가요? Source Map을 생성하는 것은 어떤 장점이 있을까요?
- Webpack의 필수적인 설정은 어떤 식으로 이루어져 있을까요?

  - 웹팩은 .config(구성파일)을 지원한다. `webpack.config.js`가 있는 경우 webpack 명령은 기본적으로 이를 선택한다.
  - 웹팩의 빌드(파일 변환) 과정을 이해하기 위해서는 아래 4가지 주요 속성을 알아야 한다.

    1. **entry**

    ```js
    // webpack.config.js
    module.exports = {
      entry: './src/index.js',
    };
    ```

    - 웹팩에서 웹 자원을 변환하기 위해 필요한 최초 진입점, JS 파일 경로이다.

    > entry 파일에는 어떤 내용이 들어가야 할까? (단일)

    - 웹 앱의 전반적인 구조와 내용이 담겨 있어야 한다. 웹팩은 해당 파일을 가지고 웹 앱에서 사용되는 모듈들의 연관관계를 이해하고 분석하기 때문에 어플리케이션을 동작시킬 수 있는 내용들이 있어야 한다.

    - 모듈 간의 의존 관계가 생기는 구조를 디펜던시 그래프(Dependency Graph)라고 한다.

    > entry 파일 분리하기

    ```js
    entry: {
      login: './src/LoginView.js',
      main: './src/MainView.js'
    }
    ```

    - 엔트리 포인트트 1개가 될 수도 있지만 , 여러 개가 될 수도 있다.

    - SPA이 아닌, 특정 페이지로 진입했을 때 서버에서 해당 정보를 내려주는 형태의 멀티 페이지 어플리케이션에 적합하다.

    <br/>

    2. **output**

    ```js
    // webpack.config.js
    module.exports = {
      output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, './dist'), // './dist/bundle.js'
      },
    };
    ```

    - 웹팩이 빌드난 후 생성된 결과물의 파일경로를 의미한다.
    - `filename`은 필수로 지정해 줘야 하며, 일반적으로 `path`속성 (해당 파일의 경로)를 함께 정의한다.

    - `path.resolve()` 코드는 인자로 넘어온 경로들을 조합하여 유효한 파일 경로를 만들어주는 Node.js API이다.

      <br/>

    > output 파일 이름 옵션

    - 앞에서 살펴본 filename 속성에 여러 가지 옵션을 넣을 수 있다.

    1. 결과 파일 이름에 entry 속성을 포함하는 옵션

    ```js
    module.exports = {
      output: {
        filename: '[name].bundle.js',
      },
    };
    ```

    2. 결과 파일 이름에 웹팩 내부적으로 사용하는 모듈 ID를 포함하는 옵션

    ```js
    module.exports = {
      output: {
        filename: '[id].bundle.js',
      },
    };
    ```

    3. 매 빌드시 마다 고유 해시 값을 붙이는 옵션

    ```js
    module.exports = {
      output: {
        filename: '[name].[hash].bundle.js',
      },
    };
    ```

    4. 웹팩의 각 모듈 내용을 기준으로 생생된 해시 값을 붙이는 옵션

    ```js
    module.exports = {
      output: {
        filename: '[chunkhash].bundle.js',
      },
    };
    ```

    이렇게 생성된 결과 파일의 이름에는 각각 엔트리 이름, 모듈 아이디, 해시 값 등이 포함된다.

    <br/>

    3. **loader**

    ```js
    // webpack.config.js
    module.exports = {
      module: {
        rules: [],
      },
    };
    ```

    - 로더(loader)는 웹팩이 웹 어플리케이션을 해석할 때 JS 파일이 아닌 웹 자원(HTML, CSS, Images, font 등)을 변환할 수 있도록 도와주는 속성이다.

      <br/>

    > 자주 사용되는 로더 종류

    - Babel Loader
    - Css Loader
    - Sass Loader
    - File Loader
    - Vue Loader
    - TS Loader

      <br/>

    - 로더를 여러 개 사용하는 경우에는 아래와 같이 rules 배열에 로더 옵션을 추가하면 된다.

    ```js
    module.exports = {
      module: {
        rules: [
          { test: /\.css$/, use: 'css-loader' },
          { test: /\.ts$/, use: 'ts-loader' },
          // ...
        ],
      },
    };
    ```

    - `test` : 어떤 파일이나 파일 속성 식별이 변화되어야한다.
    - `use` : 변환을 수행하는 데 사용해야 하는 로더를 나타낸다.

    <br/>

  > 로더 적용 순서

  - 특정 파일에 대해 여러 개의 로더를 사용하는 경우 로더가 적용되는 순서에 주의해야 한다. 로더는 기본적으로 **오른쪽에서 왼쪽 순**으로 적용된다.

  ```js
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: ['css-loader', 'sass-loader'],
      },
    ];
  }
  ```

  - scss 파일에 대해 먼저 Sass 로더로 전처리(scss 파일을 css 파일로 변환)를 한 다음, 웹팩에서 CSS 파일을 인식할 수 있게 CSS 로더를 적용한다.

    <br/>

  4. **plugin**

  - 웹팩의 기본적인 동작에 추가적인 기능을 제공하는 속성이다.
  - 로더랑 비교하면 로더는 파일을 해석하고 변환하는 과정에 관여하는 반면, 플러그인은 해당 결과물의 형태를 바꾸는 역할을 한다.

  ```js
  // webpack.config.js
  var webpack = require('webpack');
  var HtmlWebpackPlugin = require('html-webpack-plugin');

  module.exports = {
    plugins: [new HtmlWebpackPlugin(), new webpack.ProgressPlugin()],
  };
  ```

  - 플러그인의 배열에는 생성자 함수로 생성한 객체 인스턴스만 추가될 수 있다.

    - HtmlWebpackPlugin : 웹팩으로 빌드한 결과물로 HTML 파일을 생성해주는 플러그인
    - ProgressPlugin : 웹팩의 빌드 진행율을 표시해주는 플러그인

    <br/>

  > 자주 사용하는 플러그인

  - split-chunks-plugin
  - clean-webpack-plugin
  - image-webpack-loader
  - webpack-bundle-analyzer-plugin

    <br/>

  - Webpack을 이용하여 HMR(Hot Module Replacement) 기능을 설정하려면 어떻게 해야 하나요?

참고 : [https://joshua1988.github.io/webpack-guide/motivation/why-webpack.html](https://joshua1988.github.io/webpack-guide/motivation/why-webpack.html#%ED%8C%8C%EC%9D%BC-%EB%8B%A8%EC%9C%84%EC%9D%98-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%AA%A8%EB%93%88-%EA%B4%80%EB%A6%AC)

Webpack은 ES5와 호환 되는 모든 브라우저를 지원한다. (IE8 이하는 지원하지 않습니다).

## Quest

- 직전 퀘스트의 소스만 남기고, Vue의 Boilerplating 기능을 쓰지 않고 Webpack 관련한 설정을 원점에서 다시 시작해 보세요.
  - 필요한 번들링과 Source Map, HMR 등의 기능이 모두 잘 작동해야 합니다.

## Advanced

- Webpack 이전과 이후에는 어떤 번들러가 있었을까요? 각각의 장단점은 무엇일까요?
