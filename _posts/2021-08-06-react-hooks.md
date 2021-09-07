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
<hr>

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

<hr>

## useBeforeLeave
- 마우스가 document를 벗어날때(탭을 닫을 때) 실행되는 function

```js
import React, { useState, useEffect, useRef } from "react";
import ReactDOM from "react-dom";

export const useBeforeLeave = (onBefore) => {
  useEffect(() => {
    document.addEventListener("mouseleave", handle);
    return () => document.addEventListener("mouseleave", handle);
  }, []);
  if (typeof onBefore !== "function") {
    return;
  }
  const handle = (event) => {
    const { clientY } = event;
    if (clientY <= 0) {
      onBefore();
    }
  };
};
```

### index.js

```js
import React, { useState, useEffect, useRef } from "react";
import ReactDOM from "react-dom";

const App = () => {
  const donbye = () => console.log("Plz don't leave");
  useBeforeLeave(donbye);
  return (
    <div className="App">
      <h1>Hello</h1>
    </div>
  );
};
```

<hr>

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

<hr>

## useFadeIn
- 자동으로 서서히 나타나는 하나의 element. element 안으로 나타나게 하기위해 useEffect를 다시 사용

```js
import React, { useState, useEffect, useRef } from "react";
import ReactDOM from "react-dom";

export const useFadeIn = (duration = 1, delay = 0) => {
  if(typeof duration !== "number" || typeof delay !== "number"){
    return;
  }
  const element = useRef();
  useEffect(() => {
    if (element.current) {
      console.log(element.current);
      const { current } = element;
      current.style.transition = `opacity ${duration}s ease-in-out ${delay}s`;
      current.style.opacity = 1;
    }
  }, []);
  if (typeof duration !== "number" || typeof delay !== "number") {
    return;
  }
  return { ref: element, style: { opacity: 0 } };
};
```

### index.js

```js
import React, { useState, useEffect, useRef } from "react";
import ReactDOM from "react-dom";

const App = () => {
  const FadeInH1 = useFadeIn(1, 3);
  const FadeInP = useFadeIn(5, 2);
  return (
    <div className="App">
      <h1 {...FadeInH1}>Hello</h1>
      // <h1 ref={FadeInH1} style={{opacity :0}}>Hi</h1>  //useFadeIn()에서 return값이 element만 있을경우 일일이 설정
      <p {...FadeInP}>Hello</p>
    </div>
  );
};
```

<hr>


## useFullscreen
- image를 fullscreen으로 만들어줌
- 전체화면이 아닐ㄷ 때 "Exit Fullscreen"을 누르면 발생하는 오류를 해결하기 위해 document.fullscreenElement로 전제화면인지 체크한 후 아닐 경우에만 document.exitFullscre()을 실행

```js
export const useFullscreen = (callback) => {
  const element = useRef();
  const runCb = (isFull) => {
    if (callback && typeof callback === "function") {
      callback(isFull);
    }
  };
  const triggerFull = () => {
    if (element.current) {
      if (element.current.requestFullscreen) {
        element.current.requestFullscreen();
      } else if (element.current.mozRequestFullScreen) {
        element.current.mozRequestFullScreen();
      } else if (element.current.webkitRequestFullscreen) {
        element.current.webkitRequestFullscreen();
      } else if (element.current.msRequestFullscreen) {
        element.current.msRequestFullscreen();
      }
      runCb(true);
    }
  };
  const exitFull = () => {
    const checkFullScreen = document.fullscreenElement;
    if (checkFullScreen !== null) {
      document.exitFullscreen();
      if (document.exitFullscreen) {
        document.exitFullscreen();
      } else if (document.mozCancelFullScreen) {
        document.mozCancelFullScreen();
      } else if (document.webkitExitFullscreen) {
        document.webkitExitFullscreen();
      } else if (document.msExitFullscreen) {
        document.msExitFullscreen();
      }
      runCb(false);
    }
  };
  return { element, triggerFull, exitFull };
};
```

### index.js

```js
import React, { useState, useEffect, useRef } from "react";
import ReactDOM from "react-dom";

const App = () => {
  const onFulls = (isFull) => {
    console.log(isFull ? "We're Full" : "We aren't Full");
  };
  const { element, triggerFull, exitFull } = useFullscreen(onFulls);
  return (
    <div className="App">
      <div ref={element}>
        <img src="./k.jpg"/>
        <button onClick={exitFull}>Exit Fullscreen</button>
      </div>
      <button onClick={triggerFull}>Make Fullscreen</button>
    </div>
  );
};
```

