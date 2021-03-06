---
title: '๐ Redux(2) - ํ์ฉ ์์'
date: 2021-06-18 21:33:13
category: 'study'
thumbnail: 'https://gatsby-blog-images.s3.ap-northeast-2.amazonaws.com/thumb_redux.png'
description: '์ฝ๋๋ฅผ ์ค์ฌ์ผ๋ก ์ดํด๋ณด๋ Redux ํ์ฉ๋ฒ'
tags: ['Redux','react-redux','react-actions']
draft: false
---

*๋ณธ ๊ฒ์๊ธ์ ์ฑ <๋ฆฌ์กํธ๋ฅผ ๋ค๋ฃจ๋ ๊ธฐ์  ๊ฐ์ ํ> 17์ฅ '๋ฆฌ๋์ค๋ฅผ ์ฌ์ฉํ์ฌ ๋ฆฌ์กํธ ์ ํ๋ฆฌ์ผ์ด์ ์ํ ๊ด๋ฆฌํ๊ธฐ'๋ฅผ ์ ๋ฆฌํ ๋ด์ฉ์๋๋ค*

# 1. ํ๋ก์ ํธ ์๊ฐ

๊ฐ๋จํ ํ๋ก์ ํธ๋ฅผ ํตํด์ ๋ฆฌ๋์ค ์ฌ์ฉ๋ฒ์ ์ตํ๋ณด๋๋ก ํ์. ์ซ์๋ฅผ ์ฌ๋ฆฌ๊ณ  ๋ด๋ฆด์ ์๋ ์นด์ดํฐ ๊ธฐ๋ฅ๊ณผ ํ  ์ผ์ ๋ฑ๋กํ๊ณ  ์ฒดํฌํ๊ณ  ์ญ์ ํ  ์ ์๋ TodoList ๊ธฐ๋ฅ์ react + redux ์กฐํฉ์ผ๋ก ๊ตฌํํ ํ๋ก์ ํธ๋ค. ์์ฑ๋ ๋ชจ์ต์ ์๋์ ๊ฐ๋ค.

![](./images/redux/react-redux2.jpeg)

