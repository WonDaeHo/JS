# CSS
## CSS는 컨텐츠의 글꼴, 색상, 크기 및 간격을 변경하거나 애니메이션 효과, 레이아웃 구성등에 사용 됨.
## HTML로 구조를 잡고 CSS로 디자인을 입히는 역할.

# CSS 구문

```
  h1 {
 color: red;
 font-size: 15px;
}
div.blue {
 color: blue;
}
span {
 color: yellow;
}
```
h1-선택자로 규칙이 시작됨. 어떤 타겟에 다음의 효과를 적용할지를 명시함.
classname, tag, #id등 여러 규칙이 있음.
속성 값 - color, font-size등 css에서 지원되는 다양한 속성과 그에 매칭되는 값을 지정하여 효과를 적용.

## 상태에 따른 스타일링
각각의 엘리먼트들은 CSS활성자를 통해 상태에 따른 스타일링이 가능함.

## Color
Css에서 각종 생상을 지정할 때 미리 정의된 문자열, RGB, RGBA, HEX코드등으로 색을 지정할 수 있음.
rgb는 각각 red, green, blue의 색상을 0~255사이의 값으로 조합하는 색상표현. - rgb(255,0,255)
rgba는 rgb뒤에 투명도 값을 0~1의 사이의 값으로 지정 가능 -rgba(255,0,255,0)
hex코드는 rgb값을 16진수의 6자리 값으로 표현 - #ff35cc

## 애니메이션
@keyframes
```
span {
 background-color: red;
 animation-name: example;
 animation-duration: 4s;
}
@keyframes example {
 from {background-color: red;}
 to {background-color: yellow;}
}
```
keyframes을 사용하는 예는 위와 같음. animation-name에 @keyframes로 따로 정의한 애니메이션 이름을 지정해주고 몇초에 걸쳐서 적용할 지를 animation-duration에 입력해준다.

duration 속성의 경우 기본값이 0, 애니메이션은 from, to로 적용시킬 엘리먼트의 상태를 어떻게 변화시킬지 css를 이용해 지정. 위에서는 배경색을 지정했지만 폰트 크기, 색상, 위치등 다양하게 적용이 가능.

from, to 대신에 %값을 입력해서 duration의 진행도에 따른 세부 설정도 가능.
span의 위치도 이동시키는 애니메이션을 작성.
```
span {
 background-color: red;
 animation-name: example;
 animation-duration: 4s;
 position: relative;
}
@keyframes example {
 0% {background-color:red; left:0px; top:0px;}
 25% {background-color:yellow; left:200px; top:0px;}
 50% {background-color:blue; left:200px; top:200px;}
 75% {background-color:green; left:0px; top:200px;}
 100% {background-color:red; left:0px; top:0px;}
}
```
위에서 span의 포지션을 relative로 설정했는데 top, left, right, bottom 속성을 사용하기 위해서 설정.
position에 대한 자세한 설명은 아래에서 진행할 예정.

animation-delay 속성은 애니메이션 효과의 발생시간을 조절.
animation-delay: 2s; 애니메이션을 2초 뒤에 시작.
animation-delay: -2s; 애니메이션을 2초 재생된 상태로 시작.
animation-iteration-count 속성은 애니메이션 효과의 발생 횟수를 지정.
animation-iteration-count: 2; 애니메이션을 2번 실행.
animation-iteration-count: infinite; 계속 루프.
animation-direction은 애니매이션 재생 방식을 정방향, 역방향, 반복시 반전 등을 제어함.
normal - 애니메이션이 정상 (정방향)으로 재생. 기본값.
reverse - 애니메이션이 역방향으로 재생.
alternate - 애니메이션이 정방향 재생 후 역방향 재생.
alternate-reverse - 애니메이션이 역방향 재생 후 정방향 재생.
animation-timing-function 속성은 애니메이션의 속도 곡선을 지정.
linear - 동일한 속도의 애니메이션을 지정.
ease - 느리게 시작한 다음 빠르게, 느리게 종료하는 애니메이션을 지정.
ease-in - 느리게 시작되는 애니메이션을 지정.
ease-out - 느리게 끝나는 애니메이션을 지정.
ease-in-out - 시작과 끝이 느린 애니메이션을 지정.
cubic-bezier(n, n, n, n) - 큐빅 베 지어 함수에서 고유 한 값을 정의.

마지막으로 animation-fill-mode는 재생전, 재생후의 element 스타일을 지정함.
none - 애니메이션은 요소가 실행되기 전이나 후에 요소에 스타일을 적용하지 않음
forwards - 요소는 마지막 키 프레임에 의해 설정된 스타일 값을 유지 (animation-direction 및 animation-iteration-count 에 따라 다름)
backwards - 요소는 첫 번째 키 프레임에 의해 설정된 스타일 값 (animation-direction에 따라 다름)을 가져오고 애니메이션 지연 기간 동안이를 유지
both - forwards, backwards 둘 모두 사용.


[호버 되었을 때 버튼 둥둥뜨게하기.](hover1.md)
