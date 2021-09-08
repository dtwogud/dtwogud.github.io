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
<br>
# ㅁWeather App 제작

## React Native
> 페이스북이 개발한 오픈소스 모바일 어플리케이션 프레임워크로 안드로이드, iOS, 웹 용 어플리케이션 개발을 위해 사용되며, 개발자들이 네이티브 플랫폼 기능과 더불어 리액트를 사용할 수 있게 한다.
> `:` React의 방식으로 네이티브 앱을 개발하게한다~

### React와 React-Native
  ■ React : DOM이 생성되고 난 뒤 Virtual Dom을 이용해 변화된 곳을 확인하고 Dom으로 변경<br>
  ■ React-Native : Bridge를 이용해 iOS, amdroid 각각의 네이티브 언어에 접근할 수 있게 함
<br>

  -  화면출력

```js
// React
ReactDOM.render(<App />, document.getElementById('root'));

// React-Native
AppRegistry.registerComponent(appName, () => App);
```

  - HTML 문법
    - React
    - React-Native

```js
// React
import React from 'react';

class HelloMessage extends React.Component {
  render() {
    return (
      <div>
        <h1>React</h1>
        Hello React!
      </div>)}}

// React-native
import React from 'react';
import {View, Text} from 'react-native'; 

class HelloMessage extends React.Component {
  render() {
    return (
      <View>
        <Text>React-Native   Hello React-Native!</Text>
      </View>)}}
```

  1. CSS 미지원
  - 기존과 다른 CSS 문법

```js
...
<Text style={styles.text}>Getting the Weather</Text>
...
const styles = StyleSheet.create({
  text : {
    color : "grey",
    fontSize : 35
  }
})
```

## 기획의도

1. 가까이 있지만 멀게만 느껴진 모바일 기기의 개발환경 경험
2. 익숙치 않은 API, 환경설정 등에서 시행착오 절감
3. React기반

- 고려사항
  - **시인성** : 대게 작은 화면의 모바일 기기에서 파악이 쉽도록
  - **정확성** : 정확한 위치, 정확한 날씨
  - **호환성** : android, iOS 환경

## 주요기술
>모듈 설치  

### 모듈 설치
- <span style='background-color : grey'><a style='color : greenyellow'>Node.js</a></span> : JavaScript활용
- <span style='background-color : grey'><a style='color : greenyellow'>npm</a></span> : Node.js로 만들어진 Package 관리
- <span style='background-color : grey'><a style='color : greenyellow'>axios</a></span> : API로부터 data fetch

`npm install axios`

- <span style='background-color : grey'><a style='color : greenyellow'>expo</a></span> : 빠르게 App을 만들 수 있도록 도와주는 툴체인
> native-modules를 이용해서도 iOS(swift, object-c) or android(java, kotlin) 진행가능. 온전히 내 편의를 위해

`npm install -g expo-cli`
`expo init Project이름`
`npm start`

