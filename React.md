# var
기존의 var는 선언문의 생략, 중복된 변수명 선언, 함수 블록에서의 스코프설정, 함수 호이스팅 등으로
개발의 혼란을 야기함. 가독성이 떨어지기 때문에 ES6에서는 var를 지양함.
```javascript
var a = 100;
if(a > 0){
    var a = 200;
    console.log(a); // 200출력
}
console.log(a); // 200 출력
```

# let
재할당 가능
```javascript
let a = 100;
if(a > 0){
    let a = 200;
    console.log(a); // 200출력
}
console.log(a); // 100 출력
```

# const
변경할 수 없는 변수, 값을 재할당할 필요가 없을 경우 사용함. 변수와 달리 선언 시에 반드시 초기값을
할당해줘야하며, 스코프 범위는 let와 동일하게 블록레벨로 처리.
```javascript
const MY_NAME; // 초기값 할당하지 않아 SyntaxError발생
const MY_NAME = 'Kim';
MY_NAME = 'Lee'; // 값을 변경하려 하면 TypeError발생
```

# 화살표 함수
단일인자만 넘겨받을 경우 {}생략가능
```javascript
// 기존 함수 구문
var add = function(a, b){
    return a + b;
};
// 화살표 함수 구문
let add = (a, b) => {
    return a + b;
}
```
# 펼침연산자
...을 사용하는 연산자로 배열 또는 객체의 모든 값을 복사할수 있습니다.
```javascript
let a = [1,2,3]
let b = [...a, 4]
console.log(a) //[1,2,3]
console.log(b) //[1,2,3,4]
```
# 클래스
class를 사용하여 Prototype을 사용하지 않고도 간단하게 상속을 사용할 수 있게 됨.
```javascript
// class키워드 뒤에 클래스명 붙여 선언하고 블록 안쪽에 구문 작성
class Display {
}
const display = new Display();
```
# 상속
부모클래스의 속성이나 기능을 자식에게 전달한다
```javascript
class Display{
    constructor(x, y){
        this.x = x;
        this.y = y;
    }
}
/* 
선언된 Display 클래스를 React클래스 선언문 뒤에 Extends를 붙여 
Display클래스를 상속 받고 있습니다.
*/
class React extends Display{
    constructor(x, y, width, height){
/*
부모클래스의 생성자로 자식클래스에서 생성자 호출 시 부모 클래스가 초기화 되도록 강제적으로 super를 호출됩니다.
*/
        super(x, y);  
        this.width = width;
        this.height = height;
    }
}
```
# JSX문법
## 1. 닫혀야하는 태그
```javascript
import React, { Component } from 'react';
class App extends Component {
 render() {
   return (
    <div>
      <p>Hello world // 에러발생
      <p>Hello world</p> // 정상작동
      <input type='text' > 에러발생
      <input type='text' /> // 정상작동
    </div>
   );
 }
}
export default App;
```
## 2. 감싸져있는 엘리먼트
두개 이상의 엘리먼트는 무조건 하나의 엘리먼트로 감싸져 있어야합니다. 
```javascript
import React, { Component } from ‘react’;
class App extends Component {
  render() {
    return (
      <div>
        <p> Hello world</p>
        <p> Have a Nice day :)</p>
      <div>
    );
  }
}
export default App;
```
```javascript
// 리액트를 불러올 때 Component와 함께 Fragment도 불러와야 함
import React, { Component, Fragment } from ‘react’;
class App extends Component {
  render() {
    return (
      <Fragment>
        <p> Hello world</p>
        <p> Have a Nice day :)</p>
      </Fragment>
    );
  }
}
export default App;
```
# 3. JSX안에 javascript사용
```javascript
import React, { Component } from 'react';
class App extends Component {
  render() {
    let name = 'react';
    return (
      <div>
        <p>Hi {name}!</p>
      </div>
    );
  }
}
export default App;
```
# 4. CSS작성(InlineStyle)
font-size와 같이 중간에 대시 기호(-)가 들어간 속성명은 fontSize와 같이 카멜케이스로 바꿔줘야합니다.
```javascript
import React, { Component } from 'react';
class App extends Component {
    render() {
	    const styles = {
	        backgroundColor: 'black',
	        padding: '16px',
	        color: 'white',
	        fontSize: '12px'
	    };
	
	    return (
	        <div style={styles}>
	        Hello World
	        </div>
	    );
    }
}
export default App;
```
# 5. CSS작성: 외부파일 불러오기

```javascript
import './App.css'
```

# 6. html에서의 class는 className
react DOM은 HTML어트리뷰트 이름 대신 캐멀케이스(camelCase)를 네이밍 컨벤션으로 사용
JSX에서 tabindex는 tabIndex로 작성한다.
class어트리뷰트는 JavaScript의 예약어 이므로 className으로 작성한다.
```javscript
return (
  <div className="App">
    Hello World
  </div>
);
```
# 7. Props와 State
## props
부모 컴포넌트로부터 자식컴포넌트가 물려받는 속성, 다시말해 props를 이용해서 부모 컴포넌트가 자식 컴포논트에 데이터를 전달.

부모에게서 받아온 props는 자식 컴포넌트에 데이터를 전달합니다.

부모에게서 받아온 props는 자식에서 수정이 불가능하다. 이유는 단방향 데이터 흐름으로 강제로 정해져 있다.

src/App.js
App.js에서 자식 컴포넌트인 Hello 컴포넌트에게 보낼 속성을 정의.
```javascript
import React, { Component } from 'react';
import Hello from './hello.js';
class App extends Component {
    render() {
        return (
            <div className="App">
                <Hello text="React"/> //정의
            </div>
        );
    }
}
export default App;
```
src/hello.js
hello.js에서 부모컴포넌트에서 받은 props를 호출합니다.
```javascript
class Hello extends Component {
    render() {
        return (
        <div>
            <p>
              Hello {this.props.text} //정의 된 props호출
            </p>
        </div>
        );
    }
}
```
## State
State는 컴포넌트 내에서 동적으로 변동되는 데이터를 관리하며, 언제나 기본 값을 미리 설정해야 사용할 수 있다.

버튼클릭으로 숫자가 증가하는 예제

```javascript
import React, { Component } from 'react';
import './App.css';
import Counter from './Counter.js'// 카운터를 표시해줄 컴포넌트 호출
class App extends Component {
    constructor(props) {
        super(props);
        this.state = {
            number: 0
        };
    }
//함수 실행시 number값이 1 증가
    handleIncrease = () => {
      const { number } = this.state;
//this.setState는 state의 값을 변경할 때 사용 하는 함수
      this.setState({
        number: number + 1
      });
    }
render() {
        return (
          <div className="App">
            <header className="App-header">
                <img src={logo} className="App-logo" alt="logo" />
                <Counter 
                   handleIncrease={this.handleIncrease}
                   number={this.state.number}
                />
            </header>
          </div>
        );
    }
}
export default App;
```




