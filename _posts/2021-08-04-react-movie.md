---
title : "[React] movie-app 제작"
excerpt: "React JS를 이용한 영화추천 페이지"

categories:
  - react
tags:
  - [CSS, JSX, React]

toc: true
toc_sticky: true

date: 2021-08-04
last_modified_at: 2021-08-04
---

# 추천 영화 List로부터 영화를 소개시켜주는 movie app

## 기본 설정
- node.js
- npm i prop-types
  : 전달받은 props가 예상한 props인지 확인하기
- npm install react-router-dom
  : Router를 이용해 스크린간 이동
- npx create-react-app
- VS Code사용

> Application이 빈 html을 로드하고 react가 html을 밀어넣음(html을 반환하는 함수 component에 작성했던 코드) **▶** 소스코드에는 존재하지 않는 VirtualDOM을 만들어 속도가 빠른 React의 장점 활용


```js
import PropTypes from 'prop-types'

Movie.propTypes = {
  id : PropTypes.number.isRequired
}
//값이 없거나 잘못되었을 경우 console창에서 error 확인
```

### App.js

- 네비게이션을 만들어 주는 패키지 `npm install react-router-dom`
- react-router-dom 안의 많은 router 패키지 중 hashrouter, route를 import

#### Route
- /home이 될지, /about이 될지 url을 입력받은 라우터가 컴포넌트를 불러옴 
- `<Route>`에는 렌더링할 스크린과 뭘 할지 정해주는 2개의 props가 존재
- path와 component의 이름이 같아야 하진 않지만 편의를 위해 보통 일치
- react router는 url을 먼저 가져온 후 비교 하기에 2개의 component가 동시에 render되는 것을 방지하기위해 `exact={true}`를 설정
- HashRouter외 BrowserRouter도 있지만 github pages에 정확히 설정하기 까다로움

```js
import React from 'react';
import { HashRouter , Route} from "react-router-dom";
import About from "./routes/About";
import Home from "./routes/Home";
import Navigation from "./components/Navigation";
import Detail from "./routes/Detail";
import "./App.css";

function App(){
  return (
  <HashRouter>
    <Navigation/>
    <Route path="/" exact={true} component={Home} />
    <Route path="/about" component={About} />
    <Route path="/movie/:id" component={Detail} />
  </HashRouter>
  )
}

export default App;
```

### Navigation.js

- Home, about 2개의 스크린이 필요
- 기존 html사용 시 `a`태그와 `href`를 사용해 스크린을 이동하면 react는 죽고 새 페이지가 새로고침이 되기에 `Link to`태그를 이용
- router밖에서 Link를 사용하는 것은 불가능(App.js - HashRouter안 Navigation)
- 무조건 Router안에서 사용할 필요는 없음. 밖에 footer와 같은 다른 태그들도 사용가능

```js
import React from 'react';
import {Link} from "react-router-dom";
import "./Navigation.css"

function Navigation(){
  return (
  <div className="nav">
    <Link to="/">Home</Link>
    <Link to="/about">About</Link>
  </div>
  )
}

export default Navigation;
```