궁금하면 설치..
![image](https://user-images.githubusercontent.com/81230679/130633736-f6b563f0-ad34-4586-b5ce-4a58637b1beb.png)

### 화면 분할
- **App.js** : App이 실행 후 API data fetch, Loading, Weather 호출
- **Loading.js** : data fetch완료까지 대기화면
- **Weather.js** : 현재 날씨및 기온정보 업로드

![image](https://user-images.githubusercontent.com/81230679/130630486-35a10c90-9d93-469f-aa94-5920e5f59e75.png)


#### App.js - 위치(위도,경도)기반 API를 이용해 날씨정보 호출
- 모바일 기기에서 위도와 경도 확인
- https://openweathermap.org/ 를 이용해 날씨정보 추출(모바일 기기 위도,경도 사용)

```js
  getLocation = async () => {
    try {
      await Location.requestForegroundPermissionsAsync();
      const {
        coords : {latitude, longitude}
      } = await Location.getCurrentPositionAsync()
      this.getWeather(latitude,longitude)
    }
    catch (error) {
      Alert.alert("Can't find you.","So sad...")
    }
  }
  ```

![coords](https://user-images.githubusercontent.com/81230679/130614954-f6d6d3f4-7687-4b7e-87e9-7c0669be211c.PNG)

```js
  getWeather = async (latitude, longitude) => {
    const {data:
      {main: 
        {temp},
        weather,
        name
      }
    } = await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=${API_KEY}&units=metric`
      );
      this.setState({
        isLoading: false,
        condition : weather[0].main,
        temp,
        name
      });
  }
```

```js
render() {
    const {isLoading, temp, condition, name} = this.state;
    return(
      isLoading ? <Loading /> : <Weather temp={Math.round(temp*10)/10} condition = {condition} name={name}/>
    )
  }
  ```

#### Loading.js - Loading화면 출력, 위치 데이터 Permission

```js
export default function Loading(){
  return <View style={styles.container}>
    <Text style={styles.text}>Getting the Weather</Text>
    <StatusBar barStyle="light-content" />
  </View>
}
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent : "flex-end",
    paddingHorizontal : 30,
    paddingVertical :100,
    backgroundColor : "#FDF6AA"
  },
  text : {
    color : "grey",
    fontSize : 35
  }})
```

#### Weather.js - App.js로부터 전달받은 위치, 날씨정보를 기반으로 현재상태 출력

```js
const weatherOptions = {
  Thunderstorm: {
    iconName: "thunderstorm",
    gradient: ["#403B4A", "#E7E9BB"],
    title: "Thunderstorm outside",
    subtitle: "God of Thunder is comming...",
    music : "FT아일랜드 - 천둥"
  }
  ...
```

```js
export default function Weather({temp, condition, name}){
  return(
      <LinearGradient
        colors={weatherOptions[condition].gradient}
        style={styles.container}
      >
        <StatusBar barStyle="light-content" />
      <View style={styles.halfContainer}>
      <Ionicons
        name={weatherOptions[condition].iconName}
        size={80}
        color="white"
      />
      <Text style={styles.temp}>{temp} ℃</Text>
      <Text style={styles.city}>You're now in {name}</Text>
      </View>
      <View style={{...styles.halfContainer, ...styles.textContainer}}>
        <Text style={styles.title}>{weatherOptions[condition].title}</Text>
        <Text style={styles.subtitle}> : {weatherOptions[condition].subtitle}</Text>
        <Text style={styles.musicSuggest}>날씨랑 어울리는 추천곡</Text>
        <Text style={styles.music}> : {weatherOptions[condition].music}</Text>
        <Text style={styles.musicGo} onPress={() =>  Linking.openURL(`https://www.google.com/search?q=${weatherOptions[condition].music}`)}>들으러 가기</Text>
      </View>
      </LinearGradient>
  )
}
```

```js
const styles = StyleSheet.create({
  
  container : {
    flex : 1,
    justifyContent : "center",
    alignItems : "center"
  }
  ...
```

## 정리
### 후기
- 습득한 기술을 활용
  - API, React, React-Native, Modules, Library, Design, Debug
- 아쉬운점
  - 위치의 정확성(약 10m - 2km 의 오차발생)
  - 아이콘(expo vector-icon사용)
  - 디자인(조금 더 이쁘게..?)

![image](https://user-images.githubusercontent.com/81230679/130632925-37c945f3-5f44-42cd-8405-e32c61bbb012.png)

![image](https://user-images.githubusercontent.com/81230679/130620201-0d276cb1-43c7-41bd-82f1-9b1c04a56f15.png)

![image](https://user-images.githubusercontent.com/81230679/130620269-49738e3f-43be-4bfc-86c0-2ea782ef94da.png)

![image](https://user-images.githubusercontent.com/81230679/130634092-7de481b1-ac26-4f6b-a940-4a3ce8edb669.png)