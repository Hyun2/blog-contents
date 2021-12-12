## 1. useEffect

```jsx
useEffect(() => {}, []); // mount 시에만 호출, (초기화 시)
useEffect(() => {}, [value]); // value가 업데이트 될 때 호출
```

- 함수형 컴포넌트는 Hook을 이용하여 매 렌더 시마다 함수형 컴포넌트가 호출되더라도 값을 유지할 수 있다.



### 1.1. 관련성 없는 로직은 나눠서 useEffect를 여러 번 호출한다.

```jsx
useEffect(() => {
    console.log('mount, #logic 1');
}, []);

useEffect(() => {
    console.log('mount, #logic 2');
}, []);
```



### 1.2. 리턴(함수)이 필요한 경우

- 리턴 값은 함수

- 리턴 함수는 컴포넌트 언마운트 시 진행할 로직을 담는다.
  - ex) setInterval을 사용했으면 리턴 함수에서 clearInterval을 통해 클리어 해준다.
  - ex) addEventListener을 사용했으면 리턴 함수에서 removeEventListener를 통해 통해 제거 해준다.

```jsx
useEffect(() => {
	const intv = setInterval(() => {}, 1000);
    
    return () => {
        clearInterval(intv);
    }
}, [])
```

```jsx
const [width, setWidth] = useState(window.innerWidth);

useEffect(() => {
	const onResize = () => setWidth(window.innerWidth);
    window.addEventListener('resize', onResize);
    return () => {
        window.removeEventListener('resize', onResize);
    };
}, []);
```



## 2. useCallback

어떤 함수형 컴포넌트 내의 이벤트를 핸들링하는 함수(handleClick, handleChange 등)들도 상태 값의 변화 등으로 그 컴포넌트가 리렌더링되면 계속해서 다시 생성된다. 하지만 이러한 함수들은 로직이 변화하는 것이 아니기 때문에 리렌더링과 상관없이 한 번 생성한 것을 계속해서 사용해도 된다. 리렌더링 될 때마다 새로 생성하면서 발생하는 자원 낭비를 제거하고 성능을 향상시키기 위해 useCallback을 사용한다. useCallback을 사용하면 리렌더링되더라도 한 번 생성한 함수를 계속해서 사용하여 성능을 향상시킬 수 있다.



### 2.1. useCallback 사용 시 주의 할 점

useCallback 내에서 상태 값을 직접 참조하면 해당 상태 값을 디펜던시에 넣어주어야 한다.

```jsx
useCallback(() => {
    setPerson({ ...person, age: 33 });
}, [person])
```

상태 값이 직접 참조하지 않고 이전 상태 값을 가져와 사용하면 디펜던시를 넣어주지 않아도 된다.

```jsx
useCallback(() => {
    setPerson((prevState) => ({ ...prevPerson, age: 33}) );
}, []);
```



## 3. Custom Hook

변화하는 현재의 시간과 브라우저의 가로 사이즈를 가져오는 Custom Hook

![](https://imgur.com/WRs2xSC.gif)



### 3.1. 시간을 가져오는 Hook

```jsx
// useCurrentTime.js

import { useEffect, useState } from "react";

const useCurrentTime = () => {
  const [currentTime, setCurrentTime] = useState("");

  useEffect(() => {
    const handler = setInterval(() => {
      // useEffect 밖에서 선언한 currentTime 상태 값과는 다른 값
      const currentTime = new Date().toISOString().slice(11, 19);
      setCurrentTime(currentTime);
    }, 1000);

    return () => clearInterval(handler);
  }, []);

  return currentTime; // useEffect 안에서 선언한 값이 아니라 밖에서 선언한 currentTime 상태 값
};

export default useCurrentTime;
```





### 3.2. 브라우저의 width를 가져오는 Hook

```jsx
// useCurrentWidth.js

import { useEffect, useState } from "react";

const useCurrentWidth = () => {
  const [currentWidth, setCurrentWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handler = () => setCurrentWidth(window.innerWidth);

    window.addEventListener("resize", handler);

    return () => window.removeEventListener("resize", handler);
  }, []);

  return currentWidth;
};

export default useCurrentWidth;
```



```jsx
// App.js

import useCurrentTime from "./hooks/useCurrentTime";
import useCurrentWidth from "./hooks/useCurrentWidth";
import "./styles.css";

export default function App() {
  const currentTime = useCurrentTime();
  const currentWidth = useCurrentWidth();

  return (
    <div>
      <div>{currentTime}</div>
      <div>{currentWidth}</div>
    </div>
  );
}
```



## 4. Hook 사용 시 주의 할 점

Hook은 항상 컴포넌트의 가장 바깥에서 사용해야하고 호출 순서가 일정해야 한다.

참고) https://ko.reactjs.org/docs/hooks-rules.html

```jsx
const [value1, setValue1] = useState(0);
const [value2, setValue2] = useState(0);
const [value3, setValue3] = useState(0);
```

아래와 같은 코드는 안된다.

```jsx
const [value1, setValue1] = useState(0);
if (value1 > 20) {
	const [value2, setValue2] = useState(0);    
}
const [value3, setValue3] = useState(0);
```

- 조건문 안에서 사용하도록 하면 호출 순서가 항상 일정하게 보장되지 않는다. value2에 해당하는 useState가 호출될 때도 있고 안될 때도 있기 때문에
- 반복문 내에서 사용도 안된다.

```jsx
const App = () => {
    const handleClick = () => {
		const [value1, setValue1] = useState(0);
    }
    
    return (
    	<div>App</div>
    )
}
```

- 함수형 컴포넌트(App)의 가장 바깥에서 사용되어야 한다. 위 코드는 App 컴포넌트의 handleClick 이라는 함수 내에서 사용되었기 때문에 에러가 발생한다.

