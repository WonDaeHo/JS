# 스크롤바 기능 살리고 안보이게 하기.
-----------------------------------------------------
```css
.target {
    -ms-overflow-style: none; /* IE and Edge */
    scrollbar-width: none; /* Firefox */
}
.target::-webkit-scrollbar {
    display: none; /* Chrome, Safari, Opera*/
}
```
# target흔들기
----------------------------------------------------
```css

 .target{
 transform-origin: 50% 0%;
 animation-name: shake;
 animation-duration: 2s;
 animation-iteration-count: infinite;
 animation-delay: 0.5s;
 }

 @keyframes shake{
 0%{
 transform: rotate(0deg);
 }
 10%{
 transform: rotate(45deg);
 }
 20%{
 transform: rotate(-45deg);
 }
 30%{
 transform: rotate(30deg);
 }
 40%{
 transform: rotate(-30deg);
 }
 50%{
 transform: rotate(10deg);
 }
 60%{
 transform: rotate(-10deg);
 }
 70%{ 
 transform: rotate(0deg);
 }
 100%{ 
 transform: rotate(0deg);
 }
 }
```
