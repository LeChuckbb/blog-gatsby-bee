---
title: 'π Redux(4) - λ―Έλ€μ¨μ΄ redux-saga'
date: 2021-06-21 21:33:13
category: 'study'
thumbnail: 'https://gatsby-blog-images.s3.ap-northeast-2.amazonaws.com/thumb_redux.png'
description: 'thunkμ μ΄μ΄ sagaκΉμ§ μ¬μ©ν΄λ³΄λ©΄ λ―Έλ€μ¨μ΄μ λν κ°μ μ‘μ μ μμ κ² κ°λ€'
tags: ['Redux','middleware','redux-saga','ES6 Generator']
draft: false
---

![](./images/redux/redux-saga1.JPG)
*λ³Έ κ²μκΈμ μ± <λ¦¬μ‘νΈλ₯Ό λ€λ£¨λ κΈ°μ  κ°μ ν> 18μ₯ 'λ¦¬λμ€ λ―Έλ€μ¨μ΄λ₯Ό ν΅ν λΉλκΈ° μμ κ΄λ¦¬'λ₯Ό μ λ¦¬ν λ΄μ©μλλ€*

# 1. Redux-sagaλ?

redux-sagaλ redux-thunkμ μ΄μ΄ κ°μ₯ λ§μ΄ μ¬μ©λλ λΉλκΈ° μμ κ΄λ ¨ λ―Έλ€μ¨μ΄λ€. `redux-saga`λ redux-thunkλ³΄λ€ κΉλ€λ‘μ΄ μν©μμ μ μ©νλ€. μ΄λ₯Όνλ©΄ λ€μκ³Ό κ°λ€.

- κΈ°μ‘΄ μμ²­μ μ·¨μ μ²λ¦¬ν΄μΌ ν  λ(λΆνμν μ€λ³΅ μμ²­ λ°©μ§)
- νΉμ  μ‘μμ΄ λ°μνμ λ λ€λ₯Έ μ‘μμ λ°μμν€κ±°λ, API μμ²­ λ± λ¦¬λμ€μ κ΄κ³μλ μ½λλ₯Ό μ€νν  λ
- μΉμμΌμ μ¬μ©ν  λ
- API μμ²­ μ€ν¨ μ μ¬μμ²­ν΄μΌ ν  λ

## ES6 Generator νκ³ κ°κΈ°

redux-sagaλ ES6μ `μ λ€λ μ΄ν°(generator)` ν¨μλΌλ λ¬Έλ²μ μ¬μ©νλ€. λ°λΌμ μ λ€λ μ΄ν°μ λν΄ μμ λ νμκ° μλ€.

μ λ€λ μ΄ν°λ μ½λ λΈλ‘μ μ€νμ μΌμ μ€μ§νλ€κ° νμν μμ μ μ¬κ°ν  μ μλ νΉμν ν¨μλ€. μλ μ½λλ₯Ό ν΅ν΄ μ λ€λ μ΄ν° μ¬μ©λ²μ μμλ³΄μ.

```javascript
function* generatorFunction(){
    console.log('μλνμΈμ');
    yield 1;
    console.log('μ λ€λ μ΄ν° ν¨μ');
    yield 2;
    console.log('function*');
    yield 3;
    return 4;
}

const generator = generatorFunction();

generator.next();
// μλνμΈμ
// {value: 1, done:false}
generator.next();
// μ λ€λ μ΄ν° ν¨μ
// {value: 2, done:false}
generator.next();
// function*
// {value: 3, done:false}
generator.next();
// {value: 4, done:true}
generator.next();
// {value: undefined, done: true}
```

μ λ€λ μ΄ν° ν¨μλ₯Ό νΈμΆνλ©΄ **μΌλ° ν¨μμ²λΌ ν¨μ μ½λ λΈλ‘μ μ€ννλ κ²μ΄ μλλΌ** μ λ€λ μ΄ν° κ°μ²΄λ₯Ό μμ±ν΄ λ°ννλ€. μ¦, ν¨μμ νλ¦μ΄ λ©μΆ° μλ μνλ€. μ΄ν μ λ€λ μ΄ν° κ°μ²΄μ next λ©μλλ₯Ό νΈμΆνλ©΄ yield ννμκΉμ§ μ½λ λΈλ‘μ μ€ννκ³ , yieldλ κ°μ value νλ‘νΌν° κ°μΌλ‘, falseλ₯Ό done νλ‘νΌν° κ°μΌλ‘ κ°λ κ°μ²΄λ₯Ό λ°ννλ€. return λ¬Έμ λλ¬νλ©΄ done νλ‘νΌν°μ κ°μΌλ‘ trueκ° λ°νλλ μμ΄λ€.

