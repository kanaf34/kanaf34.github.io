#### Module Pattern

- Javascript 란 언어에서는 원래 모듈화 하는 개념이 없었음.
  ( 모든 작업은 전체공간에서 함께, 한꺼번에! )
- 코드가 복잡해질수록, 사용자가 많아질수록, 클라이언트단의 역할이 점점 커질수록,  
  분리하고자 하는 욕구가 자연스럽게 나타남
- '결합도를 낮추고, 응집도를 높이자!' (예전에 학교에서 많이 보던 문구..)


#### 모듈화를 하려는 방법 

| 종류     | 주사용처 | 방식                                   |
|----------|----------|----------------------------------------|
| CommonJS | Server   | 선언 : module.export / 사용 : require  |
| AMD      | Browser  | 선언 : define / 사용 : require, define |

- 참고 : [NAVER D2 - Javascript 표준을 위한 움직임:CommonJS와 AMD](http://d2.naver.com/helloworld/12864)

>	필요한 파일이 모두 로컬 디스크에 있어 바로 불러 쓸 수 있는 상황,   
>	즉 서버사이드에서는 CommonJS 명세가 AMD 방식보다 간결하다.   
>	반면 필요한 파일을 네트워크를 통해 내려받아야 하는 브라우저와 같은 환경에서는   
>	AMD가 CommonJS보다 더 유연한 방법을 제공한다.  

*[AMD]: Asyncronouse Module Definition

#### Next?

Module Bunddler (browserify, webpack 등등)
: [Browserify와 Webpack](http://blog.coderifleman.com/post/112564054684/browserify%EC%99%80-webpack)

ECMAScript 6
: [ECMA6 Features](https://github.com/lukehoban/es6features/blob/master/README.md)

