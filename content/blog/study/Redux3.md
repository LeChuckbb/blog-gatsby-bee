---
title: '๐ Redux(3) - ๋ฏธ๋ค์จ์ด redux-thunk'
date: 2021-06-20 21:33:13
category: 'study'
thumbnail: 'https://gatsby-blog-images.s3.ap-northeast-2.amazonaws.com/thumb_redux.png'
description: '๋ฆฌ๋์ค ๋ฏธ๋ค์จ์ด๋ ๋ฌด์์ธ์ง ์์๋ณด๊ณ  redux-thunk๋ฅผ ์ฌ์ฉํด๋ณด์'
tags: ['Redux','middleware','redux-thunk']
draft: false
---

*๋ณธ ๊ฒ์๊ธ์ ์ฑ <๋ฆฌ์กํธ๋ฅผ ๋ค๋ฃจ๋ ๊ธฐ์  ๊ฐ์ ํ> 18์ฅ '๋ฆฌ๋์ค ๋ฏธ๋ค์จ์ด๋ฅผ ํตํ ๋น๋๊ธฐ ์์ ๊ด๋ฆฌ'๋ฅผ ์ ๋ฆฌํ ๋ด์ฉ์๋๋ค*

# 1. ๋ฏธ๋ค์จ์ด๋?

๋ฏธ๋ค์จ์ด๋ ๋ฆฌ๋์๊ฐ ์ก์์ ์ฒ๋ฆฌํ๊ธฐ ์ ์ ์คํ๋๋ ํจ์๋ค. ๋ฆฌ๋์ค ์ฌ์ฉ ์ ํน๋ณํ ๋ฏธ๋ค์จ์ด๋ฅผ ์ค์ ํ์ง ์์๋ค๋ฉด dispatch๋ ์ก์์ ๊ณง๋ฐ๋ก ๋ฆฌ๋์๋ก ๋ณด๋ด์ง๋ค. ํ์ง๋ง ๋ฏธ๋ค์จ์ด๋ฅผ ์ค์ ํ ๊ฒฝ์ฐ ๋ฏธ๋ค์จ์ด๋ ์ก์๊ณผ ๋ฆฌ๋์ ์ฌ์ด์์ ์ค๊ฐ์ ์ญํ ์ ํ๋ค. ๋ฏธ๋ค์จ์ด๋ ์ํฏ๊ฐ ๋ณ๊ฒฝ ์ ๋ก๊ทธ๋ฅผ ์ถ๋ ฅํ๋(redux-logger) ๋ฑ ๋๋ฒ๊น ๋ชฉ์ ์ผ๋ก ํ์ฉ๋๊ฑฐ๋, ๋น๋๊ธฐ ์์์ ์ฒ๋ฆฌํ๊ธฐ ์ํด ์ฌ์ฉ๋๋ค.

![](./images/redux/redux-middle.png)

๋ฏธ๋ค์จ์ด์ ๊ธฐ๋ณธ ๊ตฌ์กฐ๋ ๋ค์๊ณผ ๊ฐ๋ค.

```jsx
// ํ์ดํ ํจ์ ์์ฑ์
const loggerMiddleware = store => next => action =>{

}

// ์์ ๋์ผํ ํจ์ ๊ตฌ์กฐ
const loggerMiddleware = function loggerMiddleware(store){
    return function(next){
        return function(action){
            // ๋ฏธ๋ค์จ์ด ๊ธฐ๋ณธ ๊ตฌ์กฐ
        }
    }
}
```

๋ฏธ๋ค์จ์ด๋ ๊ฒฐ๊ตญ **ํจ์๋ฅผ ๋ฐํํ๋ ํจ์๋ฅผ ๋ฐํํ๋ ํจ์**๋ค.
ํ๋ผ๋ฏธํฐ๋ก ๋ฐ์์จ `store`๋ ๋ฆฌ๋์ค ์คํ ์ด ์ธ์คํด์ค๋ฅผ, `action`์ ๋์คํจ์น๋ ์ก์์ ๊ฐ๋ฆฌํจ๋ค.
`next` ํ๋ผ๋ฏธํฐ๋ dispatch์ ๋น์ทํ ์ญํ ์ ํจ์์ธ๋ฐ, `next(action)`์ ํธ์ถํ๋ฉด ๋ค์์ผ๋ก ์ฒ๋ฆฌํด์ผ ํ  ๋ฏธ๋ค์จ์ด์๊ฒ ์ก์์ ๋๊ฒจ์ค๋ค. ๋ง์ฝ ๊ทธ๋ค์ ๋ฏธ๋ค์จ์ด๊ฐ ์กด์ฌํ์ง ์์ผ๋ฉด ์ต์ข์ ์ผ๋ก ๋ฆฌ๋์์๊ฒ ์ก์์ ๋๊ฒจ์ค๋ค.

๋ฆฌ๋์ค์ ๋ฏธ๋ค์จ์ด๋ฅผ ์ ์ฉํ๋ ์์๋ฅผ ์ดํด๋ณด์.
```jsx
import { createStore, applayMiddleware } from 'redux';

// ๋ฏธ๋ค์จ์ด1 ์ ์
const middleware1 = store => next => action => { // 1
    console.log('middleware1 start');
    const result = next(action);
    console.log('middleware1 end');
    return result;
};
// ๋ฏธ๋ค์จ์ด2 ์ ์
const middleware2 = store => next => action => {
    console.log('middleware2 start');
    const result = next(action);
    console.log('middleware2 end');
    return result;
};
// ์๋ฌด์ผ๋ ํ์ง ์๋ ๋ฆฌ๋์ ์ ์
const myReducer = (state, action) => {
    console.log('myReducer)';
    return state;
};

const store = createStore(myReducer, applyMiddleware(middleware1, middleware2));
store.dispatch({ type: 'someAction' });
```

