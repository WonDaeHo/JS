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
src/Counter/js
```javascript
import React, { Component } from 'react';
//Counter 컴포넌트를 생성하고 Component를 상속
class Counter extends Component {
	render() {
    return (
    <div>
        <h1>Counter</h1>
        <div>값: {this.props.number}</div>
        <button onClick={this.props.handleIncrease}>+</button>
    </div>
    );
	}
}
export default Counter;
```
state에 값을 변경할 때는, this.setState()메서드를 사용한다.

# 라이프사이클(생명주기)

라이프사이클은 리액트에서 컴포넌트가 생성되고 업데이트되고 소멸될 때까지 일련의 과정을 의미한다. 

리액트가 실행되는 특정시점에 사용자가 어떤 기능을 추가하고 싶을 때 LifeCycle API를 사용해서 기능을 추가한다.

마운트 -> 업데이트 -> 언마운트

## 컴포넌트를 생성할 때(마운트)

```javascript
constructor() => componentWillMount() => render() => componentDidMount()
```

constructor(): 생성자 메소드로 컴포넌트가 생성될 때 단 한번만 실행되며, 이 메소드에서만 state를 설정할 수 있다.

componentWillMount(): 리액트 엘리먼트를 실제 DOM노드에 추가하기 직전에 호출되는 메소드.
아직 DOM이 생성되지 않았기 때문에 DOM으로 조작할 수 없고, render가 호출되기 전이므로 setState를 사용해도 render가 호출되지 않는다.

render(): 컴포넌트 렌더링을 담당한다.

componentDidMount(): 컴포넌트가 만들어지고 render가 호출된 이후에 호출되는 메소드다.

## 컴포넌트를 변경할 때(업데이트)
```
componentWillReceiveProps() => shouldComponentUpdate() => componentWillUpdate() 
=> render() => componentDidupdate()
```
componentWillReceiveProps() : 컴포넌트 생성후에 첫 렌더링을 마친 후 호출되는 메서드.
props를 받아서 state를 변경해야 하는 경우 유용하다.

shouldComponentUpdate() : 컴포넌트 변경전에 호출되는 메소드.
props 또는 state가 변경됐을 때, 재 렌더링 여부를 return값으로 결정하게 된다.

componentWillUpdate() : shouldComponentUpdate가 불린 이후에 컴포넌트 변경 전에서 호출되는 메소드. 새로운 props또는 state가 반영되기 직전 변경된 값들을 받는다.

## 컴포넌트를 제거할 때(언마운트)
```
componentWillUnmount()
```
componentWillUnmount() : 컴포넌트가 소멸된 시점에 (DOM에서 삭제된 후)실행되는 메소드, 컴포넌트 내부에서 타이머나 비동기 API를 사용하고 있을 때, 이를 제거하기에 유용하다.

# 조건부 렌더링

APP.js
```javascript
import React from 'react';
import Hello from './Hello';
import Wrapper from './Wrapper';


function App() {
  return (
    <Wrapper>
      <Hello name="react" color="red" isSpecial={true}/>
      <Hello color="pink" />
    </Wrapper>
  )
}

export default App;
```
Hello.js
```javascript
import React from 'react';

function Hello({ color, name, isSpecial }) {
  return (
    <div style={{ color }}>
      { isSpecial ? <b>*</b> : null }
      안녕하세요 {name}
    </div>
  );
}

Hello.defaultProps = {
  name: '이름없음'
}

export default Hello;
```
isSpecial값이 true라면 <b>*</b>를, 그렇지 않다면 null을 보여준다.
JSX에서 null, false, undefind를 렌더링 하게 된다면 아무것도 나타나지 않게 된다.

# useState
Hook기능
App.js
```javascript
import React from 'react';
import Counter from './Counter';

function App() {
  return (
    <Counter />
  );
}

export default App;
```
Counter.js
```javascript
import React, { useState } from 'react';

function Counter() {
  const [number, setNumber] = useState(0);

  const onIncrease = () => {
    setNumber(number + 1);
  }

  const onDecrease = () => {
    setNumber(number - 1);
  }

  return (
    <div>
      <h1>{number}</h1>
      <button onClick={onIncrease}>+1</button>
      <button onClick={onDecrease}>-1</button>
    </div>
  );
}

export default Counter;
```

useState불러오기
```
import React, { useState } from 'react';
```

useState를 사용 할 때에는 상태의 기본값을 파라미터로 넣어서 호출.
이 함수를 호출하면 배열이 반환됨. 첫번째 원소는 현재상태, 두번째 원소는 Setter 함수.

```
const [number, setNumber] = useState(0);
```

# input상태 관리하기

APP.js
```javacript
//리엑트와 InputSample.js와 연결시킨다.
import React from "react";
import InputSample from "./inputSample";
//App함수 안에 함수 InputSample을 호출한다.
function App(){
    return (
      <InputSample/>
    );
}
//다른 곳에서도 사용할 수 있도록 내보내고 마무리
export default App;

```
InputSample.js
```javascrips
//리엑트 라이브러리와 useState 호출
import React, {useState} from "react";

//Input 함수정의
function InputSample(){
    //
    const  [text, setText] = useState('');
    const onchange = (e) =>{
        setText(e.target.value);
    };
    const onreset = () => {
        setText('');
    };
    return(
        <div>
            <input onChange={onchange} value={text} />
            <button onClick={onreset}>reset</button>
            <div>
                <b>값: {text}</b>
            </div>
        </div>
    );
}
export default  InputSample;

```

