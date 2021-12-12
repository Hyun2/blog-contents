# setState, useState의 setter

```jsx
export default function App() {
  const [value, setValue] = useState(0);

  const handleClick = () => {
    // code 1
    // setValue(value + 1);
    // setValue(value + 1);
    // setValue(value + 1);
      
    // code 2
    setValue((prev) => {
      return prev + 1;
    });
    setValue((prev) => {
      return prev + 1;
    });
    setValue((prev) => {
      return prev + 1;
    });
  };

  return (
    <div className="App">
      <h1>{value}</h1>
      <button onClick={handleClick}>클릭</button>
    </div>
  );
}
```

<div style="display:flex; justify-content:space-evenly; background-color:#E4E4E4">
    <div>
        <h3>code 1</h3>
	    <img style="max-width:100%;" src="https://imgur.com/ZVaEFWH.gif" />
    </div>
	<div>
        <h3>code 2</h3>
    	<img style="max-width:100%;" src="https://imgur.com/UddRhzH.gif" />
    </div>
</div>

- setValue를 연속으로 3번 호출해서 버튼을 클릭할 경우 값을 3만큼 올려주는 코드
- **setState(setValue)는 비동기로 호출**됨
  - 이전에 호출한 setState(setValue) 함수가 종료되기 전에 이후의 setState(setValue)들이 실행되기 때문에 원하는 결과를 얻지 못함



## 해결 방법

- **setValue의 파라미터로 값이 아니라 함수를 전달**함으로써 문제 해결
  - 함수를 전달하면 함수의 파라미터로 직전 상태값을 받음