์คํ ์ด๋ฅผ ์์ฑํ  ๋ `applyMiddleware` ํจ์์ ํ๋ผ๋ฏธํฐ๋ก ์์ ์ ์ํ ๋ฏธ๋ค์จ์ด๋ค์ ์ ๋ฌํจ์ผ๋ก์จ ๋ฆฌ๋์ค์ ๋ฏธ๋ค์จ์ด๋ฅผ ์ ์ฉํ  ์ ์๋ค. `dispatch(action)` ๋ช๋ น์ด๋ฅผ ํตํ ์คํ ๊ฒฐ๊ณผ๋ ์๋์ ๊ฐ๋ค.

```console
middleware1 start
middleware2 start
myReducer
middleware2 end
middlware1 end
```

์ก์๊ณผ ์คํ ์ด ์ค๊ฐ์ ์์นํ ๋ฏธ๋ค์จ์ด๊ฐ dispatch๋ ์ก์์ ์บ์นํ์๋ค. middleware1์ `next` ํจ์๋ middleware2๋ฅผ ํธ์ถํ๊ณ , middleware2์ next๋ ๊ทธ ๋ค์ ํธ์ถํ  ๋ฏธ๋ค์จ์ด๊ฐ ์กด์ฌํ์ง ์์ผ๋ฏ๋ก ๋ฆฌ๋์๋ฅผ ํธ์ถํ๊ฒ ๋๋ค. **์ด์ฒ๋ผ ๋ฏธ๋ค์จ์ด๋ ๋ฆฌ๋์ ํธ์ถ ์ ํ์ ํ์ํ ์์์ ์ ์ํ  ์ ์๋ค.** ์ก์ ์ ๋ณด๋ฅผ ๊ฐ๋ก์ฑ์ ๋ณ๊ฒฝํ ํ ๋ฆฌ๋์์๊ฒ ์ ๋ฌํ๊ฑฐ๋, ํน์  ์กฐ๊ฑด์ ์ก์์ ๋ฌด์ํ๊ฒ ํ  ์๋ ์๋ค.

๋ฏธ๋ค์จ์ด๋ ์์ ๊ฐ์ด ์ง์  ์ ์ํด์ ์ฌ์ฉํ๋ ๋ฐฉ๋ฒ๊ณผ ๋๋ฆฌ ์ฐ์ด๋ ๋ฏธ๋ค์จ์ด ํจํค์ง๋ฅผ ๊ฐ์ ธ์ ์ฌ์ฉํ๋ ๋ฐฉ๋ฒ์ด ์๋ค. redux-thunk์ redux-saga๋ ๋ฏธ๋ค์จ์ด ํจํค์ง ์ค ๊ฐ์ฅ ๋ง์ด ์ฌ์ฉ๋๋ ํจํค์ง์ฌ์ ๊ณต๋ถํด๋ ํ์๊ฐ ์๋ค.

| ํจํค์ง๋ช | ์ ํ ๊ธฐ์ค | ํน์ง |
| --- | --- | --- |
| `redxu-thunk` | ๋น๋๊ธฐ ์ฝ๋์ ๋ก์ง์ด ๊ฐ๋จํ  ๋. | ๊ฐ์ฅ ๋ง์ด ์ฌ์ฉํ๋ ๋ฏธ๋ค์จ์ด์ด๊ณ  ๊ฐ๋จํ๊ฒ ์์ํ  ์ ์๋ค. |
| `redux-saga` | ๋ณต์กํ ๋น๋๊ธฐ ๋ก์ง์ ๊ตฌํํด์ผ ํ  ๋. | ์ ๋ค๋ ์ดํฐ๋ฅผ ์ ๊ทน ํ์ฉํ๋ค. |

redux-saga๋ ๋ค์ ๊ฒ์๊ธ์์ ๋ค๋ค๋ณด๋๋ก ํ๋ค.

# 2. redux-thunk ์ฌ์ฉํด๋ณด๊ธฐ (๊ธฐ๋ณธ)

## 1) Thunk๋?

Thunk๋ **ํน์  ์์์ ๋์ค์ ํ  ์ ์๋๋ก ๋ฏธ๋ฃจ๊ธฐ ์ํด ํจ์ ํํ๋ก ๊ฐ์ผ ๊ฒ**์ ์๋ฏธํ๋ค.

> ์ปดํจํฐ ํ๋ก๊ทธ๋๋ฐ์์, ์ฝํฌ(Thunk)๋ ๊ธฐ์กด์ ์๋ธ๋ฃจํด์ ์ถ๊ฐ์ ์ธ ์ฐ์ฐ์ ์ฝ์ํ  ๋ ์ฌ์ฉ๋๋ ์๋ธ๋ฃจํด์ด๋ค. ์ฝํฌ๋ ์ฃผ๋ก ์ฐ์ฐ ๊ฒฐ๊ณผ๊ฐ ํ์ํ  ๋๊น์ง ์ฐ์ฐ์ ์ง์ฐ์ํค๋ ์ฉ๋๋ก ์ฌ์ฉ๋๊ฑฐ๋, ๊ธฐ์กด์ ๋ค๋ฅธ ์๋ธ๋ฃจํด๋ค์ ์์๊ณผ ๋ ๋ถ๋ถ์ ์ฐ์ฐ์ ์ถ๊ฐ์ํค๋ ์ฉ๋๋ก ์ฌ์ฉ๋๋๋ฐ...

> ์ฝํฌ(Thunk)๋ "๊ณ ๋ คํ๋ค"๋ผ๋ ์์ด ๋จ์ด์ธ "Think"์ ์์ด ๊ฒฉ ๊ณผ๊ฑฐ๋ถ์ฌ์ธ "Thunk"์์ ํ์๋ ๋จ์ด์ธ๋ฐ, ์ฐ์ฐ์ด ์ฒ ์ ํ๊ฒ "๊ณ ๋ ค๋ ํ", ์ฆ ์คํ์ด ๋ ํ์์ผ ์ฝํฌ์ ๊ฐ์ด ๊ฐ์ฉํด์ง๋ ๋ฐ์ ์ ๋๋ ๊ฒ์ด๋ผ๊ณ  ๋ณผ ์ ์๋ค. (์ํค๋ฐฑ๊ณผ)

