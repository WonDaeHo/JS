# TIL
오늘 내가 배운 것들(javascript)
---------------------------------------
- 연산자
- [삼항조건연산자](삼항조건연산자.md)
- [조건문](조건문.md)
- [객체](객체.md)
- [함수](함수.md)
---------------------------------------

<details markdown="1">
# <summary>배열 내장함수</summary>

forEach
```javascript
const a =[1,2,3,4,5];
for(let i = 0; i < a.length; i++){
  console.log(a[i];)
}

const a = [1,2,3,4,5];
a.forEach( (n) =>{
console.log(n);
});
```

map
```javascript
const array [1,2,3,4,5];
const newArray[];
for(let i = 0; i < array.length; i++){
  newArray.push(array[i]);
};
console.log(newArray);
--------------------------------------
const array = [1,2,3,4,5];

const newArray = (n) => n;
const a = array.map(newArray);
console.log(a);
```

</details>

-------------------------------------------------

<details markdown="1">
# <summary>ES6</summary>

super의 사용이유?
상속을 받게 됐을 때 자식 클래스에서 새로운 요소를 추가할때 super를 사용하면 코드의 재사용을 줄일 수 있다.
super의 2가지 용법
1. super() : 부모클래스의 생성자가 호출이 된다.
2. super. : 부모클래스 자체를 뜻함.

괄호가 있는 super -> 부모의 생성자에 접근한다.
괄호가 없는 super -> 부모의 메소드에 접근한다. ex) 함수 등

만약, super가 없다면?
-> 부모의 인자를 상속받은 자식 객체에서 무언가 새로운 함수 등을 추가하고 싶을 때, 일일이 부모 인자의 메소드 값을 동일하게 입력한 뒤 새로운 값을 입력해주어야 한다. 이것은 매우 지저분한 코드를 만들게 함은 물론, 코드의 반복이 자주 일어나므로 좋지 않은 코드라고 할 수 있다. 따라서 super를 이용해주면 아주 손쉽게 부모의 메소드 혹은 객체를 이어받되, 새로운 값을 추가해줄 수 있는 것이다.
  
</details>

--------------------------------------------------

<details markdown="1">
# <summary>삼항연산자</summary>

사용법
```
조건 ? true : false
```

ex)
```javascript
const array = [1,2,3];
let text = array.length === 0 
  ? '배열이 비어있습니다' 
  : '배열이 비어있지 않습니다.';
console.log(text);
```

</details>

------------------------------------------------------

<details markdown="1">
# <summary>파라미터</summary>

const calculateCircleArea = (r = 1) => Math.PI * r * r;

const area = calculateCircleArea();
console.log(area); // 3.141592653589793

</details>

-------------------------------------------------------

<details markdown="1">
# <summary>조건문 스킬</summary>

- 특정 값이 여러 값중 하나인지 확인해야할 때
  
  ```javascript
  const a = name => [1, 2, 3, 4].includes(name);

  console.log(isAnimal(1)); // true
  console.log(isAnimal(6)); // false
  ```
  
- 값에 따라 다른 결과물을 반환 해야 할 때
  
  ```javascript
  
  ```

</details>

-------------------------------------------------------

<details markdown="1">
# <summary>Scope</summary>

- GlobalScope:
  코드의 모든 범위에서 사용가능.
- FunctionScope:
  함수 안에서만 사용가능.
- BlockScope:
  if, for, switch등 특정 블록 내부에서만 사용이 가능.

</details>


 
