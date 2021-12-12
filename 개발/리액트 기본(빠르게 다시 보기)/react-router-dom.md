# react-router-dom

### 기본적으로 사용하는 컴포넌트

- BrowserRouter
- Link / NavLink
- Switch
- Route



```jsx
<BrowserRouter>
    <Link to='/'><i className="far fa-comment-dots"></i></Link>
    <Link to='/about'><i className="far fa-question-circle"></i></Link>
    <Link to='/profile'><i className="far fa-user"></i></i></Link>
     
    <Switch>
        <Route path='/' component={Home} />
        <Route path='/about' component={About} />
        <Route path='/profile' component={Profile} />
        <Route component={<div>404</div>} />
    </Switch>
</BrowserRouter>
```

- Route 컴포넌트들을 Switch로 감싸주면, 위에서부터 차례대로 하나씩 매칭 여부를 확인하고 매칭되는 컴포넌트를 렌더링한다.
- 매칭 없이 마지막 Route 컴포넌트에 도달하면 사용자에게 잘못된 경로로 접근했음을 알려준다.



### Route로 설정된 컴포넌트가 받는 props

- history
  - .location, .push(...), .replace(...), goBack(), goForward()
- location
  - .hash, .pathname, .search, .state
- match
  - .isExact, .url, .path, .params