<hr>

## useHover

```js
export const useHover = onHover => {
  if (typeof onHover !== "function") {
    return;
  }
  const element = useRef();
  useEffect(() => {
    if (element.current) {
      element.current.addEventListener("mouseenter", onHover);
    }
    return () => {
      if (element.current) {
        element.current.removeEventListener("mouseenter", onHover);
      }
    };
  }, []);
  return element;
};
```

### index.js

```js
import ReactDOM from "react-dom";
import React, { useState, useEffect, useRef } from "react";

const App = () => {
  const onClick = () => {
    console.log("Say Hello");
  }
  const title = useHover(onClick);
  return(
    <div className="App">
      <h1 ref={title}>Hi</h1>
    </div>
  )
}
```

<hr>


## useNetwork
- navigator가 online 또는 offline이 되는걸 막아주는 역할
- 브라우저 console-Network에서 online, offline 설정 가능

```js
export const useNetwork = (onChange) => {
  const [status, setStatus] = useState(navigator.onLine);
  const handleChange = () => {
    if (typeof onChange === "function") {
      onChange(navigator.onLine);
    }
    setStatus(navigator.onLine);
  };
  useEffect(() => {
    window.addEventListener("online", handleChange);
    window.addEventListener("offline", handleChange);
    () => {
      window.removeEventListener("online", handleChange);
      window.removeEventListener("offline", handleChange);
    };
  }, []);
  return status;
};
```

### index.js

```js
import React, { useState, useEffect, useRef } from "react";
import ReactDOM from "react-dom";

const App = () => {
  const handleNetworkChange = (online) => {
    console.log(online ? "online" : "offline");
  };
  const online = useNetwork(handleNetworkChange);
  return (
    <div className="App">
      <h1>{online ? "Online" : "Offline"}</h1>
    </div>
  );
};
```

<hr>


## useNotification
- 구글 크롬 알림처럼 notification을 해줌
- console에서 new Notofocation("hi")처럼 확인 가능

```js
export const useNotification = (title, options) => {
  if (!("Norofication" in window)) {
    return;
  }
  const fireNotif = () => {
    if (Notification.permission !== "granted") {
      Notification.requestPermission().then((permission) => {
        if (permission === "granted") {
          new Notification(title, options);
        } else {
          return;
        }
      });
    } else {
      new Notification(title, options);
    }
  };
  return fireNotif;
};
```

### index.js

```js
import React, { useState, useEffect, useRef } from "react";
import ReactDOM from "react-dom";
import k from "./k.jpg";

const App = () => {
  const triggerNotif = useNotification("Hello mo'fucker?", {
    body: "i want money"
  });
  return (
    <div className="App">
      <button onClick={triggerNotif}>Notification</button>
    </div>
  );
};
```

<hr>


## useScroll
- 마우스 스크롤을 이용해 content를 지나쳤을 때 효과를 줌

```js
export const useScroll = () => {
  useEffect(() => {
    window.addEventListener("scroll", onscroll);
    return () => {
      window.removeEventListener("scroll", onscroll);
    };
  }, []);
  const onscroll = () => {
    setState({ y: window.scrollY, x: window.scrollX });
    console.log("Y", window.scrollY, "X", window.scrollX);
  };
  const [state, setState] = useState({
    x: 0,
    y: 0
  });
  return state;
};
```

### index.js

```js
import React, { useState, useEffect, useRef } from "react";
import ReactDOM from "react-dom";
import useScroll from "./useScroll";

const App = () => {
  const { y } = useScroll();
  return (
    <div className="App" style={{ height: "1000vh" }}>
      <h1 style={{ position: "fixed", color: y > 100 ? "red" : "blue" }}>
        hello
      </h1>
    </div>
  );
};
```

<hr>


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

<hr>


## usePreventLeave (useState, useEffect를 사용하지 않는 hook아닌 hook..)
- window창을 닫을 때 저장하지 않는 등의 문제 해결을 위한 알림창
- beforeunload는 window가 닫히기 전에 function이 실행되는걸 허락

```js
export const usePreventLeave = () => {
  const listener = (event) => {
    event.preventDefault();
    event.returnValue = "";  //chtome에서 해당구문 없으면 동작 안함
  };
  const enablePrevent = () => window.addEventListener("beforeunload", listener);
  const disablePrevent = () =>
    window.removeEventListener("beforeunload", listener);
  return { enablePrevent, disablePrevent };
};
```