์๋ฅผ ๋ค์ด ์ฃผ์ด์ง ํ๋ผ๋ฏธํฐ์ 1์ ๋ํ๋ ํจ์๋ฅผ ๋ง๋ค๊ณ  ์ถ๋ค๋ฉด, ์๋์ ๊ฐ์ด ์์ฑํ  ๊ฒ์ด๋ค.

```jsx
const addOne = x => x + 1;
addOne(1); // 2
```

addOne์ ํธ์ถํ์ ๋ ๋ฐ๋ก 1+1์ด ์ฐ์ฐ๋๋ ๊ฒ์ ๋ณผ ์ ์๋ค. ๊ทธ๋ฐ๋ฐ ์ด ์ฐ์ฐ ์์์ ๋์ค์ผ๋ก ๋ฏธ๋ฃจ๊ณ (thunk) ์ถ๋ค๋ฉด?

```jsx
const addOne = x => x + 1;
function addOneThunk(x) {
    const thunk = () => addOne(x);
    return thunk;
}

const fn = addOneThunk(1);
setTimeout(() => {
    const value = fn(); // fn์ด ์คํ๋๋ ์์ ์ ์ฐ์ฐ
    console.log(value);
}, 1000);
```

1์ด๊ฐ ์ง๋๋ฉด fn() -> addOneThunk(1)์ return๊ฐ์ธ thunk -> addOne(x)๊ฐ ํธ์ถ๋๋ ๊ตฌ์กฐ๋ค.
์ด๋ฅผ ํ์ดํ ํจ์๋ก๋ง ์์ฑํ๋ค๋ฉด ์๋์ ๊ฐ๋ค.

```jsx
cont addOne = x => x + 1;
const addOneThunk = x => () => addOne(x);

const fn = addOneThunk(1);
setTimeout(() => {
    const value = fn(); // fn์ด ์คํ๋๋ ์์ ์ ์ฐ์ฐ
    console.log(value);
}, 1000);
```

`redux-thunk` ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ์ฌ์ฉํ๋ฉด thunk ํจ์๋ฅผ ๋ง๋ค์ด์ ๋์คํจ์นํ  ์ ์๋ค. ๊ทธ๋ฌ๋ฉด ๋ฆฌ๋์ค ๋ฏธ๋ค์จ์ด๊ฐ ๊ทธ ํจ์๋ฅผ ์ ๋ฌ๋ฐ์ store์ dispatch์ getState๋ฅผ ํ๋ผ๋ฏธํฐ๋ก ๋ฃ์ด์ ํธ์ถํด์ค๋ค.

๋ค์์ redux-thunk์์ ์ฌ์ฉํ  ์ ์๋ ์์ thunk ํจ์๋ค.
```jsx
const sampleThunk = () => (dispatch, getState) => {
    // ํ์ฌ ์ํ๋ฅผ ์ฐธ์กฐํ  ์ ์๊ณ 
    // ์ ์ก์์ ๋์คํจ์นํ  ์๋ ์๋ค.
}
```

## 2) ๋ฏธ๋ค์จ์ด ์ ์ฉํ๊ธฐ

