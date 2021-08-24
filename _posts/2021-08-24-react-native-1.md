---
title : "[React Native] Weather App"
excerpt: "위치기반 현재날씨 정보 Upload"

categories:
  - react
tags:
  - [JavaScript, React]

toc: true
toc_sticky: true

date: 2021-08-24
last_modified_at: 2021-08-24
---
# ㅁWeather App 제작

## React Native
  - 페이스북이 개발한 오픈소스 모바일 어플리케이션 프레임워크로 안드로이드, iOS, 웹 용 어플리케이션 개발을 위해 사용되며, 개발자들이 네이티브 플랫폼 기능과 더불어 리액트를 사용할 수 있게 한다.
  : React의 방식으로 네이티브 앱을 개발하게한다~

  ### React와 React-Native
  - 

```js
document.addEventListener('DOMContentLoaded', () => {
  const h1 = (text) => `<h1>${text}</h1>`
  document.body.innerHTML += h1('DOMContentLoaded 이벤트 발생')
})
```

### 문서 객체 가져오기

|이름|선택자 형태|설명|
|---|---|---|
|태그 선택자|태그|특정 태그를 가진 요소를 추출|
|아이디 선택자|#아이디|특정 id속성을 가진요소를 추출|
|클래스 선택자|.클래스|특정 class속서을 가진 요소를 추출|
|속성 선택자|[속성=값]|특정 속성 값을 갖고있는 요소를 추출|
|후손 선택자|선택자_A 선택자_B|선택자_A아래에 있는 선택자_B를 선택|

- querySelector() : 문서 객체 읽어들이는 함수
- querySelectorAll() : 문서 객체 여러개를 배열로 읽어들이는 함수
- 문서객체.textContent ='' : 입력된 문자열을 그대로 기입
- 문서객체.innerHTML ='' : 입력된 문자열을 HTML형식으로 기입
- 문서객체.setAttribute(속성이름, 값) : 특정 속성에 값을 지정
- 문서객체.getAttribute(속성이름) : 특정 속성을 추출

```js
document.addEventListener('DOMContentLoaded',()=>{
  const header = document.querySelector('h1')
  const a = document.querySelector('#a')
  const b = document.querySelector('#b')
  const rects = document.querySelector('.rect')

  header.textContent = 'HEADERS'
  hearer.style.color = 'white'
  header.style.backgroundColor = 'black'
  header.style.padding = '10px'

  a.textContent ='<h1>textContent속성</h1>'
  b.textContent ='<h1>textContent속성</h1>'

rects.forEach(rect, index)=>{
  const width = (index+1)*100
  const src = `https://placekitten.com/${width}/250`
  rects.setAttribute('src',src)
}
})
```

### 문서 객체 조작

- document.createElement(문서 객체 이름)
- 부모 객체.appendChild(자식 객체)
- removeChild() : 문서 객체 제거
- appendChild() 메소드 등으로 부모 객체와 이미 연결이 완료된 문서 객체의 경우 parentNode 속성으로 부모 객체에 접근할 수 있으므로, 일반적으로 어떤 문서 객체를 제거할 때는 다음과 같은 형태의 코드를 사용


```js
문서객체.parentNode.removeChild(문서객체)
```
<br>

```js
document.addEventListener('DOMContentLoaded', () => {
  const header = document.createElement('h1')

  header.textContent='문서 객체 동적으로 생성'
  header.setAttribute('data-custom','사용자 정의 속성')
  header.style.color = 'white'
  header.style.backgroundColor = 'black'

  document.body.appendChild(header)
})
```

- 문서객체.addEventListener(이벤트이름, 콜백함수)
- 문서객체.removeEventListener(이벤트이름, 이벤트리스너)

```js
h1.addEventListener('click',{event} => {
  counter++
  h1.textContent=`클릭횟수:${counter}`
})
```

- 키보드 이벤트
  - keydown : 키가 눌릴 때 실행. 키보드를 누르고 있을때도 입력 실행됨
  - keypress : 키가 입력되었을 때 실행(웹 브라우저에서 아시아권문자를 제대로 처리못하는 문제가 있음)
  - keyup : 키보드에서 키가 떨어질 때 실행
  - 키보드 키 코드
    - code : 입력한 키
    - keyCode : 입력한 키를 나타내는 숫자
    - ${event.altKey} : alt키 눌렀는지
    - ${event.ctrlKey} : ctrl키 눌렀는지
    - ${event.shiftKey} : shift키 눌렀는지
      **→** code속성은 입력한 키를 나타내는 문자열이 들어있음