μ λ¦¬νμλ©΄ μ λ€λ μ΄ν°λ `yield` ν€μλλ₯Ό ν΅ν΄ μ€νμ μΌμ μ€μ§νκ³ , `next` λ©μλλ₯Ό ν΅ν΄ λ€μ μ¬κ°νλ κ²μ΄λ€.

```javascript
function* watchGenerator(){
    console.log('λͺ¨λν°λ§ μ€...');
    let prevAction = null;
    while(true){
        const action = yield;
        console.log('μ΄μ  μ‘μ: ' ,prevAction);
        prevAction = action;
        if (action.type === 'HELLO')
            console.log('μλνμΈμ!');
    }
}

const watch = watchGenerator();

watch.next();
// λͺ¨λν°λ§ μ€...
// { value: undefined, done:false }
watch.next( {type: 'TEST' });
// μ΄μ  μ‘μ: null
// { value: undefined, done: false }
watch.next( {type: 'HELLO' });
// μ΄μ  μ‘μ: {type: TEST}
// μλνμΈμ!
// { value: unedfined, done: false} 
```

redux-sagaλ μ μ½λμ λΉμ·ν μλ¦¬λ‘ μλνλ€. redux-sagaλ μ°λ¦¬κ° λμ€ν¨μΉνλ μ‘μμ λͺ¨λν°λ§ν΄μ κ·Έμ λ°λΌ νμν μμμ λ°λ‘ μνν  μ μλ λ―Έλ€μ¨μ΄λ€.

# 2. redux-saga μ¬μ©ν΄λ³΄κΈ° (κΈ°λ³Έ)

κΈ°μ‘΄μ thunk ν¨μλ‘ κ΅¬ννλ λΉλκΈ° μΉ΄μ΄ν°λ₯Ό μ΄λ²μλ redux-sagaλ₯Ό μ΄μ©ν΄μ κ΅¬νν΄λ³΄μ. μ΄μ  κΈμμ λ§λ  νλ‘μ νΈλ₯Ό κ·Έλλ‘ κ°μ Έλ€ μ°λλ‘ νλ€. 

## 1) μ λ€λ μ΄ν° ν¨μ(saga) λ§λ€κΈ°

```bash
yarn add redux-saga
```

μ°μ  redux-saga λΌμ΄λΈλ¬λ¦¬λ₯Ό μ€μΉνλ€. μ΄ν counter λͺ¨λμ μ΄μ΄μ κΈ°μ‘΄ thunk ν¨μλ₯Ό μ κ±°νκ³ , `INCREASE_ASYNC`, `DECREASE_ASYNC` μ‘μ νμμ μ μΈνλ€. ν΄λΉ μ‘μμ λν μ‘μ μμ± ν¨μλ λ§λ€κ³ , μ λ€λ μ΄ν° ν¨μλ λ§λ λ€. μ΄ μ λ€λ μ΄ν° ν¨μλ₯Ό μ¬κ°(saga)λΌκ³  λΆλ₯Έλ€. λ¦¬λμλ κΈ°μ‘΄μ λ§λ  κ² κ·Έλλ‘ μ μ§νλ€.

```jsx {3,7,8,12,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33}
// modules/counter.js
import { createAction, handleActions } from 'redux-actions';
import { delay, put, takeEvery, takeLatest } from 'redux-saga/effects';

const INCREASE = 'counter/INCREASE';
const DECREASE = 'counter/DECREASE';
const INCREASE_ASYNC = 'counter/INCREASE_ASYNC';
const DECREASE_ASYNC = 'counter/DECREASE_ASYNC';

export const increase = createAction(INCREASE);
export const decrease = createAction(DECREASE);

// λ§μ°μ€ ν΄λ¦­ μ΄λ²€νΈκ° payload μμ λ€μ΄κ°μ§ μλλ‘
// () => undefinedλ₯Ό λ λ²μ§Έ νλΌλ―Έν°λ‘ λ£μ΄ μ€λ€
export const increaseAsync = createAction(INCREASE_ASYNC, () => undefined);
export const decreaseAsync = createAction(DECREASE_ASYNC, () => undefined);

function* increaseSaga(){
    yield delay(1000); // 1μ΄λ₯Ό κΈ°λ€λ¦°λ€
    yield put(increase()); // νΉμ  μ‘μμ λμ€ν¨μΉνλ€
}
function* decreaseSaga(){
    yield delay(1000);
    yield put(decrease());
}

export function* counterSaga(){
    // takeEveryλ λ€μ΄μ€λ λͺ¨λ  μ‘μμ λν΄ νΉμ  μμμ μ²λ¦¬νλ€
    yield takeEvery(INCREASE_ASYNC, increaseSaga);
    // yakeLatestλ κΈ°μ‘΄μ μ§ν μ€μ΄λ μμμ΄ μλ€λ©΄ μ·¨μ μ²λ¦¬νκ³ 
    // κ°μ₯ λ§μ§λ§μΌλ‘ μ€νλ μμλ§ μννλ€
    yield takeLatest(DECREASE_ASYNC, decreaseSaga);
}

const initialState = 0;

const counter = handleActions(
    {
        [INCREASE]: state => state +1,
        [DECREASE]: state => state -1,
    },
    initialState
);

export default counter;
```