### index.js

```js
import ReactDOM from "react-dom";
import usePreventLeave from "./usePreventLeave.js";
export { usePreventLeave as default } from "./usePreventLeave";

const App = () => {
  const {enablePrevent, disablePrevent} = usePreventLeave();
  return(
    <div className="App">
      <button onClick={enablePrevent}>protect</button>
      <button onClick={disablePrevent}>unprotect</button>
    </div>
  )
}
```

<hr>


## useConfirm (useState, useEffect를 사용하지 않는 hook아닌 hook..)
- 사용자가 동작하기 전 확인하는 것(like Ary ypu sure?)

```js
export const useConfirm = (message = "", onConfirm, onCancel) => {
  if (!onConfirm || typeof onConfirm !== "function") {   //onConfirm 없는 경우 typeof 검사에서 undefined로 필터링 되기에 !onConfirm은 안써도 될듯..
    return;
  }
  if (onCancel && typeof onCancel !== "function") {
    return;
  }
  const confirmAction = () => {
    if (window.confirm(message)) {
      onConfirm();
    } else {
      onCancel();
    }
  };

  // const confirmAction = () => {
  //   if (window.confirm(message)) {
  //   onConfirm();
  //   } else {
  //     try {
  //       onCancel();
  //     } catch (error) {
  //       return;
  //     }
  //   }
  // };
  //   //onCancel()은 필수가 아니라 없는 경우에도 Cancel을 누르면 실행 되기에 예외발생으로 프로그램 오류 방지

  return confirmAction();
};
```

### index.js

```js
import { StrictMode } from "react";
import ReactDOM from "react-dom";
import useConfirm from "./useConfirm.js";
export { useConfirm as default } from "./useConfirm";

const App = () => {
  const deleteWorld = () => {console.log("Delete the World")}
  const abort = () => {console.log("Aborted")}
  const confirmDelete = useConfirm("Are you sure?", deleteWorld, abort)
  return(
    <div className="App">
      <button onClick={confirmDelete}>Delete the World</button>
    </div>
  )
}
```

<hr>


## useAxios
- HTTP request를 만드는 컴포넌트

```js
import defaultAxios from "axios";
import { useState, useEffect } from "react";

const useAxios = (opts, axiosInstance = defaultAxios) => {
  const [state, setState] = useState({
    loading: true,
    error: null,
    data: null
  });
  const [trigger, setTrigger] = useState(0);
  if (!opts.url) {
    return;
  }
  const refetch = () => {
    setState({
      ...state,
      loading: true
    });
    setTrigger(Date.now());
  };
  useEffect(() => {
    axiosInstance(opts)
      .then((data) => {
        setState({
          ...state,
          loading: false,
          data
        });
      })
      .catch((error) => {
        setState({ ...state, loading: false, error });
      });
  }, [trigger]);
  return { ...state, refetch };
};

export default useAxios;
```

### index.js

```js
import React, { useState, useEffect, useRef } from "react";
import ReactDOM from "react-dom";
import useAxios from "./useAxios";

const App = () => {
  const { loading, error, data, refetch } = useAxios({
    url: "https://yts-proxy.now.sh/list_movies.json?sort_by=rating"
  });
  console.log(
    `Loading : ${loading}\n Data : ${JSON.stringify(data)}\n Error : ${error}`
  );
  return (
    <div className="App">
      <h1>{data && data.status}</h1>
      <h2>{loading && "Loading"}</h2>
      <button onClick={refetch}>Refetch</button>
    </div>
  );
};
```

<hr>

# NPM publishing

```
//terminal
npm init

package name : @mooks/use-title
description : React Hook to update yout document's title~
git repository : 깃 주소
keywords : react, react hooks
author : MJ
```

- terminal을 통해 위와 같이 정리해주고 난 후 package.json 확인
  - publish가 중요한 목적이라면 package.json 내` "main" : "index.js"`는 필수로 있어야 함

- useState, useEffect도 함께 설치를 해 주어야 함
- `package.json - "dependencies"  ➡️ "peerDependencies"` : 요구되지만 설치할 필요 없다는 의미

```
//terminal
npm i react react-dom
```

- https://npmjs.com/org/mooks - create

```
//terminal
npm login
npm publish --access public
```

- codesandbox 에서 react 프로젝트 `Add Dependency`를 통해 확인 가능!

```js
import useTitle from "@mooks/use-title";
```