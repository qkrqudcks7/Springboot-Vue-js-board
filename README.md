# Spring Boot + Vue.js 연습 (게시판)


## rest api , spring security jwt , vue.js , vuex 위주로 작성 
<hr/>

## :pushpin: 1. Spring security JWT(Json Web Token) 
토큰 기반 인증 시스템에서 주로 사용하는 JWT를 SpringSecurity 기반으로 로그인 처리를 했습니다.

### :question: 왜 토큰 기반인가?

#### [ 토큰 기반 인증 시스템 ]
인증받은 사용자들에게 토큰을 발급하고, 서버에 요청을 할 때 헤더에 토큰을 함께 보내도록 하여 유효성 검사를 한다.
이런 시스템은 더이상 사용자의 인증 정보를 서버에 유지하지 않고 클라이언트 측에서 들어오는 요청만으로 작업을 처리한다.
따라서 사용자의 로그인 여부에 신경쓰지 않고 쉽게 시스템을 확장할 수 있다.

![](https://blog.kakaocdn.net/dn/ogoAg/btqAriyT5sY/YYt2wkEz50kKN47mLwRDXK/img.png)

1. 사용자가 아이디 , 비밀번호로 로그인
2. 서버에서 해당 정보를 확인
3. 해당 정보가 있다면, 사용자에게 토큰을 발급
4. 클레이언트에서 전달받은 토큰을 저장하고, 서버에 요청을 할 때마다 해당 토큰을 서버에 함께 전달
5. 서버는 토큰을 검증하고, 요청에 응답


<hr/>

### :question: JWT란?
Json Web Token은 JSON 객체로 정보를 전송하는 웹표준이다.
JWT는 <b>'.'</b>을 구분자로 세 부분으로 구분된 문자열이다.

![](https://blog.kakaocdn.net/dn/cmtrRL/btqAZO41bpf/6bLetr0rhyyjENyROBfAO1/img.png)

1. header : 토큰 타입, 해싱 알고리즘을 저장
2. payload : 실제 전달한 데이터
3. signature : 위변조를 방지하기 위한 값

JWT는 다른 토큰들과 달리 토큰자체에 데이터를 가지고 있다. 때문에 사용자 인증 요청시 필요한 정보를 전달하는 객체로 사용할 수 있다.

<hr/>

### :point_right: Spring Boot JWT 적용

#### 1. SecurityConfig 작성
Spring Security를 이용하기 위해서 WebSecurityConfigurerAdapter를 상속받은 클래스에 @EnableWebSecurity를 넣음으로서 Spring Security Filter Chain 을 사용한다는 것을 명시
![image](https://user-images.githubusercontent.com/66015002/116978941-581f0d00-acff-11eb-926a-3ec3a148a61c.png)

#### 2. JWT 생성, 검증 컴포넌트 작성

![image](https://user-images.githubusercontent.com/66015002/116979883-80f3d200-ad00-11eb-9478-c33398b4822b.png)

JWT를 생성 및 검증하는 컴포넌트를 생성했다. UserDetailsImpl에서 로그인 된 정보를 토큰에 저장하여 생성한다.

#### 3. 실제 인증 작업을 하는 필터 생성
![image](https://user-images.githubusercontent.com/66015002/116980838-b3ea9580-ad01-11eb-8b6a-1e5a9b059fa2.png)

검증이 끝난 JWT로부터 유저 정보를 받아옵니다.

#### 4. 토큰에 저장할 유저정보(UserDetailsService)
![image](https://user-images.githubusercontent.com/66015002/116981070-01ff9900-ad02-11eb-8190-93fc159b1a07.png)
Username을 아이디로 지정하여 찾아온다.

#### 5.SecurityConfig 추가 작성
![image](https://user-images.githubusercontent.com/66015002/116982248-86065080-ad03-11eb-8362-50d08ad50ff1.png)

#### :question: CORS란?
Cross Origin Resource Sharing. 다른 출처의 자원을 공유할 수 있도록 설정하는 권한 체제이다.
1. registry.addMapping으로 CORS에 적용할 URL 패턴을 정의한다.
2. registry.allowdOrigins로 자원 공유를 허락할 Origin을 지정한다.
3. registry.allowedMethods로 허용할 HTTP method를 지정한다. 

#### 6. 로그인
![image](https://user-images.githubusercontent.com/66015002/116983663-56f0de80-ad05-11eb-8a85-aae09feff6c7.png)
