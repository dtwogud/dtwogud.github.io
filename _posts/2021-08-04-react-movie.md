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
- prop-types
  : 전달받은 props가 예상한 props인지 확인하기

```js
$ npm i prop-types
import PropTypes from 'prop-types'
```

- axios
  : node.js와 브라우저를 위한 HTTP통신 라이브러리.data fetch

```js
$ npm i axios
import axios from 'axios';
```

- react-router-dom
  : Router를 이용해 스크린간 이동

```js
$ npm install react-router-dom
import { HashRouter , Route} from "react-router-dom";
```

- npx create-react-app
- VS Code사용

> Application이 빈 html을 로드하고 react가 html을 밀어넣음으로써 소스코드에는 존재하지 않는 VirtualDOM을 만들어 속도가 빠른 React의 장점 활용

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

#### Router

- 기존 html사용 시 `a`태그와 `href`를 사용해 스크린을 이동하면 react는 죽고 새 페이지가 새로고침이 되기에 `Link to`태그를 이용
> react-router 공식문서에 따르면 `to={{pathname : "",hash : "", state : {object : true}}}` 등으로 변경할 수도있음
- router밖에서 Link를 사용하는 것은 불가능(App.js - HashRouter안 Navigation)
- 무조건 Router안에서 사용할 필요는 없음. 밖에 footer와 같은 다른 태그들도 사용가능
- Router안에 있는 모든 Route들은 props를 가지는데 이를 사용할 수 있음

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

### Movie.js

- JS에서 data fetch를 위해 fetch사용하는 방법 외에 axios도 사용


- Movie의 모든 props를 Link의 object로 사용
- 자바스크립트 문법을 이용해 `year : year, title : title`처럼 복잡하게 선언하지 않음

```js
import React from 'react';
import PropTypes from 'prop-types'
import {Link} from "react-router-dom";
import './Movie.css'

function Movie({id, year, poster, summary, title, genres}){
  return (
    <div className="movie">
      <Link to={{
        pathname:`/movie/${id}`,
        state:{
          year,
          title,
          summary,
          poster,
          genres
        }
      }}>
      <img src={poster} alt={title} title={title} />
      <div className="movie__data">
        <h3 className="movie__title"> {title} </h3>
        <h5 className="movie__year">{year}</h5>
        <ul className="movie__genres">
          {genres.map((genre, index) => (
            <li key={index} className="genres__genre">{genre}</li>
          ))}
        </ul>
        <p className="movie__summary">{summary.slice(0, 140)}...</p>
      </div>
    </Link>
    </div>
  )
}

Movie.propTypes = {
  id : PropTypes.number.isRequired,
  year : PropTypes.number.isRequired,
  poster : PropTypes.string.isRequired,
  summary : PropTypes.string.isRequired,
  title : PropTypes.string.isRequired,
  genres : PropTypes.arrayOf(PropTypes.string).isRequired
}

export default Movie;
```

### Detail
- 리스트의 영화 카드를 선택하지 않고 url을 직접 `https://dtwogud.github.io/movie-app-2021/#/movie/15553`와 같이 지정해 들어올 경우 object전달이 불가능하기에 Home으로 reeirect
- 전달된 object중 웹사이트가 어디있는지 장소를 바꿀 수 있는 history를 사용

```js
import React from 'react';

class Detail extends React.Component {

  componentDidMount() {
    const {location, history} = this.props;
    if (location.state === undefined) {
      history.push("/");
    }
    console.log(location.state);
  }

  render() {
    const {location, history} = this.props;
    if (location.state){
      return (
        <div>
      <span>{location.state.title}</span><br></br>
      <span>{location.state.summary}</span>
      </div>
        )
    }
    else {
      return null;
    }
  }
}

export default Detail;
```