## 2) redux-saga λ―Έλ€μ¨μ΄ μ μ©νκΈ°

λ£¨νΈ μ¬κ°λ₯Ό λ§λ€μ΄μ μμμ μ μν `counterSaga`λ₯Ό λ£μ΄ μ€λ€.

```jsx {3,14,15,16,17}
// modules/index.js
import { combineReducers } from 'redux';
import counter, { counterSaga } from './counter';
import sample from './sample';
import loading from './loading';
import { all } from 'redux-saga/effects';

const rootReducer = combineReducers({
    counter,
    sample,
    loading
});

export function* rootSaga(){
    // all ν¨μλ μ¬λ¬ μ¬κ°λ₯Ό ν©μ³ μ€λ€.
    yield all([counterSaga()]);
}

export default rootReducer;
```

μ΄μ  μ€ν μ΄μ redux-saga λ―Έλ€μ¨μ΄λ₯Ό μ μ©ν΄ μ€λ€.
μ‘μ λμ€ν¨μΉ κ³Όμ μ λ νΈνκ² νμΈνκΈ° μν΄ λ¦¬λμ€ κ°λ°μ λκ΅¬(devtools)λ₯Ό ν¨κ» μ μ©νλ€.

```jsx {9,12,13,16,19,21}
// ./index.js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import {applyMiddleware, createStore} from 'redux';
import {Provider} from 'react-redux';
import rootReducer, {rootSaga} from './modules';
import {createLogger} from 'redux-logger';
import ReduxThunk from 'redux-thunk';
import createSagaMiddleware from 'redux-saga';
import { composeWithDevTools } from 'redux-devtools-extension';

const logger = createLogger();
const sagaMiddleware = createSagaMiddleware();
const store = createStore(
  rootReducer, 
  composeWithDevTools(applyMiddleware(logger, ReduxThunk, sagaMiddleware))
);
sagaMiddleware.run(rootSaga);

ReactDOM.render(
  <Provider store={store}>
    <React.StrictMode>
    <App />
    </React.StrictMode>
  </Provider>,
  document.getElementById('root')
);
```

App μ»΄ν¬λνΈμμ CounterContainerλ₯Ό λ λλ§ νκ²λ μ§μ ν΄μ£Όκ³  κ²°κ³Όλ₯Ό νμΈν΄λ³΄μ. counter λ¦¬λμ€ λͺ¨λμ λ³κ²½νκΈ°λ νμ§λ§ CounterContainerμμ μμ ν  κ²μ μλ€. κΈ°μ‘΄μ μ¬μ© μ€μ΄λ thunk ν¨μμ λκ°μ μ΄λ¦μΌλ‘ μ‘μ μμ± ν¨μλ₯Ό λ§λ€μκΈ° λλ¬Έμ΄λ€.

![](./images/redux/redux-saga1.JPG)

μ μ¬μ§μ +1 λ²νΌμ λ λ² λλ μ λμ μν©μ΄λ€. `INCREASE_ASYNC` μ‘μμ΄ λ λ² λμ€ν¨μΉλκ³  μ΄μ λ°λΌ INCREASE μ‘μλ λ λ² λμ€ν¨μΉλλ κ²μ λ³Ό μ μλ€. `takeEvery`λ₯Ό μ¬μ©νμ¬ increaseSagaλ₯Ό λ±λ‘νμΌλ―λ‘ λμ€ν¨μΉλλ λͺ¨λ  `INCREASE_ASYNC `μ‘μμ λν΄ 1μ΄ ν INCREASE μ‘μμ λ°μμν€λ κ²μ΄λ€.