[์ด์  ๊ธ](https://lechuck.netlify.com/study/Redux2)์์ ๋ง๋   `counter` ๊ด๋ จ ์ฝ๋์ ๋ฏธ๋ค์จ์ด๋ฅผ ์ ์ฉํด๋ณด์.

![](./images/redux/counter.jpeg)

```bash
yarn add redux-thunk
```

```jsx {11}
// index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore, applyMiddleware } from 'redux';
import { Provider } from 'react-redux';
import './index.css';
import App from './App';
import rootReducer from './modules';
import ReduxThunk from 'redux-thunk';

const store = createStore(rootReducer, applyMiddleware(logger, ReduxThunk));

ReactDOM.render(
    <Provider store ={store}>
        <App />
    </Provider>,
    document.getElementById('root')
);

```

์์ ์ ๊น ์ธ๊ธํ๋๋ก `createStore` ํจ์ ์์ ๋ฏธ๋ค์จ์ด๋ฅผ ํ๋ผ๋ฏธํฐ๋ก ์ ๋ฌ๋ฐ๋ `applyMiddleware` ํจ์๋ฅผ ๋ฃ์ด์ค๋ค. redux-thunk ๋ฏธ๋ค์จ์ด๋ฅผ ์ฌ์ฉํ๊ธฐ ๋๋ฌธ์ `ReduxThunk`๋ฅผ ์ ๋ฌํ์๋ค.

## 3) Thunk ์์ฑ ํจ์ ๋ง๋ค๊ธฐ

`redux-thunk`๋ ์ก์ ์์ฑ ํจ์์์ ์ผ๋ฐ ์ก์ ๊ฐ์ฒด๋ฅผ ๋ฐํํ๋ ๋์ ์ ํจ์๋ฅผ ๋ฐํํ๋ค. ์นด์ดํฐ ๊ฐ์ ๋น๋๊ธฐ์ ์ผ๋ก ๋ณ๊ฒฝ์ํค๋ Thunk ์์ฑ ํจ์๋ฅผ ๋ง๋ค์ด๋ณด์.

```jsx {11,12,13,14,15,16,17,18,19,20,21}
// modules/counter.js

import { createAction, handleActions } from 'redux-actions';

const INCREASE = 'counter/INCREASE';
const DECREASE = 'counter/DECREASE';

export const increase = createAction(INCREASE);
export const decrease = createAction(DECREASE);

// 1์ด ๋ค์ increase ํน์ decrease ํจ์๋ฅผ ๋์คํจ์นํจ
export const increaseAsync = () => dispatch => {
    setTimeout(() => {
        dispatch(increase());
    }, 1000);
};
export const decreaseAsync = () => dispatch => {
    setTimeout(() => {
        dispatch(decrease());
    }, 1000);
};

const initialState = 0;

const counter = handleActions(
    {
        [INCREASE]: state => state +1,
        [DECREASE]: state => state -1
    },
    initialState
);

export default counter;
```

Thunk ์์ฑ ํจ์๋ฅผ ์๋์ ๊ฐ์ด CounterContainer ์ปดํฌ๋ํธ์ ์ ์ฉํ๋ค. ๊ทธ๋ฌ๋ฉด ์นด์ดํฐ์ +1, -1 ๋ฒํผ์ ๋๋ ์ ๋ 1์ด ๋ค์ ์ ์ฉ๋๋ ๊ฒ์ ๋ณผ ์ ์๋ค.

```jsx
// container/CounterContainer.js
import React from 'react';
import { connect } from 'react-redux';
import { increaseAsync, decreaseAsync } from '../modules/counter';
import Counter from '../components/Counter';

const CounterContainer = ({ number, increaseAsync, decreaseAsync}) => {
    return (
        <div>
            <Counter number={number}
            onIncrease={increaseAsync} 
            onDecrease={decreaseAsync} />
        </div>
    );
};

export default connect(
    state => ({
        number: state.counter
    }),
    {
        increaseAsync,
        decreaseAsync
    }
)(CounterContainer);
```

# 3. redux-thunk ์ฌ์ฉํด๋ณด๊ธฐ (์์ฉ)
## 1) API ํธ์ถ ํจ์ ์์ฑํ๊ธฐ
์ด๋ฒ์๋ thunk์ ์์ฑ์ ํ์ฉํ์ฌ ์น ์์ฒญ ๋น๋๊ธฐ ์์์ ์ฒ๋ฆฌํด๋ณธ๋ค.
์น ์์ฒญ ์ฐ์ต์ ์ํด JSONPlaceholder ์์ ์ ๊ณตํ๋ ๊ฐ์ง API๋ฅผ ์ฌ์ฉํ๋ค.

- ํฌ์คํธ ์ฝ๊ธฐ( id๋ 1~100 ์ฌ์ด ์ซ์)  
GET https://jsonplaceholder.typicode.com/posts/:id

- ๋ชจ๋  ์ฌ์ฉ์ ์ ๋ณด ๋ถ๋ฌ์ค๊ธฐ  
GET https://jsonplaceholder.typicode.com/users

API ํธ์ถ์ Promise ๊ธฐ๋ฐ์ axios ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ์ฌ์ฉํ๋ค.

```bash
yarn add axios
```

๊ฐ๋์ฑ์ ๋์ด๊ณ  ์ ์ง๋ณด์์ ํธ์์ฑ ์ฆ์ง์ ์ํ์ฌ API ํธ์ถ ํจ์๋ฅผ ๋ฐ๋ก ์์ฑํ๋ค.
```jsx
// lib/api.js
import axios from 'axios';

export const getPost = id => 
    axios.get(`https://jsonplaceholder.typicode.com/posts/${id}`);

export const getUsers = id =>
    axios.get(`https://jsonplaceholder.typicode.com/users`);
```

## 2) ์ก์ ํ์, thunk ํจ์, ๋ฆฌ๋์ ๋ง๋ค๊ธฐ

์ด์  ์ API๋ฅผ ์ฌ์ฉํ์ฌ ๋ฐ์ดํฐ๋ฅผ ๋ฐ์์ ์ํ๋ฅผ ๊ด๋ฆฌํ  sample์ด๋ผ๋ ๋ฆฌ๋์๋ฅผ ์์ฑํด ๋ณด์.

```jsx
// modules/sample.js
import { handleActions } from 'redux-actions';
import * as api from '../lib/api';

// ์ก์ ํ์์ ์ ์ธํ๋ค.
// ํ ์์ฒญ๋น ์ธ ๊ฐ์ ์ก์ ํ์์ ์ง๋๋ค.

const GET_POST = 'sample/GET_POST';
const GET_POST_SUCCESS = 'sample/GET_POST_SUCCESS';
const GET_POST_FAILURE = 'sample/GET_POST_FAILURE';

const GET_USERS = 'sample/GET_USERS';
const GET_USERS_SUCCESSS = 'sample/GET__USERS_SUCCESS';
const GET_USERS_FAILURE = 'sample/GET_USERS_FAILURE';

// thunk ํจ์๋ฅผ ์์ฑํ๋ค.
// thunk ํจ์ ๋ด๋ถ์์๋ ์์ํ  ๋, ์ฑ๊ณตํ์ ๋, ์คํจํ์ ๋ ๋ค๋ฅธ ์ก์์ ๋์คํจ์นํ๋ค.
// ํ๋ผ๋ฏธํฐ id๋ Container Component ์์ getPost ํธ์ถ ์ ์ ํด์ง ๊ฒ.
export const getPost = id => async dispatch => {
    dispatch({ type: GET_POST }); // ์์ฒญ์ ์์ํ ๊ฒ์ ์๋ฆผ.
    try{
        const response = await api.getPost(id);
        dispatch({
            type: GET_POST_SUCCESS,
            payload: response.data
        }); // ์์ฒญ ์ฑ๊ณต
    } catch(e){
        dispatch({
            type: GET_POST_FAILURE,
            payload:e,
            error: true
        }); // ์๋ฌ ๋ฐ์
        throw e; // ๋์ค์ ์ปดํฌ๋ํธ๋จ์์ ์๋ฌ๋ฅผ ์กฐํํ  ์ ์๊ฒ ํด ์ค
    }
};

export const getUsers = () => async dispatch => {
    dispatch({ type: GET_USERS }); // ์์ฒญ์ ์์ํ ๊ฒ์ ์๋ฆผ
    try{
        const response = await api.getUsers();
        dispatch({
            type: GET_USERS_SUCCESS,
            payload: response.data
        }); // ์์ฒญ ์ฑ๊ณต
    } catch(e){
        dispatch({
            type: GET_USERS_FAILURE,
            payload:e,
            error: true
        }); // ์๋ฌ ๋ฐ์
        throw e;
    }
};

const initialState = {
    loading: {
        GET_POST: false,
        GET_USERS: false
    },
    post: null,
    users: null
};

const sample = handleActions(
    {
        [GET_POST]: state => ({
            ...state,
            loading: {
                ...state.loading,
                GET_POST:true // ์์ฒญ ์์
            }
        }),
        [GET_POST_SUCCESS]: (state, action) => ({
            ...state,
            loading: {
                ...state.loading,
                GET_POST: false // ์์ฒญ ์๋ฃ
            },
            post: action.payload 
            // ์ํฏ๊ฐ post์ GET_POST ์์ฒญ์ผ๋ก ๋ฐ์์จ ๊ฒฐ๊ณผ(payload)๋ฅผ ํ ๋น.
        }),
        [GET_POST_FAILURE]: (state, action) => ({
            ...state,
            loading: {
                ...state.loading,
                GET_POST: false // ์์ฒญ ์๋ฃ
            }
        }),
        [GET_USERS]: state => ({
            ...state,
            loading: {
                ...state.loading,
                GET_USERS: true // ์์ฒญ ์์
            }
        }),
        [GET_USERS_SUCCESS]: (state, action) => ({
            ...state,
            loading: {
                ...state.loading,
                GET_USERS: false // ์์ฒญ ์๋ฃ
            },
            users:action.payload 
            // ์ํฏ๊ฐ usesr์ GET_USERS ์์ฒญ์ผ๋ก ๋ฐ์์จ ๊ฒฐ๊ณผ(payload)๋ฅผ ํ ๋น.
        }),
        [GET_USERS_FAILURE]: (state, action) => ({
            ...state,
            loading: {
                ...state.loading,
                GET_USERS: false // ์์ฒญ ์๋ฃ
            }
        })
    },
    initialState
);
```

์ค๋ณต๋๋ ๋ก์ง์ ๋์ค์ ๋ฆฌํฉํ ๋งํ  ์์ ์ด๋ค.
sample ๋ฆฌ๋์๋ฅผ ๋ฃจํธ ๋ฆฌ๋์์ ํฌํจ์ํจ๋ค.

```jsx
// modules/index.js
import { combineReducers } from 'redux';
import counter from './counter';
import sample from './sample';

const rootReducer = combineReducers({
    counter, // ์ฌ์ฉํ์ง๋ ์์!
    sample
});

export default rootReducer;
```

## 3) UI (Presentational Componet) ๋ง๋ค๊ธฐ

์ด์  UI (Presentational Componet)์ Container Component๋ฅผ ๋ง๋ค์ด์ผ ํ๋ค.
UI๋ API๋ฅผ ํตํด ๋ฐ์์จ ๋ฐ์ดํฐ๊ฐ ์ถ๋ ฅ๋๋ ํ๋ฉด์ด๋ฏ๋ก, ๋ฐ์์ฌ ๋ฐ์ดํฐ์ ํ์์ ๋ด๋ ํ์๊ฐ ์๋ค.

```json {5,6,14,15}
// post
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
}

