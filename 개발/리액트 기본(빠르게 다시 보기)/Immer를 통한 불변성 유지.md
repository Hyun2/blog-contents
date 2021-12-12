#  Immer를 통한 불변성 유지



### immer 없이 배열을 다루는 것은 까다롭다.

immer는 리액트에서 자바스크립트의 오브젝트나 배열 형태의 기존 상태 값을 변경하지 않고 새로운 상태 값을 setter에게 넘겨주는 코드를 작성할 때 유용하다. 오브젝트는 immer 패키지 없이 자바스크립트의 기본 함수들을 이용할 수도 있겠으나 배열을 다루는 경우 기본 함수만으로 다루기에 까다로운 부분이 존재한다.



```javascript
// immer 없이 오브젝트의 불변성을 유지하는 경우: 할만 하다.

const todo = {
	'title': 'Try immer',
    'done': false
}

const updatedTodo = {
	...todo,
    'done': true
}
```



```javascript
// immer 없이 배열의 불변성을 유지하는 경우: 까다롭다.

// 바나나, 오렌지를 제거하고 딸기 끼워넣기
const fruits = ['사과', '바나나', '오렌지', '키위'];

const newFruits = [
    ...fruits.slice(0, 1),
    '딸기',
    ...fruits(3)
]
```



```javascript
// immer로 배열의 불변성을 유지하는 경우: 간결하다.

const newFruits = [
    ...produce(fruits, draft => {
        draft.splice(1, 2, '딸기');
    })
]
```



### 왜 기존의 상태 값을 유지해야 하는가?

기존의 상태 값은 유지한 채 새로운 상태 값을 세팅해 주어야 리액트가 변화를 인지하고 브라우저를 다시 그린다.



### React에서 활용하기

![](https://imgur.com/caWgagM.gif)

```jsx
const init = [
  { todo: "Learn ES6+", done: true },
  { todo: "Try immer", done: false }
];

const [todos, setTodo] = useState(init);

const handleClick = () => {
  setTodo(
    produce(todos, (draft) => {
      draft.find((item) => item.todo === "Try immer").done = true;
      draft.push({
        todo: "Tweet about it",
        done: false
      });
    })
  );
};
```

- produce는 draft를 리턴한다.



```jsx
// setTodo 파라미터를 함수로 전달: 연속된 setState 호출로 오동작하는 경우

const handleClick = () => {
  setTodo((prev) =>
    produce(prev, (draft) => {
      draft.find((item) => item.todo === "Try immer").done = true;
      draft.push({
        todo: "Tweet about it",
        done: false
      });
    })
  );
};
```