![](./images/redux/redux-saga2.JPG)

λ°λ©΄ -1 λ²νΌμ λκ°μ΄ λ λ² λλ μ λλ DECREASE μ‘μμ΄ ν λ²λ§ λμ€ν¨μΉλκ³  μλ€. μ΄λ decreaseSagaλ₯Ό λ±λ‘ν  λ `takeLatest`λ₯Ό μ¬μ©νκΈ° λλ¬Έμ΄λ€. takeLatestλ μ¬λ¬ μ‘μμ΄ μ€μ²©λμ΄ λμ€ν¨μΉλ  λ κΈ°μ‘΄μ κ²λ€μ λ¬΄μνκ³  κ°μ₯ λ§μ§λ§ μ‘μλ§ μ λλ‘ μ²λ¦¬νλ€.

λμ κ³Όμ μ μ λ¦¬ν΄λ³΄λ©΄ λ€μκ³Ό κ°λ€.

1. +1 λ²νΌμ΄ λλ¦¬λ©΄ components/Counterμ onIncrease ν¨μκ° νΈμΆλλ€.

2. onIncrease ν¨μλ /containers/CounterContainerμμ μ λ¬λ°μ κ²μ΄λ€. CounterContainerλ connect ν¨μλ₯Ό ν΅ν΄ storeλ‘λΆν° μνμ λμ€ν¨μΉλ₯Ό μ€μκ°μΌλ‘ λ°κ³  μμκ³ , onIncrease ν¨μλ μ€ν μ΄μμ μ λ¬λ°μ `increaseAsync` μ‘μ μμ± ν¨μλ€. (λ§€κ°λ³μλ‘ μ λ¬λ μ΄λ¦λ§ λ€λ₯Έκ²)

3. increaseAsync μ‘μ μμ± ν¨μλ ./modules/counterμμ μ μλμλ€. λ£¨νΈ μ¬κ°μ λ΄κ²¨μ μ€ν μ΄μ ν¬ν¨λμκΈ° λλ¬Έμ CounterContainerμμλ νΈμΆν μκ° μμλ κ²μ΄λ€.
increaseAsync μ‘μ μμ± ν¨μκ° μμ±ν `INCREASE_ASYNC`
 μ‘μ κ°μ²΄μ μν΄μ `counterSaga` μ λ€λ μ΄ν° ν¨μκ° νΈμΆλλ€. `takeEverey`μ μν΄μ `INCREASE_ASYNC` μ‘μμ΄ λ€μ΄μ€λ©΄ 
`increaseSaga` ν¨μλ₯Ό νΈμΆνκ³ , μ΄λ 1μ΄ λλ μ΄ νμ λ€μ `INCREASE` μ‘μμ λμ€ν¨μΉνκ² λμ΄, μ΅μ’μ μΌλ‘ μνμ +1μ ν΄μ€λ€.


# 3. redux-saga μ¬μ©ν΄λ³΄κΈ° (μμ©)

## 1) μ λ€λ μ΄ν° ν¨μ(saga) λ§λ€κΈ°

μ΄λ²μλ redux-sagaλ₯Ό μ΄μ©ν΄μ λΉλκΈ° API μμ²­μ ν΄λ³΄μ. κΈ°μ‘΄μ thunkλ‘ κ΄λ¦¬νλ μ‘μ μμ± ν¨μλ₯Ό μμ κ³ , μ¬κ°λ₯Ό μ¬μ©ν΄μ μ²λ¦¬νμ. sample λ¦¬λμ€ λͺ¨λμ λ€μκ³Ό κ°μ΄ μμ νλ©΄ λλ€.

