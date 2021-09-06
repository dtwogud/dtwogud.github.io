---
title : "[React] React Hooks "
excerpt: "React Hooks 사용 및 NPM Package 등록"

categories:
  - react
tags:
  - [React, JS, Hooks ]

toc: true
toc_sticky: true

date: 2021-08-06
last_modified_at: 2021-08-06
---

![image](https://user-images.githubusercontent.com/81230679/132156097-8b7d281c-3461-4aa6-96cb-d4df7d7433bf.png)

> https://ko.reactjs.org/docs/hooks-intro.html

# React Hook

> React 버전 16.8부터 새로 추가된  Hook은 기존 Class 바탕의 코드를 작성할 필요 없이 상태 값과 여러 React의 기능을 사용. function component에서 state을 가질 수 있게 되어 React hook을 사용해 App을 만들 때 class component, render 등을 사용하지 않고 하나의 function이 되는 것. 즉 함수형 프로그래밍이 가능해지는 것

- 기존 클래스형 컴포넌트의 단점을 Hooks를 이용해 보완가능
  - 클래스형 컴포넌트에서 로직 재사용 시 사용하는 고차컴포넌트, 렌더속성값 패턴은 리액트 요소 트리를 깊게 만들어 성능에 부정적인 영향과 개발 시 디버깅이 힘들어지는 문제점 발생
  - 거로 연관성 없는 로직들을 하나의 생명주기에서 작성하는 경우 발생
  - JS의 클래스 문법, this등의 이해도 필요..

- 👍 hook을 이용하면?
여러 hook들끼리 재조립을 통한 재사용 가능한 로직 제작 가능(한곳에 모을 수 있음)


## useState와 useEffect

### useState
- useState는 함수형 컴포넌트에서 상탯값 관리
- state값과 이 값을 업데이트 하는 함수를 쌍으로 제공. 클래스 컴포넌트에서 this.setState와 유사하지만 이전 state와 새로운 state를 합치지 않는 차이점
- useState는 인자로 초기 state설정값을 하나 받는데 이 초기값은 첫 번째 렌더링 시에 한번 사용됨

```js
const [state, setState] = useState(intialState);
setState(newState);
```
### useEffect
- 첫번째는 function, 두번째는 deps(dependency : 리스트에 있는 값일 때만 값이 변하도록 활성화) 두 개의 인자를 가짐
- 어떤 effect(명령형함수, 타이머, 로깅, 변형, DOM을 조작하는 side effects)를 발생시키고 싶을 때 사용.
- mount될 때 동작, unmount될 때 이벤트가 발생한 뒤 정리 필요
- useEffect에 전달된함수는 렌더링 완료 후에 실행되지만, 어떤 값이 변경됐을 경우에 실행하게도 가능. useEffect는 렌더링 결과가 실제 돔에 반영 된 후에 호출됨.(공식문서 참조)
https://ko.reactjs.org/docs/hooks-reference.html#useeffect

```js
const App = () => {
  const sayHello = () =>console.log("hello");
  // useEffect(sayHello, [number]);  //useEffect의 위치에 따른 console변화
  const [number, setNumber] = useState(0);
  const [aNumber, setAnumber] = useState(0);
    useEffect(sayHello, [number]);    //number가 바뀔때만 sayHello 실행(deps !!)
  return(
    <div className = "App">
      <div>Hi</div>
      <button onClick = {()=>{setNumber(number + 1)}}>
      <button onClick = {()=>{setAnumber(aNumber + 1)}}>
    </div>
  )
}
```

> componentDidMount와 componentDidUpdate와는 다르게, useEffect로 전달된 함수는 지연 이벤트 동안에 레이아웃 배치와 그리기를 완료한 후 발생 그렇지만, 모든 effect가 지연될 수는 없음. 예를 들어 사용자에게 노출되는 DOM 변경은 사용자가 노출된 내용의 불일치를 경험하지 않도록 다음 화면을 다 그리기 이전에 동기화 되어야 하기에 useEffect는 브라우저 화면이 다 그려질 때까지 지연되지만, 다음 어떤 새로운 렌더링이 발생하기 이전에 발생하는 것도 보장. React는 새로운 갱신을 시작하기 전에 이전 렌더링을 항상 완료

### 👆 Hooks사용 규칙
- 최상위(at the top level)에서만 Hook을 호출. 반복문, 조건문, 중첩된 함수 내에서 Hook을 실행 **X**
- React 함수 컴포넌트 내에서만 Hook을 호출. 일반 JavaScript 함수에서는 Hook 호출 **X** (Hook을 추가로 호출할 수 있는 단 한곳이 직접 작성한 custom Hook.)

  - 당연한 말이지만 react와 node.js 설치
  - package publish를 위한 npm 설치
  - hooks를 자동으로 테스트 하기

## useTitle
- 문서 제목 Update

```js
export const useTitle = initialTitle => {
  const [title, setTitle] = useState(initialTitle);
  const updateTitle = () => {
    const htmlTitle = document.querySelector("title");   //<title>
    htmlTitle.innerText = title;
  };
  useEffect(updateTitle, [title]);
  return setTitle;
};
```

### index.js

```js
import ReactDOM from "react-dom";
import useTabs from "./useTabs.js";
import React, { useState, useEffect, useRef } from "react";
import useTitle from "./useTitle.js";

const App = () => {
  const titleUpdater = useTitle("Loading...");
  setTimeout(() => {titleUpdater("Home")},5000);
  return(
    <div className="App">
      <h1>Hi</h1>
    </div>
  )
}
);
```

## useInput
- useInput의 initialValue와 유효성 검사를 가능하게 해주는 validator를 사용

```js
export const useInput = (initialValue, validator) => {
  const [value, setValue] = useState(initialValue);
  const onChange = (event) => {
    const {
      target: { value } //event.target.value
    } = event;
    let willUpdate = true;
    if (typeof validator === "function") {
      willUpdate = validator(value);
    }
    if (willUpdate) {
      setValue(value);
    }
  };
  return { value, onChange };
};
```

### index.js

```js
import React, { useState, useEffect, useRef } from "react";
import ReactDOM from "react-dom";

const App = () => {
  const maxLen = (value) => value.length <= 10;
  // const maxLen = (value) => !value.lnclude("@");  //특수문자 입력방지 예시
  const name = useInput("Mr.", maxLen);

  return (
    <div className="App">
      <input placeholder="Name" {...name} />
    </div>
  );
};

const rootElement = document.getElementById("root");
ReactDOM.render(
  <StrictMode>
    <App />
  </StrictMode>,
  rootElement
);
```

## usePageLeave



### index.js

## useClick
- useRef() : 기본적으로 component의 특정 부분을 선택할 수 있는 방법(like document.getElementByID()) 

```js
const example = useRef();
setTimeout(() => {example.current.focus()}, 5000);
return(
  <input ref = {example} placeholder ="Hi" />
)
```

- useClick함수를 정의하고 그 안에서 useEffect(function, [])함수를 호출하면 페이지가 열리는 시작점에서 내부 function을 호출하고 []로 인해 페이지가 시작하는 최초에만 함수발생, 하지만 내부 function에 return값을 주면 페이지가 종료되는 시점에 return을 실행시켜 useClick함수에서 함수를 파라미터로 던지고 받을 수 있음

```js
export const useClick = (onClick) => {
  if (onClick !== "function"){
    return ;
  }
  const element = useRef();
  useEffect(() => {
    if (element.current) {
      element.current.addEventListener("click", onClick);
    }
    //useEffect는 mount될 때 동작, unmount될 때 이벤트가 발생한 뒤 정리 필요. component가 mount되지 않았을 때 eventListener가 배치되지 않기 위해 아래 추가
    return () => {
      if (element.current) {
        element.current.removeEventListener("click", onClick);
      }
    };
  }, []);   //componentDidMount때 한번만 실행되라는 의미(deps 존재**X**)
  return element;
};
```

### index.js

```js
import ReactDOM from "react-dom";
import React, { useState, useEffect, useRef } from "react";
import useClick from "./useClick.js";

const App = () => {
  const onClick = () => console.log("Say Hello");
  const title = useClick(onClick);
  return(
    <div className="App">
      <h1 ref={title}>Hi</h1>
    </div>
  )
}
```
## useFadeIn



### index.js

## useFullscreen



### index.js

## useHover



### index.js

## useNetwork



### index.js

## useNotification



### index.js

## useScroll



### index.js

## useTabs
- API와 같은 다른 무언가로부터 가져올 data를 content로 지정
- allTabs의 기본 인덱스는 `0`

```js
export const useTabs = (initialTab, allTabs) => {
  const [currentIndex, setCurrentIndex] = useState(initialTab);
  if (!allTabs || !Array.isArray(allTabs)) return;
  return {
    currentItem: allTabs[currentIndex],
    changeItem: setCurrentIndex
  };
};
```

### index.js

```js
import ReactDOM from "react-dom";
import useTabs from "./useTabs.js";
import React, { useState, useEffect, useRef } from "react";

const content = [
  {
    tab : "section1",
    content : "I'm the section 1"
  },
  {
    tab : "section2",
    content : "I'm the section 2"
  }
]

const App = () => {
  const {currentItem, changeItem} = useTabs(0, content)
  return(
    <div className = "App">
      {content.map(
        (section, index) => (
        <button onClick={() => changeItem(index)}>{section.tab}</button>
      ))}
      <div>{currentItem.content}</div>
    </div>
  )
}
```

## usePreventLeave



### index.js

## useConfirm



### index.js

## useAxios



### index.js