## Presentational & Container ์ปดํฌ๋ํธ
๋ฆฌ์กํธ-๋ฆฌ๋์ค ํ๋ก์ ํธ์์๋ ํ๋ ์  ํ์ด์๋ ์ปดํฌ๋ํธ์ ์ปจํ์ด๋ ์ปดํฌ๋ํธ๋ฅผ ๋ถ๋ฆฌํ๋ ํจํด์ ์ฃผ๋ก ์ฌ์ฉํ๋ค๊ณ  ํ๋ค. `ํ๋ ์  ํ์ด์๋ ์ปดํฌ๋ํธ`๋ ์ฃผ๋ก ์ํ ๊ด๋ฆฌ๊ฐ ์ด๋ฃจ์ด์ง์ง ์๊ณ , ๊ทธ์  props๋ฅผ ๋ฐ์ ์์ ํ๋ฉด์ UI๋ฅผ ๋ณด์ฌ ์ฃผ๊ธฐ๋ง ํ๋ ์ปดํฌ๋ํธ๋ฅผ ๋งํ๋ค. `์ปจํ์ด๋ ์ปดํฌ๋ํธ`๋ ๋ฆฌ๋์ค์ ์ฐ๋๋์ด ๋ฆฌ๋์ค๋ก๋ถํฐ ์ํ๋ฅผ ๋ฐ์์ค๊ธฐ๋ ํ๊ณ  ๋ฆฌ๋์ค ์คํ ์ด์ ์ก์์ ๋์คํจ์นํ๊ธฐ๋ ํ๋ ์ปดํฌ๋ํธ๋ฅผ ๋งํ๋ค.
![์ฌ์ง ์ถ์ฒ : https://the-1.tistory.com/8](./images/redux/react-redux.png)

## Ducks ํจํด
๋ณธ ํ๋ก์ ํธ์ ํ์ผ ๊ตฌ์กฐ๋ ์๋์ ๊ฐ๋ค. `Ducks ํจํด`์ผ๋ก ๋ถ๋ฆฌ๋ ์๋ ํ์ผ ๊ตฌ์กฐ๋, **์ก์ ํ์/์ก์ ์์ฑ์/๋ฆฌ๋์**๋ฅผ `๋ชจ๋`์ ๋ชจ์๋๋ ๋ฐฉ์์ด๋ค.

```directory {4}
src/
|---App.js
|---index.js
|---components/ (Presentational Components)
|   |---Counter.js
|   |---Todos.js
|---containers/ (Container Components)
|   |---CounterContainer.js
|   |---TodosContainer.js
|---modules/
|   |---counter.js
|   |---index.js
|   |---todos.js
```
## ๊ตฌ์กฐ ๋์ํ

๋ณธ ํ๋ก์ ํธ์ ๊ตฌ์กฐ๋ฅผ ๋์์ผ๋ก ํํํด๋ณด์๋ฉด ๋ค์๊ณผ ๊ฐ๋ค.

![](./images/redux/diagram.JPG)


# 2. UI(Presentational ์ปดํฌ๋ํธ) ์ค๋นํ๊ธฐ

CRA๋ก ๋ฆฌ์กํธ ํ๋ก์ ํธ๋ฅผ ์์ฑํ ๋ค `redux`, `react-redux` ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ์ค์นํ๋ค.
```bash
yarn create react-app react-redux-tutorial
cd react-redux-tutorial
yarn add redux react-redux
```

Todos.js ํ์ผ์ `<TodoItem>`, `<Todos>` ๋ ๊ฐ ์ปดํฌ๋ํธ๋ฅผ ์์ฑํ์๋๋ฐ ์ทจํฅ์ ๋ฐ๋ผ ํ์ผ ๋ ๊ฐ๋ก ๋ถ๋ฆฌํด๋ ์ข๋ค. `<Counter>`, `<Todos>` ์ปดํฌ๋ํธ๋ฅผ ์์ฑํ ํ `<App>` ์ปดํฌ๋ํธ์ ๋ ๋๋งํ๋ค. ์ดํ ๋ธ๋ผ์ฐ์ ์ ์ ์ ์ถ๋ ฅ๋ ๊ฒ์ ํ์ธ.

```jsx{3}
// components/Counter.js
import React from 'react';

const Counter = ({ number, onIncrease, onDecrease }) => {
    return (
        <div>
            <h1>{number}</h1>
            <div>
                <button onClick={onIncrease}>+1</button>
                <button onClick={onDecrease}>-1</button>
            </div>
        </div>
    );
};

export default Counter;
```

```jsx
// components/Todos.js
import React from 'react';

const TodoItem = ({ todo, onToggle, onRemove }) => {
    return (
        <div>
            <input type="checkbox"/>
            <span>์์  ํ์คํธ</span>
            <button>์ญ์ </button>
        </div>
    );
};

const Todos = ({
    input,  // ์ธํ์ ์๋ ฅ๋๋ ํ์คํธ
    todos,  // ํ  ์ผ ๋ชฉ๋ก์ด ๋ค์ด์๋ ๊ฐ์ฒด
    onChangeInput,
    onInsert,
    onToggle,
    onRemove,

}) => {
    const onSubmit = e => {
        e.preventDefault();
    };
    return(
        <div>
            <form onSubmit={onSubmit}>
                <input/>
                <button type="submit">๋ฑ๋ก</button>
            </form>
            <div>
                <TodoItem />
                <TodoItem />
                <TodoItem />
                <TodoItem />
                <TodoItem />
            </div>
        </div>
    );
};

export default Todos;
```

```jsx
// App.js
import React from 'react';
import Counter from './components/Counter';
import Todos from './components/Todos';

function App() {
  return (
  <div>
    <Counter number={0} />
    <hr />
    <Todos />
  </div>
  );
}

export default App;

```

# 3. ๋ชจ๋ ๋ง๋ค๊ธฐ

๋ฆฌ๋์ค ๊ด๋ จ ์ฝ๋๋ฅผ ์์ฑํ๋ค. ๋ชจ๋์๋ **์ก์ ํ์/์ก์ ์์ฑ์/๋ฆฌ๋์** ์ฝ๋๋ฅผ ์์ฑํด์ผ ํ๋ค.

## counter ๋ชจ๋ ๋ง๋ค๊ธฐ

### 1) ์ก์ ํ์ ์ ์ํ๊ธฐ

```jsx
// modules/counter.js
const INCREASE = 'counter/INCREASE';
const DECREASE = 'counter/DECREASE';
```
์ก์ ํ์์ ๋๋ฌธ์๋ก ์ ์ํ๊ณ , ์ก์ ํ์ ์ด๋ฆ์ ์ถฉ๋ ๋ฐฉ์ง๋ฅผ ์ํด **'๋ชจ๋ ์ด๋ฆ/์ก์ ์ด๋ฆ'** ํํ๋ก ์ ์ํ๋ค.


### 2) ์ก์ ์์ฑ์ ๋ง๋ค๊ธฐ

```jsx {5,6}
// modules/counter.js
const INCREASE = 'counter/INCREASE';
const DECREASE = 'counter/DECREASE';

export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });
```
์ก์ ์์ฑ์์๋ expxort ํค์๋๋ฅผ ๋ถ์ฌ์ ์ถํ ๋ค๋ฅธ ํ์ผ์์ ๋ถ๋ฌ์ฌ ์ ์๋๋ก ํ๋ค.

### 3) ์ด๊ธฐ ์ํ ๋ฐ ๋ฆฌ๋์ ํจ์ ๋ง๋ค๊ธฐ

```jsx
// modules/counter.js
(...)

const initialState = {
    number: 0
};

function counter(state = initialState, action){
    switch(action.type){
        case INCREASE:
            return {
                number: state.number +1        
            };
        case DECREASE:
            return {
                number: state.number -1
            };
        case default:
            return state;
    }
}

export default counter;
```
๋ฆฌ๋์ ํจ์๋ ํ์ฌ ์ํ๋ฅผ ์ฐธ์กฐํด์ ์ํ๋ฅผ ์๋ฐ์ดํธํ๊ณ  ์๋ก์ด ๊ฐ์ฒด๋ฅผ ์์ฑํด์ ๋ฐํํ๊ฒ๋ ๊ตฌ์ฑํ๋ค.

## todos ๋ชจ๋ ๋ง๋ค๊ธฐ

### 1) ์ก์ ํ์ ์ ์ํ๊ธฐ

```jsx
// modules/todos.js
const CHANGE_INPUT = 'todos/CHANGE_INPUT'; // ์ธํ ๊ฐ์ ๋ณ๊ฒฝ
const INSERT = 'todos/INSERT'; // ์๋ก์ด todo๋ฅผ ๋ฑ๋ก
const TOGGLE = 'todos/TOGGLE'; // todo ์ฒดํฌ๋ฅผ ํ ๊ธ
const REMOVE = 'todos/REMOVE'; // todo๋ฅผ ์ ๊ฑฐ
```


### 2) ์ก์ ์์ฑ์ ๋ง๋ค๊ธฐ

```jsx
// modules/todos.js
(...)
export const changeInput = input => ({
    type: CHANGE_INPUT,
    input
});

let id = 3; // insert๊ฐ ํธ์ถ๋  ๋๋ง๋ค 1์ฉ ๋ํด์ง๋ค
export const insert = text => ({
    type: INSERT,
    todo: {
        id: id++,
        text,
        done: false
    }
});

export const toggle = id => ({
    type: TOGGLE,
    id
});

export const remove = id => ({
    type: REMOVE,
    id
});
```

### 3) ์ด๊ธฐ ์ํ ๋ฐ ๋ฆฌ๋์ ํจ์ ๋ง๋ค๊ธฐ

```jsx
// modules/todos.js
(...)
const initialState = {
    input: '',
    todos:[
        {
            id:1,
            text: '๋ฆฌ๋์ค ๊ธฐ์ด ๋ฐฐ์ฐ๊ธฐ',
            done: true
        },
        {
            id:2,
            text: '๋ฆฌ์กํธ์ ๋ฆฌ๋์ค ์ฌ์ฉํ๊ธฐ',
            done:false
        }
    ]
};

function todos(state = initialState, action){
    switch(action.type){
        case CHANGE_INPUT:
            return{
                ...state,   // ์ํฏ๊ฐ์ ๋ถ๋ณ ๊ฐ์ฒด๋ก ๊ด๋ฆฌํด์ผ ํ๋ฏ๋ก ์์ ํ  ๋๋ง๋ค ์ ๊ฐ ์ฐ์ฐ์(...)๋ฅผ ํ์ฉํด ์๋ก์ด ๊ฐ์ฒด๋ฅผ ์์ฑํ๋ค.
                input: action.input
            };
        case INSERT:
            return{
                ...state,
                todos: state.todos.concat(action.todo)
            };
        case TOGGLE:
            return{
                ...state,
                todos: state.todos.map(todo =>
                    todo.id === action.id ? {...todo, done: !todo.done}: todo
                )
            };
        case REMOVE:
            return {
                ...state,
                todos: state.todos.filter(todo => todo.id !== action.id)
            };
        default:
            return state;   // ์ฒ๋ฆฌํ  ์ก์์ด ์๋ค๋ฉด ์ํฏ๊ฐ์ ๋ณ๊ฒฝํ์ง ์๋๋ค.
    }
}

export default todos;
```

## ๋ฃจํธ ๋ฆฌ๋์ ๋ง๋ค๊ธฐ

```jsx
// modules/index.js
import { combineReducers } from 'redux';
import counter from './counter';
import todos from './todos';

const rootReducer = combineReducers({
    counter, 
    todos,
});

export default rootReducer;
```
`redux`์์ ์ ๊ณตํ๋ `combineReducers` ์ ํธ ํจ์๋ฅผ ์ด์ฉํ์ฌ ๋ฃจํธ ๋ฆฌ๋์๋ฅผ ๋ง๋ ๋ค.


# 4. ๋ฆฌ์กํธ App์ Redux ์ ์ฉํ๊ธฐ

```jsx
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore } from 'redux';
import { Provider } from 'react-redux';
import { composeWithDevTools } from 'redux-devtools-extension';
import './index.css';
import App from './App';
import rootReducer from './modules';

const store = createStore(rootReducer, composeWithDevTools());

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root'),
);
```

`react-redux`์์ ์ ๊ณตํ๋ `<Provider>` ์ปดํฌ๋ํธ๋ก `<App>` ์ปดํฌ๋ํธ๋ฅผ ๊ฐ์ธ์ค๋ค. ์ด๋ store๋ฅผ ํ๋ผ๋ฏธํฐ๋ก ์ ๋ฌํ์ฌ, `<Provider>` ์ปดํฌ๋ํธ๋ก ๊ฐ์ธ์ง ๋ชจ๋  ์ปดํฌ๋ํธ(App ์ปดํฌ๋ํธ)์์ `store`์ ์ ๊ทผํ  ์ ์๊ฒ ๋๋ค. stroe์ ์ ๊ทผํ  ์ ์๋ค๋ ๊ฒ์ ๊ณง ๋ฆฌ๋์ค ์ํ ๊ด๋ฆฌ๊ฐ ๊ฐ๋ฅํ๋ค๋ ๊ฒ๊ณผ ๊ฐ๊ณ , ์ด๋ **๋ฆฌ์กํธ ํ๋ก์ ํธ์ ๋ฆฌ๋์ค๋ฅผ ์ ์ฉ**ํ ๊ฒ์ด๋ค.

> The `<Provider>` component makes the Redux store available to any nested components that need to access the Redux store.

๋ํ `Redux DevTools`๋ฅผ ์ ์ฉํ์ฌ ์น ๋ธ๋ผ์ฐ์ ์์์ ๋๋ฒ๊น(์ํ ๊ด๋ฆฌ)์ ๋ฆฌ๋์ค ๊ฐ๋ฐ์ ๋๊ตฌ๋ฅผ ํ์ฉํ  ์ ์๋ค. `redux-devtools-extension` ํจํค์ง ์ค์น๋ง์ผ๋ก ์ ์ฉํ  ์ ์๋ ๊ฒ์ ์๋๊ณ , ์น ๋ธ๋ผ์ฐ์ ์์์ ํ์ฅ ํ๋ก๊ทธ๋จ ๋ํ ์ค์นํด์ผ ํ๋ค.


# 5. Container Components ๋ง๋ค๊ธฐ


## 1) CounterContainer ๋ง๋ค๊ธฐ

```jsx
// containers/CounterContainer.js
import React from 'react';
import { connect } from 'react-redux';
import Counter from '../components/Counter';
import { increase, decrease } from '../modules/counter';

const CounterContainer = ({ number, increase, decrease }) => {
    return (
        <Counter number={number} onIncrease={increase} onDecrease={decrease} />
    );
};

const mapStateToProps = state => ({
    number: state.counter.number,
});
const mapDispatchToProps = dispatch => ({
    increase: () => {
        dispatch(increase());
    },
    decrease: () => {
        dispatch(decrease());
    },
});
export default connect(
    mapStateToProps,
    mapDispatchToProps,
)(CounterContainer);
```
CounterContainer ์ปดํฌ๋ํธ๋ `react-redux`์์ ์ ๊ณตํ๋ `connect` ํจ์๋ฅผ ์ฌ์ฉํด์ ๋ฆฌ๋์ค์ ์ฐ๋ํ๋ค. ์์ `<Provider>` ์ปดํฌ๋ํธ๋ก ๊ฐ์ธ์ค ์ปดํฌ๋ํธ ์์์ connect ํจ์๋ฅผ ์ฌ์ฉํ์ฌ ์ค์ง์ ์ธ ์ฐ๊ฒฐ์ ํด์ฃผ๋ ๊ตฌ์กฐ์ธ ๊ฒ์ผ๋ก ๋ณด์ธ๋ค. react-redux์์ ์ ๊ณตํ๋ `Hooks`๋ฅผ ์ฌ์ฉํด์ connect ํจ์๋ฅผ ๋์ฒดํ  ์ ์๋ค๊ณ  ํ๋ค. ํ์ง๋ง ๋ณธ ๊ฒ์๊ธ์์๋ connect๋ง ๋ค๋ฃจ๋๋ก ํ๊ฒ ๋ค.

> The connect() function connects a React component to a Redux store.

`connect(mapStateToProps, mapDispatchToProps)(์ฐ๋ํ  ์ปดํฌ๋ํธ)`

`mapStateToProps`๋ ๋ฆฌ๋์ค ์คํ ์ด ์์ **์ํ**๋ฅผ ์ปดํฌ๋ํธ์ props๋ก ๋๊ฒจ์ฃผ๊ธฐ ์ํด ์ค์ ํ๋ ํจ์์ด๊ณ , `mapDispatchToProps`๋ **์ก์ ์์ฑ ํจ์(ํน์ dispatch)**๋ฅผ ์ปดํฌ๋ํธ์ props๋ก ๋๊ฒจ์ฃผ๊ธฐ ์ํด ์ฌ์ฉํ๋ ํจ์๋ค. 

์ฆ, '์ฐ๋ํ  ์ปดํฌ๋ํธ' ๋ถ๋ถ์ `<CounterContainer>` ์ปดํฌ๋ํธ๋ฅผ ๋ฃ์ผ๋ฉด `์คํ ์ด`์ **์ํ**์ **์ก์ ์์ฑ ํจ์**๊ฐ props๋ก CounterContainer ์ปดํฌ๋ํธ์ ์ ํด์ง๋ ๊ฒ์ด๋ค. ์ ์์ ์์  mapStateToProps์์ ๋ฐํํ number, mapDispatchToProps์์ ๋ฐํํ increase()์ decrease()๊ฐ CounterContainer์ props๋ก ์ ํด์ง๋ค.

๋ค๋ฅธ ์ปดํฌ๋ํธ์์ ์ํ ๋ณํ๊ฐ ๋ฐ์ํด์ ์คํ ์ด๊ฐ ์๋ฐ์ดํธ ๋์์ ๊ฒฝ์ฐ, ๋ณธ ์ปดํฌ๋ํธ์์ ์ ์ํ mapStateToProps์ mapDispatchToProps๊ฐ ํธ์ถ๋์ด ์คํ ์ด์ ์ต์  ์ํ๋ฅผ ๊ณต์ ํ๋ ๊ตฌ์กฐ๋ฅผ ๊ฐ๋๋ค. **์ด๋ฅผ ํตํด ํ๋ก์ ํธ ์ ์ญ์์ ์ํ ๊ด๋ฆฌ๊ฐ ๊ฐ๋ฅํ๋ค.** mapStateToProps์ mapDispatchToProps์ ๊ธฐ๋ฅ์ ์ด์  ๊ธ์์ ์๊ฐํ subsribe์ ๊ธฐ๋ฅ์ ๋์ฒด ํน์ ํฌํจํ๋ ๊ฒ์ผ๋ก ๋ณด์ธ๋ค.

> If a mapStateToProps function is specified, the new wrapper component will subscribe to Redux store updates. This means that any time the store is updated, mapStateToProps will be called.

mapStateToProps์ mapDispatchToProps๋ฅผ ์ ์ํ ๋ ํ๋ผ๋ฏธํฐ๋ก ์ ๋ฌํ๋ state, dispatch๋ ๊ฐ๊ฐ ์คํ ์ด๊ฐ ์ง๋ ์ํ์ ์คํ ์ด์ ๋ด์ฅ ํจ์(dispatch)๋ฅผ ์๋ฏธํ๋ค.

์ด์  ์๋์ ๊ฐ์ด CounterContainer ์ปดํฌ๋ํธ๋ฅผ App์ ๋ฃ์ด์ฃผ์.

```jsx {4,9}
// App.js
import React from 'react';
import Todos from './components/Todos';
import CounterContainer from './containers/CounterContainer';

const App = () => {
    return (
        <div>
            <CounterContainer />
            <hr />
            <Todos />
        </div>
    );
};

export default App;
```

์ฌ๊ธฐ๊น์ง ์ฝ๋๋ฅผ ์์ฑํ๋ค๋ฉด ์นด์ดํฐ์ +1, -1๋ฒํผ์ด ์ ์ ์๋ํ๋ ๊ฒ์ ํ์ธํ  ์ ์๋ค. ๋ฒํผ์ ๋๋ ์ ๋์ ์๋ ๊ณผ์ ์ ์ถ๋ก ํด๋ณด์๋ฉด ๋ค์๊ณผ ๊ฐ๋ค. 

1. modules/counter.js์์ ์์ฑํ ์ก์๊ณผ ๋ฆฌ๋์ ํจ์๋ modeuls/index.js์ ๋ฃจํธ ๋ฆฌ๋์์ ๋ด๊ฒจ์ง๋ค. ์ด ๋ฃจํธ ๋ฆฌ๋์๋ ./index.js์์ ์คํ ์ด๋ฅผ ์์ฑํ  ๋ ์คํ ์ด์ ํ๋ผ๋ฏธํฐ๋ก ์ ๋ฌ๋๊ณ , ํด๋น ์คํ ์ด๊ฐ `<Provider>` ์ปดํฌ๋ํธ๋ฅผ ํตํด `<App>` ์ปดํฌ๋ํธ์ ์์์ ์์นํ๊ฒ ๋จ์ผ๋ก์จ(๋ฆฌ์กํธ ํ๋ก์ ํธ์ ๋ฆฌ๋์ค๋ฅผ ์ฐ๋), ๊ทธ๋ฆฌ๊ณ  `connect()`๋จ์ผ๋ก์จ ๋ชจ๋  ์ปดํฌ๋ํธ์์ ์คํ ์ด๋ฅผ ์ฐธ์กฐํ  ์ ์๊ฒ ๋๋ค. 

2. components/Counter.js์ ์์ฑ๋์ด ์๋ +1, -1 button์ ๋๋ฅด๋ฉด ๋ฒํผ์ ํ ๋น๋์ด ์๋ onincrease ํจ์๋ฅผ ํตํด์ ์คํ ์ด์ `dispatch(action)` ๋์์ ์ทจํ๋ค.

3. ์คํ ์ด์ ๋ฆฌ๋์๊ฐ ์ํ ๋ณํ๋ฅผ ์ผ๊ธฐํ์ฌ ์คํ ์ด์ ์ํ๊ฐ ์๋ฐ์ดํธ๋๋ฉด, ์คํ ์ด์ `connect`๋์ด ์๋ `<CounterContainer>`๊ฐ mapStateToProps๋ฅผ ํตํด ์ ๋ฌ๋ฐ์ ์ํ๋ฅผ ๋ค์ modules/counter.js์ ์ ๋ฌํ๊ณ , ๋ฆฌ๋ ๋๋ง ๋์ด ํ๋ฉด์ ์ถ๋ ฅ๋๋ ๊ณผ์ ์ ์๊ฐํด๋ณผ ์ ์๋ค.

### connect ํจ์ ๊ฐ๋ตํํ๊ธฐ

์์์ ์์ฑํ `connect` ํจ์ ์ฝ๋๋ ์๋์ ๊ฐ์ด ๊ฐ๋ตํ๊ฒ ์์ฑํ  ์ ์๋ค. ๊ธฐ์กด์๋ mapStateToProps์ mapDispatchToProps๋ฅผ ๋ฏธ๋ฆฌ ์ ์ธํด ๋๊ณ  ๋ถ๋ฌ๋ค๊ฐ ์ฌ์ฉํ๋ค๋ฉด, ์๋๋ connect ํจ์ ๋ด๋ถ์ **์ต๋ช ํจ์ ํํ**๋ก ์ ์ธํ๋ ๋ฐฉ์์ด๋ค.

```jsx {7,8,9,10,11,12}
// containers/CounterContainer.js
(...)
export default connect(
    state => ({
        number: state.counter.number,
    }),
    distpatch => ({
        increase: () => dispatch(increase()),
        decrease: () => dispatch(decrease()),
    }),
)(CounterContainer);
```
๊ฐ ์ก์ ์์ฑ ํจ์๋ฅผ ํธ์ถํ๊ณ  dispatch๋ก ๊ฐ์ธ๋ ์์์ด ์กฐ๊ธ ๋ฒ๊ฑฐ๋ก์ธ ์ ์๋ค. ํนํ ์ก์ ์์ฑ ํจ์์ ๊ฐ์๊ฐ ๋ง์์ง๋ฉด ๋์ฑ ๋ฒ๊ฑฐ๋กญ๋ค. ๊ทธ๋ด ๋ `redux`์์ ์ ๊ณตํ๋ `bindActionCreators` ์ ํธ ํจ์๋ฅผ ์ฌ์ฉํด์ ๊ฐ๋ตํํ  ์ ์๋ค.

```jsx {8,9,10,11,12,13,14,15,16}
// containers/CounterContainer.js
import { bindActionCreators }  from 'redux';
(...)
export default connect(
    state => ({
        number: state.counter.number,
    }),
    dispatch =>
        bindActionCreators(
            {
                increase,
                decrease,
            },
            dispatch,
        ),
)(CounterContainer);
```

์๋์ ๊ฐ์ด ์์ฑํ๋ฉด connect ํจ์๊ฐ bindActionCreators ์์์ ๋์ ํด์ฃผ๊ธฐ ๋๋ฌธ์ `bindActionCreators`๋ฅผ ์๋ตํ  ์ ์๋ค. `mapDispatchToProps์` ํด๋นํ๋ ํ๋ผ๋ฏธํฐ๋ฅผ ํจ์ ํํ๊ฐ ์๋ **์ก์ ์์ฑ ํจ์๋ก ์ด๋ฃจ์ด์ง ๊ฐ์ฒด** ํํ๋ก ๋ฃ์ด์ฃผ๋ ๋ฐฉ๋ฒ์ด๋ค.

```jsx {5,6,7,8,9}
export default connect(
    state => ({
        number: state.counter.number,
    }),
    {
        increase,
        decrease,
    },
)(CounterContainer);
```

## 2) TodosContainer ๋ง๋ค๊ธฐ

CounterContainer์ ๋์ผํ ๋ฐฉ์์ผ๋ก TodosContainer๋ฅผ ์์ฑํ๋ค. props๊ฐ ์กฐ๊ธ ๋ ๋ง์์ก์ ๋ฟ์ด๋ค.


```jsx
import React from 'react';
import { connect } from 'react-rdux';
import { changeInput, insert, toggle, remove } from '../modules/todos';
import Todos from '../components/Todos';

const TodosContainer = ({
    input,
    todos,
    changeInput,
    insert,
    toggle,
    remove,
}) =>{
    return (
        <Todos
            input={input}
            todos={todos}
            onChangeInput={changeInput}
            onInsert={insert}
            onToggle={toggle}
            onRemove={remove}
        />
    );
};

export default connect(
    // ๋น๊ตฌ์กฐํ ํ ๋น์ ํตํด todos๋ฅผ ๋ถ๋ฆฌํ์ฌ 
    // state.todos.input ๋์  todos.input์ ์ฌ์ฉ
    ({ todos }) => ({
        input: todos.input,
        todos: todos.todos,
    }),
    {
        changeInput,
        insert,
        toggle,
        remove,
    },
)(TodosContainer);
```

๋ง์ฐฌ๊ฐ์ง๋ก `<App>` ์ปดํฌ๋ํธ์์ ๊ธฐ์กด `<Todos>` ์ปดํฌ๋ํธ ์ถ๋ ฅํ๋ ์ฝ๋๋ฅผ `<TodosContainer>` ์ปดํฌ๋ํธ๋ฅผ ์ถ๋ ฅํ๊ฒ๋ ๊ณ ์น๋ค.

```jsx {4,11}
// App.js
import React from 'react';
import CounterContainer from './containers/CounterContainer';
import TodosContainer  from './containers/TodosContainer';

const App = () => {
    return (
        <div>
            <CounterContainer />
            <hr />
            <TodosContainer />
        </div>
    );
};

export default App;
```

๊ธฐ์กด์ ์์๋ก ์์ฑํด๋์๋ `<TodoItem>`, `<Todos>` ์ปดํฌ๋ํธ์ ์ฝ๋๋ฅผ ์์ ํ๋ค.

```jsx {7,8,9,10,12,13,14,15,16,31,32,34,37,38,41,42,43,44,45,46}
// components/Todos.js
import React from 'react';

const TodoItem = ({ todo, onToggle, onRemove }) => {
    return (
        <div>
            <input
                type="checkbox"
                onClick={() => onToggle(todo.id)}
                checked={todo.done}
                readOnly={true}
            />
            <span style={{ textDecoration: todo.done ? 'line-through' : 'none' }}>
                {todo.text}
            </span>
            <button onClick={() => onRemove(todo.id)}>์ญ์ </button>
        </div>
    );
};

const Todos = ({
    input,
    todos,
    onChangeInput,
    onInsert,
    onToggle,
    onRemove,
}) => {
    const onSubmit = e => {
        e.preventDefault();
        onInsert(input);
        onChangeInput(''); // ๋ฑ๋ก ํ ์ธํ ์ด๊ธฐํ
    };
    const onChange = e => onChangeInput(e.target.value);
    return(
        <div>
            <form onSubmit={onSubmit}>
                <input value={input} onChange={onChange} />
                <button type="submit">๋ฑ๋ก</button>
            </form>
            <div>
            {todos.map(todo =>(
                <TodoItem
                    todo={todo}
                    key={todo.id}
                    onToggle={onToggle}
                    onRemove={onRemove}
                />
            ))}
            </div>
        </div>
    );
};

export default Todos;
```

์๋์ ๊ฐ์ด ์์ฑ๋ ๊ฒฐ๊ณผ๋ฅผ ํ์ธํ  ์ ์๋ค.

![](./images/redux/react-redux3.jpeg)

# 6. redux-actions ๋ผ์ด๋ธ๋ฌ๋ฆฌ ํ์ฉํ๊ธฐ
`redux-actions` ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ํ์ฉํ๋ฉด `createAction` ํจ์๋ฅผ ํตํด ์ก์ ์์ฑ ํจ์๋ฅผ ๋ ์งง์ ์ฝ๋๋ก ์์ฑํ  ์ ์๋ค. ๊ทธ๋ฆฌ๊ณ  ๋ฆฌ๋์๋ฅผ ์์ฑํ  ๋๋ switch/case ๋ฌธ์ด ์๋ `handleActions`๋ผ๋ ํจ์๋ฅผ ์ฌ์ฉํ์ฌ ๊ฐ ์ก์๋ง๋ค ์๋ฐ์ดํธ ํจ์๋ฅผ ์ค์ ํ๋ ํ์์ผ๋ก ์์ฑํ  ์ ์๋ค.

```bash
yarn add redux-actions
```

## 1) counter ๋ชจ๋์ ์ ์ฉํ๊ธฐ
```jsx {9,10,11}
// modules/counter.js
const INCREASE = 'counter/INCREASE';
const DECREASE = 'counter/DECREASE';

// ๊ธฐ์กด ์ก์ ์์ฑ์ ์์ฑ ๋ฐฉ์
export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });

// createAction์ ํ์ฉํ ์ก์ ์์ฑ์ ์์ฑ ๋ฐฉ์
export const increase = createAction(INCREASE);
export const decrease = createAction(DECREASE);
```
๊ธฐ์กด์ ์ก์์ type์ ์ผ์ผ์ด ์ง์ ํด ์ฃผ์๋ ๋ฐฉ์์์ `createAction`์ ํ์ฉํ๋ ๋ฐฉ์์ผ๋ก ๋ณ๊ฒฝํ๋ค.

```jsx {17,18,19,20,21,22,23,24,25}
// ๊ธฐ์กด ๋ฆฌ๋์
function counter(state = initialState, action){
    switch(action.type){
        case INCREASE:
            return {
                number: state.number +1        
            };
        case DECREASE:
            return {
                number: state.number -1
            };
        case default:
            return state;
    }
}

// handleActions() ๋ฅผ ์ด์ฉํ ๋ฆฌ๋์
const counter = handleActions(
    {
        [INCREASE]: (state, action) => ({ number: state.number +1 }),
        [DECREASE]: (state, action) => ({ number: state.number -1 }),
    },
    initialState,
);
```
`handleActions` ํจ์๋ฅผ ์ฌ์ฉํด์ ๋ฆฌ๋์ ์์ฑ ๋ฐฉ์์ ๊ฐ์ ํ๋ค. handleACtions ํจ์์ ์ฒซ ๋ฒ์งธ ํ๋ผ๋ฏธํฐ์๋ ๊ฐ ์ก์์ ๋ํ ์๋ฐ์ดํธ ํจ์๋ฅผ ๋ฃ์ด ์ฃผ๊ณ , ๋ ๋ฒ์งธ ํ๋ผ๋ฏธํฐ์๋ ์ด๊ธฐ ์ํ๋ฅผ ๋ฃ์ด์ค๋ค.

## 2) todos ๋ชจ๋์ ์ ์ฉํ๊ธฐ

todos๋ชจ๋๋ counter ๋ชจ๋๊ณผ ๋ง์ฐฌ๊ฐ์ง๋ก `createAction` ํจ์๋ฅผ ์ด์ฉํด์ ์ก์ ์์ฑ์ ์์ฑ ๋ฐฉ์์ ๊ฐ๋ตํํ๋ค. ๊ทธ๋ฐ๋ฐ todos ๋ชจ๋์ ์ก์ ์์ฑ์๋ counter ๋ชจ๋์ ์ก์ ์์ฑ์์๋ ๋ฌ๋ฆฌ ํ๋ผ๋ฏธํฐ๋ฅผ ํ์๋ก ํ๋ค. ์๋ฅผ ๋ค์๋ฉด ์๋์ ๊ฐ๋ค.

```jsx
const MY_ACTION = 'sample/MY_ACTION';
const myAction = createAction(MY_ACTION, text => `${text}!!`);
const action = myAction('hello world');
/*
    ๊ฒฐ๊ณผ:
    { type: MY_ACTION, payload: 'hello world!!'}
*/
```

`createAction` ํจ์์ ์ฒซ ๋ฒ์งธ ํ๋ผ๋ฏธํฐ๋ก ๋ค์ด๊ฐ๋ ๊ฐ์ Action ๊ฐ์ฒด์ type ํ๋กํผํฐ ํค์ ๊ฐ์ด ๋๋ค. ๋ ๋ฒ์งธ ํ๋ผ๋ฏธํฐ๋ก ๋ค์ด๊ฐ๋ ๊ฐ์ `payload` ํ๋กํผํฐ ํค์ ๊ฐ์ด ๋๋ค. ๋ ๋ฒ์งธ ํ๋ผ๋ฏธํฐ์ ๊ฐ ๋์  ํจ์๋ฅผ ์ ์ธํด์ payload ํ๋กํผํฐ ํค์ ์ด๊ธฐํ๋  ๊ฐ์ ๋ณํํด์ค ์ ์๋ค.


```jsx {31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47}
import { createAction } from 'redux-actions';

const CHANGE_INPUT = 'todos/CHANGE_INPUT';
const INSERT = 'todos/INSERT';
const TOGGLE = 'todos/TOGGLE';
const REMOVE = 'todos/REMOVE';

// ๊ธฐ์กด ์ก์ ์์ฑ์ ์์ฑ ๋ฐฉ์
export const changeInput = input => ({
    type: CHANGE_INPUT,
    input
});
let id = 3; // insert๊ฐ ํธ์ถ๋  ๋๋ง๋ค 1์ฉ ๋ํด์ง๋ค
export const insert = text => ({
    type: INSERT,
    todo: {
        id: id++,
        text,
        done: false
    }
});
export const toggle = id => ({
    type: TOGGLE,
    id
});
export const remove = id => ({
    type: REMOVE,
    id
});

// createAction์ ํ์ฉํ ์ก์ ์์ฑ์ ์์ฑ ๋ฐฉ์
export const changeInput = createAction(CHANGE_INPUT, input => input);

let id = 3;
export const insert = createAction(INSERT, text => ({
    id: id++,
    text,
    done: false,
}));

export const toggle = createAction(TOGGLE, id => id);
export const remove = createAction(REMOVE, id => id);
```

์ด์  `handleActions` ํจ์๋ฅผ ์ด์ฉํด์ ๋ฆฌ๋์๋ฅผ ๋ง๋ ๋ค.
`createAction`์ผ๋ก ๋ง๋  ์ก์ ์์ฑ ํจ์๋ ํ๋ผ๋ฏธํฐ๋ก ๋ฐ์ ์จ ๊ฐ์ Action ๊ฐ์ฒด ์์ ๋ฃ์ ๋ action.id, action.todo์ ๊ฐ์ด ์ํ๋ ์ด๋ฆ์ผ๋ก ๊ธฐ์ฌํ  ์ ์๋ ๊ฒ ์๋๋ผ action.payload ๋ผ๋ ๊ณตํต ์ด๋ฆ์ผ๋ก ๋ฃ์ ์ ๋ฐ์ ์๋ค. ๋ฐ๋ผ์ ๋ฆฌ๋์ ํจ์ ๋ด์์ ์กฐํํ  ๋ `action.payload`์ ๊ฐ์ ํํ๋ก ์กฐํํ๋๋ก ๊ตฌํํด์ผ ํ๋ค.




```jsx {46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67}
const initialState = {
    input: '',
    todos:[
        {
            id:1,
            text: '๋ฆฌ๋์ค ๊ธฐ์ด ๋ฐฐ์ฐ๊ธฐ',
            done: true
        },
        {
            id:2,
            text: '๋ฆฌ์กํธ์ ๋ฆฌ๋์ค ์ฌ์ฉํ๊ธฐ',
            done:false
        }
    ]
};

// ๊ธฐ์กด ๋ฆฌ๋์ ์์ฑ ๋ฐฉ์
function todos(state = initialState, action){
    switch(action.type){
        case CHANGE_INPUT:
            return{
                ...state,
                input: action.input
            };
        case INSERT:
            return{
                ...state,
                todos: state.todos.concat(action.todo)
            };
        case TOGGLE:
            return{
                ...state,
                todos: state.todos.map(todo =>
                    todo.id === action.id ? {...todo, done: !todo.done}: todo
                )
            };
        case REMOVE:
            return {
                ...state,
                todos: state.todos.filter(todo => todo.id !== action.id)
            };
        default:
            return state;
    }
}

// handleActions ํจ์๋ฅผ ์ด์ฉํ ๋ฆฌ๋์ ์์ฑ ๋ฐฉ์
const todos = handleActions(
    {
        [CHANGE_INPUT]: (state, action) => ({ ...state, input: action.payload }),
        [INSERT]: (state, action) => ({
            ...state,
            todos: state.todos.concat(action.payload),
        }),
        [TOGGLE]: (state, action) => ({
            ...state,
            todos: state.todos.map(todo =>
                todo.id === action.payload ? {...todo, done: !todo.done } : todo,
            ),
        }),
        [REMOVE]: (state, action) => ({
            ...state,
            todos: state.todos.filter(todo => todo.id !== action.payload),
        }),
    },
    initialState,
);
```

๋ชจ๋  ์ถ๊ฐ ๋ฐ์ดํฐ ๊ฐ์ `action.payload` ํํ๋ก ์กฐํํ๋ ๋ฐฉ์์ ๊ฐ๋์ฑ์ ์ข์ง ์๋ค. `๊ฐ์ฒด ๋น๊ตฌ์กฐํ ํ ๋น` ๋ฌธ๋ฒ์ผ๋ก action ๊ฐ์ payload ์ด๋ฆ์ ์๋ก ์ค์ ํด์ฃผ๋ฉด action.payload๊ฐ ์ด๋ค ๊ฐ์ ์๋ฏธํ๋์ง ๋ ์ฝ๊ฒ ํ์ํ  ์ ์๋ค.

```jsx {3,4,6,8,11,14,16}
const todos = handleActions(
    {
        [CHANGE_INPUT]: (state, { payload: input}) => ({ ...state, input}),
        [INSERT]: (state, {payload: todo}) => ({
            ...state,
            todos: state.todos.concat(todo),
        }),
        [TOGGLE]: (state, {payload:id}) => ({
            ...state,
            todos: state.todos.map(todo =>
                todo.id === id ? {...todo, done: !todo.done } : todo,
            ),
        }),
        [REMOVE]: (state, {payload:id}) => ({
            ...state,
            todos: state.todos.filter(todo => todo.id !== id),
        }),
    },
    initialState,
);
```

# ๊ฒฐ๋ก 
react ํ๋ก์ ํธ์ redux๋ฅผ ์ ์ฉํ์ฌ ์ํ ๊ด๋ฆฌ๋ฅผ ํด๋ณด์๋ค. redux์์ ์ ๊ณตํ๋ `createStore` ํจ์์ ์ฌ์ฉ๋ฒ, react-redux์์ ์ ๊ณตํ๋ `<Provider>` ์ปดํฌ๋ํธ์ `connect` ํจ์์ ์ฌ์ฉ๋ฒ ๋ฑ์ ์์งํ๋๋ก ํ์. redux-actions์์ ์ ๊ณตํ๋ `createAction` ํจ์์ `handleActions` ํจ์๋ ์ฝ๋ ๊ฐ๋์ฑ์ ๋์ด๊ธฐ ์ํด ์ํฉ์ ๋ฐ๋ผ ์์ฉํ๋ฉด ์ข์๊ฒ์ด๋ค.

์ฌ์ค ์ด๋ ๊ฒ ์์ ๊ท๋ชจ์ ํ๋ก์ ํธ์์๋ redux ๋ณธ์ฐ์ ํ์๋ฅผ ์ฒด๊ฐํ  ์ ์๋ค๊ณ  ํ๋ค. ์ข ๋ ํฐ ๊ท๋ชจ์ ํ๋ก์ ํธ์์ ๋ฆฌ๋์ค๋ฅผ ํ์ฉํ์ฌ ์ํ๋ฅผ ๊ด๋ฆฌํด๋ณด๋ฉด ๋ง์ ๊ณต๋ถ๊ฐ ๋  ๊ฒ ๊ฐ๋ค.

[๋ฆฌ๋์ค ์ ์ฐ๊ณ  ๊ณ์๋์?](https://ridicorp.com/story/how-to-use-redux-in-ridi/) ๊ธ์ ์ฐธ๊ณ ํด์ ๋ฆฌ๋์ค ๊ฐ๋ฐ ํธ๋ ๋๋ฅผ ์ ๋ฐ๋ผ๊ฐ๋๋ก ํ์. `redux-toolkit`์ด ์ ๋ง ์ ์ฉํด๋ณด์ธ๋ค. ์ด ๊ธ์ ์ ์ ๋ํ ๋ฒจ๋กํผํธ ๊น๋ฏผ์ค ๋์ด๋ผ๋ ๊ฒ ์กฐ๊ธ ๋๋๋ค.

