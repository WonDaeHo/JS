```
class HelloWorld extends React.Component {
 render() {
 return (<h1>Hello World! </h1>);
 }
}
ReactDOM.render( < HelloWorld / > , document.querySelector("#app"))
```
React를 이용하여 컴포넌트를 생성할 때 React.Component를 상속받아 생성한다.
ReactDom.render는 생성한 컴포넌트를 html에 그리기 위해 id가 app인 element를 찾아 HellowWorld 컴포넌트를 그리는 명령인데
보통 react로 이루어진 앱은 SPA(Single Page Application)로 구성되어 이용한 UI(사용자가 제품/서비스를 사용할 때, 마주하게 되는 면)코드가 작성이 된다.
jsx는 javascript의 확장문법으로서 javascript를 html에 넣지 않고도 js파일안에 html 구문까지 작성할 수 있도록 해줌.
```
class HelloWorld extends React.Component {
 render() {
 const data = 1; //const type으로 변수를 선언함. 현재 var를 거의 사용하지않는다. let과 const를 주로 사용.
 const loopCount = ["A","B","C","D","E"];
 return (
 <div>
 <h1>Hello World! {data}, {data+1} </h1> 
 {loopCount.map(entry => <p>{entry}</p>)}
 </div> 
 );
 }
}
```
