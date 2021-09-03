---
title : "[React] Movie-app 제작"
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
- 네비게이션을 만들어 주는 패키지 `react-router-dom`
- react-router-dom속 많은 router 패키지 중 hashrouter, route를 import

#### Route
- 요청받은 pathname에 해당하는 component를 렌더링
- `<Route>`에는 렌더링할 스크린(path)과 뭘 할지(component)정해주는 2개의 props가 존재
  : 둘의 이름이 같아야 하진 않지만 편의를 위해 보통 일치
- react router는 url을 먼저 가져온 후 비교 하기에 2개의 component가 동시에 render되는 것을 방지하기위해 `exact={true}`를 설정
- BrowserRouter(#을 포함X)도 있지만 github pages에 정확히 설정하기 까다롭기에 HashRouter사용

```js
import { HashRouter , Route} from "react-router-dom";

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

- 기존 html사용 시 `a`태그 `href`를 사용해 스크린을 이동하면 react는 죽고 새 페이지가 새로고침이 되기에 `Link`태그 `to`를 이용
- Router안에 있는 모든 Route들은 props를 가지는데 이를 사용할 수 있음
> react-router 공식문서에 따르면 `to={{pathname : "",hash : "", state : {object : true}}}` 등으로 변경할 수도있음
- router밖에서 Link를 사용하는 것은 불가능

```js
import {Link} from "react-router-dom";

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

### Home.js

- YTSsite movie-list API를 가져오기 위해 axios를 활용
- 다소 시간이 소비되는 axios를 가진 conponentDidMount가 완료될 때 까지 기다리린 후 실행을 위한 async-await(thanks to ES6)
- 이에 isLoading값이 false로 변환되면 movies배열에 전달된 object에서 필요한 영화 정보만을 가지고 Movies.js를 호출

```js
import React from 'react';
import axios from 'axios';

class  Home extends React.Component {
  state = {
    isLoading: true,
    movies : []
  };

  getMovies = async() => {
    const {
      data: {
        data: { movies }
      }
    } = await axios.get("https://yts-proxy.now.sh/list_movies.json?sort_by=rating")
    this.setState({ movies, isLoading: false})
  }
    //movies배열에 영화 list넣은 후 loading 완료 -> wer'e ready
    //this.setState({ movies : movies})  setState의 movies : axios의 movies
  
  componentDidMount() {
    this.getMovies()
  }
  
  render(){  //react는 자동적으로 render실행
    const {isLoading, movies} = this.state
    return (
      <section className="container">
      { isLoading ? (
      <div className="loader"><span className="loader__text">Loading...</span> </div> 
      ) : (
      <div className="movies">
        {movies.map(movie =>(
            <Movie
              key={movie.id}
              id={movie.id}
              year={movie.year}
              title={movie.title}
              summary={movie.summary}
              poster={movie.medium_cover_image}
              genres={movie.genres}
            />
        ))}
      </div>
      )}
  </section>)
  }
}

export default Home;
```

### Movie.js
- Movie의 모든 props를 Link의 object로 사용하기 위해 props-type을 통한 error check
- 자바스크립트 문법을 이용해 `year : year, title : title`처럼 복잡하게 선언하지 않음
- item과 index 2가지 argument를 전달하는 map함수를 이용해 key값이 없는 오류를 해결

![image](https://user-images.githubusercontent.com/81230679/131988998-05a63d4f-dde8-451e-b873-a432373e1314.png)


### Detail
- 리스트의 영화 카드를 선택하지 않고 url을 직접 `https://dtwogud.github.io/movie-app-2021/#/movie/15553`와 같이 지정해 들어올 경우 object전달이 불가능하기에 Home으로 redirect
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

