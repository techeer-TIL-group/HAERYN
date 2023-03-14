## 1. Next.js 를 실행해보자


![이미지 2023  3  14  오후 11 59](https://user-images.githubusercontent.com/81466548/225054245-88d1c1b3-ffca-4ff2-ba8f-2840176506fc.jpg)

  next.js를 첫 실행하면 다음과 같은 모습이 됩니다.
하나씩 뜯어봅시다.

1) pages/ 디렉터리는 기존의 react-router와 같이  사용하기 때문에 더 쉽습니다.
페이지 디렉터리안의 파일은 퍼블릭 페이지가 되기 때문에 특정 주소 접근이 용이합니다.
예를 들어 index.js 파일을 about.js로 설정하면 [http://localhost:3000/about](http://localhost:3000/about) 주소로 접근할 수 있습니다.

2) public/ 디렉터리 안에는 웹 사이트의 모든 퍼블릭 페이지와 정적 콘텐츠가 있습니다.
이미지 파일, 컴파일된 CSS 스타일 시트, 컴파일된 js 파일, 폰트 등이 있습니다.

3) style/ 디렉터리 안에는 스타일 시트를 둘 수도 있디만 꼭 필요한건 아닙니다.
필수 요소는 public과 page 이 둘입니다.
components/ 디렉토리 아래, utilities/ 디렉터리에 사용하는 등 개발자의 입맛대로 커스텀해도 됩니다.


## 2. 타입스크립트 지원


  기본적으로 타입스크립트로 작성된 프레임워크라, ‘타입 정의’가 필수적입니다.
tsconfig.json 파일만 있으면 준비는 끝납니다.
`npm run dev` 명령어를 실행하면, 주 언어에 관한 의존성 패키지를 설치해달라고 합니다.
필요한 패키지를 설치하고, js를 ts로 바꾸면 타입스크립트 앱을 시작할 수 있습니다.

  tsconfig는 Next.js에서 자동으로 기록하는게 일반적이지만 필요할 경우 수정이 가능합니다.
이 때 Next.js가 바벨의 @babel/plugin-transform-typescript를 사용해요.

따라서 몇가지 주의사항이 있습니다.

1) const enum을 지원하지 않기에 바벨 설정에서 babel-plugin-const-enum을 추가해야합니다.
2) export & import 구문을 사용할 수 없습니다.
   babel-plugin-replacets-export-assignment를 설치하거나
   import x, {y} from ‘same-package’ 또는 export default x 와 같은 ECMAScript 구문으로 바꿔야해요.

이외에도 몇 가지 주의사항과 컴파일러 옵션이 기본 타입스크립트와 다르기 때문에 공식 바벨문서를 참고하세요.



## 3. 바벨 설정 커스터마이징


  바벨은 자바스크립트 ‘트랜스컴파일러’라고 합니다.
최신 자바스크립트 코드를 하위 호환성을 보장하는 스크립트 코드로 변환 시키는 것을 주로 담당합니다.
하위 호환성이 보장된다면 버전에 상관없이 모든 웹 브라우저에서 자바스크립트 코드를 실행할 수 있습니다.
또한 빠르게 발전하는 자바스크립트 코드의 발전 속도를 브라우저 또는 Node.js의 지원영역에 맞추지 않고,
현재의 환경에서 실행할 수 있습니다.

예를 들어 Node.js에서 export default를 사용해 Hello World를 출력해봅시다.

```jsx
"use strict";
Object.defineProperty(exports, "__esModule", {
	value: true
});
exports.default = _default;
function _default() {
console.log("Hello, World!");
```

이렇게 바뀐 코드는 오류 없이 실행됩니다.

  바벨 설정을 커스터마이징 하려면 최상단 디렉토리에 .babelrc 파일을 새로 만들면 됩니다.
이 파일에는 기본적으로 다음과 같은 내용을 저장해야 합니다.

```jsx
{
	"prests": ["next/babel"]
}
```

예를 들어  파이프라인 연산자를 Next에서 바벨을 통해 사용하려면 npm으로 바벨 플러그인을 설치해야합니다.

```jsx
npm install --save-dev @babel/plugin-proposal-pipeline-operator @babel/core
```

그리고 .babelrc 파일을 다음과 같이 수정합니다.

```jsx
{
	"prests": ["next/babel"],
	"plugins": [
		[
			"@babel/plugin-proposal-pipeline-operator",
			{ "proposal": "fsharp" }
		]
	]
}
```

이 다음에는 개발 서버를 재시작하고 기능을 사용해 볼 수 있습니다.


## 4. 웹팩 설정 커스터마이징


  웹팩은 특정 라이브러리, 페이지, 기능에 대해 컴파일된 코드를 전부 포함하는 번들로 만들어줍니다.
예를 들어 서로 각지 다른 라이브러리에서 각각 한 개씩 세걔의 컴포넌트를 불러오는 페이지를 만들었습니다.
웹팩은 이들을 클라이언트가 받아서 실행할 수 있는 하나의 번들로 합쳐줍니다.
자바스크립트파일, CSS, SVG 등 모든 자원에 대한 각각의 컴파일, 번들 최소화 작업을 조율하는 인프라인거죠.

  SASS, LESS 같은 CSS 전처리기를 사용해서 앱 스타일을 만들고 싶다면
웹팩 설정을 수정해서 위 파일들을 분석, 처리 후 CSS 파일들을 만들도록 해야합니다.
대부분 next.config.js 파일의 기본값을 변경하면 끝납니다.

```jsx
module.exports = {
	// 변경할 설정값
}
```

  기본 웹팩 설정을 바꾸려면 앞서 만든 객체의 webpack이라는 키에 새로운 속성값을 지정합니다.
만약 my-custom-loader라는 로더를 추가한다면 다음과 같이 변경합니다.

```jsx
module.exports = {
	webpack: (config, options) => {
		config.module.rules.push({
			test: /\.js/,
			use: [
				options.defaultLoaders.babel,
				{
					loader: "my-custom-loader", // 사용할 로더 지정
					options: loaderOptions,     // 로더의 옵션 지정
				}
			],
		});
		return config;
	},
}
```

  이런 식으로 웹팩 설정을 추가하면 나중에 Next.js의 기본 설정과 합쳐집니다.
가급적 기본 설정을 지우거나 직접 바꾸는 것이 아니라, 추가로 설정값을 확장하거나 덮어쓰는것이 권장됩니다.