```jsx {4,15,16,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,57,58,59,60}
// modules/sample.js
import { createAction,handleActions } from 'redux-actions';
import * as api from '../lib/api';
import { call, put, takeLatest } from 'redux-saga/effects';
import { startLoading, finishLoading } from './loading';

const GET_POST = 'sample/GET_POST';
const GET_POST_SUCCESS = 'sample/GET_POST_SUCCESS';
const GET_POST_FAILURE = 'sample/GET_POST_FAILURE';

const GET_USERS = 'sample/GET_USERS';
const GET_USERS_SUCCESS = 'sample/GET_USERS_SUCCESS';
const GET_USERS_FAILURE = 'sample/GET_USERS_FAILURE';

export const getPost = createAction(GET_POST, id => id);
export const getUsers = createAction(GET_USERS);

function* getPostSaga(action){
    yield put(startLoading(GET_POST)); // λ‘λ© μμ. put = dispatch
    // νλΌλ―Έν°λ‘ actionμ λ°μ μ€λ©΄ μ‘μμ μ λ³΄λ₯Ό μ‘°νν  μ μλ€.
    try{
        // callμ μ¬μ©νλ©΄ Promiseλ₯Ό λ°ννλ ν¨μλ₯Ό νΈμΆνκ³ , κΈ°λ€λ¦΄ μ μλ€.
        // μ²« λ²μ§Έ νλΌλ―Έν°λ ν¨μ, λλ¨Έμ§ νλΌλ―Έν°λ ν΄λΉ ν¨μμ λ£μ μΈμλ€.
        const post = yield call(api.getPost, action.payload); // API νΈμΆ
        yield put({
            type: GET_POST_SUCCESS,
            payload: post.data
        });
    } catch(e){
        yield put({
            type: GET_POST_FAILURE,
            payload: e,
            error : true
        });
    }
    yield put(finishLoading(GET_POST)); // λ‘λ© μλ£
}

function* getUsersSaga(){
    yield put(startLoading(GET_USERS));
    try{
        const users = yield call(api.getUsers);
        yield put({
            type: GET_USERS_SUCCESS,
            payload: users.data
        });
    } catch(e){
        yield put({
            type: GET_USERS_FAILURE,
            payload: e,
            error: true
        });
    }
    yield put(finishLoading(GET_USERS));
}

export function* sampleSaga(){
    yield takeLatest(GET_POST, getPostSaga);
    yield takeLatest(GET_USERS, getUsersSaga);
}

const initialState = {
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
    },
    initialState
);

export default sample;

```

μ΄ν modules/index.jsμμ sampleSagaλ₯Ό λ£¨νΈ μ¬κ°μ λ±λ‘νκ³ ,
App μ»΄ν¬λνΈμμ SampleContainerλ₯Ό λ λλ§νκ²λ λ³κ²½ν΄μ£Όλ©΄ μμ±μ΄λ€.


## 2) λ¦¬ν©ν λ§
Redux-thunk λμ λ§μ°¬κ°μ§λ‘ μ€λ³΅λ μ½λλ₯Ό μ κ±°νμ¬ λ¦¬ν©ν λ§ ν΄λ³΄μ. λ°©μμ λΉμ·νλ€.

```jsx
// lib/createReqeustSaga.js
import { call, put } from 'redux-saga/effects';
import { startLoading, finishLoading } from '../modules/loading';

export default function createReqeustSaga(type, request){
    const SUCCESS = `${type}_SUCCESS`;
    const FAILURE = `${type}_FAILURE`;

    return function*(action){
        yield put(startLoading(type)); // λ‘λ© μμ
        try{
            const response = yield call(request, action.payload);
            yield put({
                type: SUCCESS,
                payload: response.data
            });
        } catch(e){
            yield put({
                type: FAILURE,
                payload: e,
                error: true
            });
        }
        yield put(finishLoading(type)); // λ‘λ© λ
    };
}
```

```jsx {16,17}
// modules/sample.js
import { createAction,handleActions } from 'redux-actions';
import * as api from '../lib/api';
import { takeLatest } from 'redux-saga/effects';
import createReqeustSaga from '../lib/createRequestSaga';

const GET_POST = 'sample/GET_POST';
const GET_POST_SUCCESS = 'sample/GET_POST_SUCCESS';

const GET_USERS = 'sample/GET_USERS';
const GET_USERS_SUCCESS = 'sample/GET_USERS_SUCCESS';

export const getPost = createAction(GET_POST, id => id);
export const getUsers = createAction(GET_USERS);

const getPostSaga = createReqeustSaga(GET_POST, api.getPost);
const getUsersSaga = createReqeustSaga(GET_USERS, api.getUsers);

export function* sampleSaga(){
    yield takeLatest(GET_POST, getPostSaga);
    yield takeLatest(GET_USERS, getUsersSaga);
}

const initialState = {
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
    },
    initialState
);

export default sample;

```


# References

<λ¦¬μ‘νΈλ₯Ό λ€λ£¨λ κΈ°μ  κ°μ ν>(κΉλ―Όμ€, 2019)

<λ¦¬μ‘νΈ μ€μ  νλ‘κ·Έλλ° κ°μ ν>(μ΄μ¬μΉ, 2020)

<λͺ¨λ μλ°μ€ν¬λ¦½νΈ Deep Dive>(μ΄μλͺ¨, 2020)