// users
[
  {
    "id": 1,
    "name": "Leanne Graham",
    "username": "Bret",
    "email": "Sincere@april.biz",
    "address": {
      "street": "Kulas Light",
      "suite": "Apt. 556",
      "city": "Gwenborough",
      "zipcode": "92998-3874",
      "geo": {
        "lat": "-37.3159",
        "lng": "81.1496"
      }
    },
    "phone": "1-770-736-8031 x56442",
    "website": "hildegard.org",
    "company": {
      "name": "Romaguera-Crona",
      "catchPhrase": "Multi-layered client-server neural-net",
      "bs": "harness real-time e-markets"
    }
  }, (...)
]
```

ํ๋ฉด์ ์ถ๋ ฅํ  ๋ด์ฉ์ post์ ๊ฒฝ์ฐ title,body์ด๊ณ  users์ ๊ฒฝ์ฐ username๊ณผ email์ด๋ค.

```jsx
// components/Sample.js
import React from 'react';

const Sample = ({ loadingPost, loadingUsers, post, users}) => {
    return (
        <div>
            <section>
                <h1>ํฌ์คํธ</h1>
                {loadingPost && '๋ก๋ฉ ์ค...'}
                {!loadingPost && post && (
                    <div>
                        <h3>{post.title}</h3>
                        <h3>{post.body}</h3>
                    </div>
                )}
            </section>
            <hr />
            <section>
                <h1>์ฌ์ฉ์ ๋ชฉ๋ก</h1>
                {loadingUsers && '๋ก๋ฉ ์ค...'}
                {!loadingUsers && users && (
                    <ul>
                        {users.map(user => (
                            <li key={user.id}>
                                {user.username} ({user.email})
                            </li>
                        ))}
                    </ul>
                )}
            </section>
        </div>
    );
};

export default Sample;
```

๋ฐ์ดํฐ๋ฅผ ๋ถ๋ฌ์์ ๋ ๋๋งํด ์ค ๋๋ **์ ํจ์ฑ ๊ฒ์ฌ**๋ฅผ ํด ์ฃผ๋ ๊ฒ์ด ์ค์ํ๋ค.
axios์ ์ฑ๊ณตํด์ post๊ฐ true์ผ ๋(post &&) post.title๊ณผ post.body๋ฅผ ๋ณด์ฌ์ฃผ๊ฒ ๋ค๋ ๊ฒ์ฒ๋ผ ๋ง์ด๋ค. ๋ง์ฝ ๋ฐ์ดํฐ๊ฐ ์๋๋ฐ๋ ๋ถ๊ตฌํ๊ณ  post.title์ ์กฐํํ๋ ค๊ณ  ํ๋ฉด ์๋ฌ๊ฐ ๋ฐ์ํ๋ ๋ฐ๋์ ์ ํจ์ฑ ๊ฒ์ฌ๋ฅผ ํด์ค์ผ ํ๋ค.

## 4) Container Component ๋ง๋ค๊ธฐ

```jsx
// containers/SampleContainer.js
import React, { useEffect } from 'react';
import {connect} from 'react-redux';
import Sample from '../components/Sample';
import { getPost, getUsers } from '../modules/sample';

