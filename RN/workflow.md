## React의 workflow
HTML ➡️ Parsing ➡️ DOM Tree ➡️ Style (css ..) ➡️ Parsing ➡️ Render Tree ➡️ 
Attachment (synchronous) ➡️ Layout(reflow) ➡️ Painting

- DOM Tree : HTML을 파싱해서 DOM Tree의 생성
- Render Tree : 외부 CSS 파일과 각 엘리먼트의 inline 스타일을 파싱해서 새로운 Render Tree의 생성
- Attachment: 노드의 스타일을 처리하는 과정, 객체형태의 생성
- Layout : 좌표(위치)가 주어지는 과정
- Painting : 화면에 나타내는 작업

## React Native의 Workflow
RN 소스.js ➡️ Page Render ➡️ 리엑트 컴포넌트 DOM에 마운팅 ➡️ 브릿지 ➡️ 리엑트 컴포넌트 Rending (Objc api, java api .. ) ➡️ 해당 플랫폼의 Process

리엑트는 브라우저 DOM에 랜더링 하지만,
리엑트 네이티브는 Bridge(브릿지) 라는 개념을 도입해서 iOS, android 플렛폼으로 렌더링 합니다.
브릿지가 해당하는 플렛폼의 Native UI요소에 접근하는 인터페이스를 제공하기 때문에 가능한 것이지요.
DOM으로 랜더링하는 대신에 iOS의 경우는 Objc api를 호출하여 iOS 컴포넌트로 렌더링하여 처리합니다. 
android라면 java api를 호출하여 안드로이드 컴포턴트로 랜더링 하여 호출하겠죠.