const SampleContainer = ({
    getPost,
    getUsers,
    post,
    users,
    loadingPost,
    loadingUsers
}) => {
    useEffect(() => {
        getPost(1);
        getUsers();
    }, [getPost, getUsers]);

    return (
        <Sample
            post={post}
            users={users}
            loaidngPost={loadingPost}
            loadingUsers={loadingUsers}
        />
    );
};

export default connect(
    ({sample}) => ({
        post: sample.post,
        users: sample.users,
        loadingPost: sample.loading.GET_POST,
        loaidngUsers: sample.loading.GET_USERS
    }),
    {
        getPost,
        getUsers
    }
)(SampleContainer);
```

๋ง์ง๋ง์ผ๋ก App ์ปดํฌ๋ํธ์์ SampleContainer ์ปดํฌ๋ํธ๋ฅผ ๋ ๋๋งํ๊ฒ๋ ํด์ฃผ๋ฉด ์์ฑ์ด๋ค.

![](./images/redux/result1.JPG)

๋์ ๊ณผ์ ์ ์ ๋ฆฌํด๋ณด์๋ฉด ๋ค์๊ณผ ๊ฐ๋ค.

1. Container Component์์ ๋ ๋๋ง์ด ๋์๋ง์ useEffect์ ์ํด `getPost`, `getUsers` thunk ํจ์๋ฅผ ํธ์ถํ๋ค. (connect ํจ์๋ก store์ ์ฐ๊ฒฐ๋์ด ์๊ธฐ ๋๋ฌธ์ ํธ์ถ์ด ๊ฐ๋ฅํ ๊ฒ.)

2. getPost ํจ์๊ฐ ์คํ๋๋ฉด API ์์ฒญ์ด ์์ํ์์ ์๋ฆฌ๋ `GET_POST` ์ก์์ ๋จผ์  dispatchํ๋ค.

3. ๋ฆฌ๋์์ ์ ์๋ `GET_POST` ์ก์์ ๋์์ loading state๋ฅผ true๋ก ๋ณ๊ฒฝํ๋ ๊ฒ์ด๋ค.

4. ์ดํ ๋น๋๊ธฐ API ํจ์ ์ฒ๋ฆฌ ๊ณผ์ ์ด ์งํ๋๋ค. ์ฒ๋ฆฌ๊ฐ ๋๋๋ฉด, ์ฑ๊ณต/์คํจ ์ฌ๋ถ์ ๋ฐ๋ผ `GET_POST_SUCCESS` ์ก์ ํน์ `GET_POST_FAILURE` ์ก์์ dispatchํ๋ค. GET_POST์ ๋ง์ฐฌ๊ฐ์ง๋ก ๋ฆฌ๋์์ ์ ์๋๋๋ก ์ํ ๋ณํ๋ฅผ ์ํ.

5. `getUsers` thunk ํจ์ ๋ํ ์ด์ ๊ฐ์ ๋ฐฉ์์ผ๋ก, ๊ฑฐ์ ๋์์ ์ผ๋ก ์ํ๋๋ค.

## 5) ๋ฆฌํฉํ ๋ง
thunk ํจ์์ ์ฝ๋๊ฐ ๋๋ฌด ๊ธธ๊ณ  ๋ก๋ฉ ์ํ๋ฅผ ๋ฆฌ๋์์์ ๊ด๋ฆฌํ๋ ์์์ ๊ท์ฐฎ๋ค.
๋ฐ๋ณต๋๋ ๋ก์ง์ ๋ฐ๋ก ๋ถ๋ฆฌํ์ฌ ์ฝ๋๋ฅผ ์ค์ฌ๋ณด๋๋ก ํ์.

```jsx
// lib/createRequestThunk.js
export default function createRequestThunk(type, request){
    // ์ฑ๊ณต ๋ฐ ์คํจ ์ก์ ํ์์ ์ ์
    const SUCCESS = `${type}_SUCCESS`;
    const FAILURE = `${type}_FAILURE`;
    
    // params๋ SampleContainer์์ getPost()ํธ์ถ ์ ํ๋ผ๋ฏธํฐ๋ก ์ ๋ฌํ ๊ฐ์ ๋ฐ๋๋ค.
    return params => async dispatch =>{
        dispatch({type}); // API ์์ฒญ์ด ์์๋จ
        try{
            const response = await request(params);
            dispatch({
                type:SUCCESS,
                payload:response.data
            }); // API ์์ฒญ์ ์ฑ๊ณต
        } catch(e){
            dispatch({
                type: FAILURE,
                payload:e,
                error:true
            }); // API ์์ฒญ ์ค ์๋ฌ ๋ฐ์
            throw e;
        }
    };
}

// ์ฌ์ฉ๋ฒ: createRequestThunk('GET_USERS', api.getUsers);
```
์ ํธ ํจ์ `createRequestThunk`๋ API ์์ฒญ์ ํด ์ฃผ๋ thunk ํจ์๋ฅผ ํ ์ค๋ก ์์ฑํ  ์ ์๊ฒ ํด์ค๋ค. ์ก์ ํ์๊ณผ API๋ฅผ ์์ฒญํ๋ ํจ์๋ฅผ ํ๋ผ๋ฏธํฐ๋ก ๋ฃ์ด ์ฃผ๋ฉด ๋๋จธ์ง ์์์ ๋์  ์ฒ๋ฆฌํ๋ค. ์ด ํจ์๋ฅผ ์ฌ์ฉํ์ฌ ๊ธฐ์กด thunk ํจ์์ ์ฝ๋๋ฅผ ๋์ฒดํด๋ณด์.

```jsx {4,14,15}
// modules/sample.js
import { handleActions } from 'redux-actions';
import * as api from '../lib/api';
import createRequestThunk from '../lib/createRequestThunk';

const GET_POST = 'sample/GET_POST';
const GET_POST_SUCCESS = 'sample/GET_POST_SUCCESS';
const GET_POST_FAILURE = 'sample/GET_POST_FAILURE';

const GET_USERS = 'sample/GET_USERS';
const GET_USERS_SUCCESSS = 'sample/GET__USERS_SUCCESS';
const GET_USERS_FAILURE = 'sample/GET_USERS_FAILURE';

export const getUsers = createRequestThunk(GET_USERS, api.getUsers);
export const getPost = createRequestThunk(GET_POST, api.getPost);
// export const getPost = id => async dispatch => {
//     dispatch({ type: GET_POST }); // ์์ฒญ์ ์์ํ ๊ฒ์ ์๋ฆผ.
//     try{
//         const response = await api.getPost(id);
//         dispatch({
//             type: GET_POST_SUCCESS,
//             payload: response.data
//         }); // ์์ฒญ ์ฑ๊ณต
//     } catch(e){
//         dispatch({
//             type: GET_POST_FAILURE,
//             payload:e,
//             error: true
//         }); // ์๋ฌ ๋ฐ์
//         throw e; // ๋์ค์ ์ปดํฌ๋ํธ๋จ์์ ์๋ฌ๋ฅผ ์กฐํํ  ์ ์๊ฒ ํด ์ค
//     }
// };

const initialState = {
    (...)
};

const sample = handleActions(
    (...)
);
```

์ด๋ฒ์๋ ์์ฒญ์ ๋ก๋ฉ ์ํ ๊ด๋ฆฌ๋ฅผ ๊ฐ์ ํ์.
๊ธฐ์กด์๋ ๋ฆฌ๋์ ๋ด๋ถ์์ ๊ฐ ์์ฒญ์ ๊ด๋ จ๋ ์ก์์ด ๋์คํจ์น๋  ๋๋ง๋ค ๋ก๋ฉ ์ํ๋ฅผ ๋ณ๊ฒฝํด์ฃผ์๋ค.
์ด ์์์ ๋ก๋ฉ ์ํ๋ง ๊ด๋ฆฌํ๋ ๋ฆฌ๋์ค ๋ชจ๋์ ๋ฐ๋ก ์์ฑํ์ฌ ์ฒ๋ฆฌํ์.

```jsx
// modules/loading.js
import { createAction, handleActions } from 'redux-actions';

const START_LOADING = 'loading/START_LOADING';
const FINISH_LOADING = 'loading/FINISH_LOADING';

// ์์ฒญ์ ์ํ ์ก์ ํ์์ payload๋ก ์ค์ ํ๋ค. ex) sample/GET_POST
export const startLoading = createAction(
    START_LOADING,
    requestType => requestType
);

export const finishLoading = createAction(
    FINISH_LOADING,
    requestType => requestType
);

const initialState = {};

const loading = handleActions(
    {
        [START_LOADING]: (state, action) => ({
            ...state,
            [action.payload]: true
        }),
        [FINISH_LOADING]: (state, action) => ({
            ...state,
            [action.payload]: false
        })
    },
    initialState
);

export default loading;
```

๋ก๋ฉ ์ํ ๊ด๋ฆฌ๋ฅผ ์ํ ์ก์ ํ์, ์ก์ ์์ฑ์, ๋ฆฌ๋์๋ฅผ ๋ง๋ค์๋ค. loading ๋ฆฌ๋์ ๋ํ ./index.js์์ ๋ฃจํธ ๋ฆฌ๋์์ ํฌํจ์ํจ๋ค.

loading ๋ฆฌ๋์ค ๋ชจ๋์์ ๋ง๋  ์ก์ ์์ฑ ํจ์๋ createRequestThunk์์ ์ฌ์ฉํ๋ค.


```jsx {2,12,19,26}
// lib/createRequestThunk.js
import { startLoading, finishLoading } from '../modules/loading';

export default function createRequestThunk(type, request){
    // ์ฑ๊ณต ๋ฐ ์คํจ ์ก์ ํ์์ ์ ์
    const SUCCESS = `${type}_SUCCESS`;
    const FAILURE = `${type}_FAILURE`;
    
    // params๋ SampleContainer์์ getPost()ํธ์ถ ์ ํ๋ผ๋ฏธํฐ๋ก ์ ๋ฌํ ๊ฐ์ ๋ฐ๋๋ค.
    return params => async dispatch =>{
        dispatch({type}); // API ์์ฒญ์ด ์์๋จ
        dispatch(startLoading(type)); // ๋ก๋ฉ์ค์ผ๋ก ์ํ ๋ณ๊ฒฝ
        try{
            const response = await request(params);
            dispatch({
                type:SUCCESS,
                payload:response.data
            }); // API ์์ฒญ์ ์ฑ๊ณต
            dispatch(finishLoading(type)); // ๋ก๋ฉ ๋
        } catch(e){
            dispatch({
                type: FAILURE,
                payload:e,
                error:true
            }); // API ์์ฒญ ์ค ์๋ฌ ๋ฐ์
            dispatch(startLoading(type));
            throw e;
        }
    };
}

// ์ฌ์ฉ๋ฒ: createRequestThunk('GET_USERS', api.getUsers);
```

๊ทธ๋ฌ๋ฉด SampleContainer์์ ๋ก๋ฉ ์ํ๋ฅผ ๋ค์๊ณผ ๊ฐ์ด ์กฐํํ  ์ ์๋ค.
์์ ๋ฃจํธ ๋ฆฌ๋์์ ํฌํจ์์ผ store์ ์ ํด์ก๋ loading ๋ชจ๋์ ์ํ๋ฅผ connect ํจ์๋ฅผ ํตํด ๋ถ๋ฌ์ ์ฌ์ฉํ๋ ๊ฒ์ด๋ค.

```jsx {31,34,35}
// containers/SampleContainer.js
import React, { useEffect } from 'react';
import {connect} from 'react-redux';
import Sample from '../components/Sample';
import { getPost, getUsers } from '../modules/sample';

const SampleContainer = ({
    getPost,
    getUsers,
    post,
    users,
    loadingPost,
    loadingUsers
}) => {
    useEffect(() => {
        getPost(1);
        getUsers();
    }, [getPost, getUsers]);

    return (
        <Sample
            post={post}
            users={users}
            loaidngPost={loadingPost}
            loadingUsers={loadingUsers}
        />
    );
};

export default connect(
    ({sample, loading}) => ({
        post: sample.post,
        users: sample.users,
        loadingPost: loading['sample/GET_POST'],
        loadingUsers: loading['sample/GET_USERS]'
        // loadingPost: sample.loading.GET_POST,
        // loaidngUsers: sample.loading.GET_USERS
    }),
    {
        getPost,
        getUsers
    }
)(SampleContainer);
```

์ด์  sample ๋ฆฌ๋์์์ ๋ถํ์ํ ์ฝ๋๋ฅผ ์ง์ฐ๋ฉด ๋๋ค.

```jsx
// modules/sample.js
import { handleActions } from 'redux-actions';
import * as api from '../lib/api';
import createRequestThunk from '../lib/createRequestThunk';

const GET_POST = 'sample/GET_POST';
const GET_POST_SUCCESS = 'sample/GET_POST_SUCCESS';
// const GET_POST_FAILURE = 'sample/GET_POST_FAILURE';

const GET_USERS = 'sample/GET_USERS';
const GET_USERS_SUCCESS = 'sample/GET_USERS_SUCCESS';
// const GET_USERS_FAILURE = 'sample/GET_USERS_FAILURE';

export const getPost = createRequestThunk(GET_POST, api.getPost);
export const getUsers = createRequestThunk(GET_USERS, api.getUsers);

const initialState = {
    // loading : {
    //     GET_POST: false,
    //     GET_USERS: false
    // },
    // post: null,
    // users: null
    post:null,
    users:null
};

const sample = handleActions(
    {
        [GET_POST_SUCCESS]: (state,action) => ({
            ...state,
            post: action.payload
        }),
        [GET_USERS_SUCCESS]: (state,action) => ({
            ...state,
            users: action.payload
        })
        // [GET_POST_SUCCESS]: (state, action) => ({
        //     ...state,
        //     loading: {
        //         ...state.loading,
        //         GET_POST: false // ์์ฒญ ์๋ฃ
        //     },
        //     post: action.payload // ์ํฏ๊ฐ post์ GET_POST ์์ฒญ์ผ๋ก ๋ฐ์์จ ๊ฒฐ๊ณผ(payload)๋ฅผ ํ ๋น.
        // }),
    },
    initialState
);

export default sample;

```

๋์ ๊ณผ์ ์ ์ ๋ฆฌํด๋ณด๋ฉด ๋ค์๊ณผ ๊ฐ๋ค.

1. `SampleContainer`์์ useEffect์ ์ํด `getPost`, `getUsers` thunk ํจ์๊ฐ ํธ์ถ๋๋ค. 

2. modules/sample.js ์ ์ ์๋ getPost์ getUsers๋ `createRequsetThunk` ํจ์๋ฅผ ๊ฐ๋ฆฌํจ๋ค.

3. `createRequestThunk` ํจ์๋ API ์์ฒญ์ ์ํ dispatch๋ฅผ ์ํํ๋ฉด์, ๋ก๋ฉ ์ํ ๊ด๋ฆฌ๋ฅผ ์ํ dispatch๋ ์ํํ๋ค. ๋ฆฌ๋์๋ ์ด ์ก์์ ๋ฐ์ ์ํ๋ฅผ ๋ณํํ ํ ์คํ ์ด์ ์ ์ฅํ๋ค.

4. ์คํ ์ด์ ์ ์ฅ๋ ์ํ๋ฅผ `SampleContainer`์์ connect ํจ์๋ฅผ ํตํด ๊ฐฑ์ ํด์จ๋ค. loadingPost์ loadingUsers๋ ๋ก๋ฉ ์ํ๋ฅผ ๊ด๋ฆฌํ๊ธฐ ์ํ type ์ธ์๋ payload๋ผ๋ ๋ณ๋์ ๊ฐ์ ๊ฐ์ ธ์ ์ด๋ค API ์์ฒญ์ ๊ดํ ๋ก๋ฉ ์ํ์ธ์ง๋ฅผ ํ์ํ๋ค.

ex) {type: "loading/START_LOADING", payload: "sample/GET_POST"}

5. ์ํ๋ฅผ UI์ ์ถ๋ ฅํ๋ค.

# References

[๋ฆฌ๋์ค ๋ฏธ๋ค์จ์ด๋ ๋ฌด์์ธ๊ฐ? (1)](https://velog.io/@youthfulhps/%EB%A6%AC%EB%8D%95%EC%8A%A4-%EB%AF%B8%EB%93%A4%EC%9B%A8%EC%96%B4%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)

<๋ฆฌ์กํธ๋ฅผ ๋ค๋ฃจ๋ ๊ธฐ์  ๊ฐ์ ํ>(๊น๋ฏผ์ค, 2019)

<๋ฆฌ์กํธ ์ค์  ํ๋ก๊ทธ๋๋ฐ ๊ฐ์ ํ>(์ด์ฌ์น, 2020)