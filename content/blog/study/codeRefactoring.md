---
title: 'π React Redux μ½λ λ¦¬ν©ν λ§ '
date: 2021-08-20 22:57:13
category: 'study'
thumbnail: 'https://gatsby-blog-images.s3.ap-northeast-2.amazonaws.com/thumb_redux.png'
description: 'μ½λμ§€ λ ν·κ°λ¦¬κΈ° μ¬μ΄ λ¬Έλ²λ€μ μ λ¦¬νλ€'
tags: ['redux-actions', 'Immer','Redux-toolkit']
draft: false
---


# 1. (λͺ¨λ) redux-actions

`redux-actions`λ λ¦¬λμ€μ **μ‘μ μμ± ν¨μ(μ‘μ μμ±μ)**μ **λ¦¬λμ**λ₯Ό μ’ λ κ°νΈνκ² μμ±ν  λ μ¬μ©λλ λΌμ΄λΈλ¬λ¦¬λ€.

## 1) createAction()

```jsx {10,11,12}
import { createAction } from 'redux-actions';

const INCREASE = 'counter/INCREASE';
const DECREASE = 'counter/DECREASE';

// κΈ°μ‘΄ μ‘μ μμ± λ°©μ
export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });

// createAction()μ νμ©ν μ‘μ μμ± λ°©μ
export const increase = createAction(INCREASE);
export const decrease = createAction(DECREASE); 
```

`createAction()`μ νλΌλ―Έν°λ‘ μ‘μ νμμ μ λ¬νμ¬ μ‘μ κ°μ²΄λ₯Ό λ§λ€μ΄λλ€.


μλμ κ°μ΄ `createAction()`μ λ λ²μ§Έ νλΌλ―Έν°λ‘ payload μμ± ν¨μλ₯Ό μ λ¬ν΄μ μ‘μ κ°μ²΄μ payloadλ₯Ό λ§λΆμΌ μ μλ€.

```jsx {5,6,7,8,9}
import {createAction} from 'redux-actions';

const WRITE_POST = 'write/WRITE_POST';

export const writePost = createAction(WRITE_POST, ({ title, body, tags }) => ({
    title,
    body,
    tags,
}));
```

κ·Έλ¬λ©΄ λ€μκ³Ό κ°μ μ‘μ κ°μ²΄κ° μμ±λλ€.

```
{ type: write/WRITE_POST, payload:{title:'', body:'', tags:''}}
```

μ΄λ κ² μμ±λ `writePost` μ‘μ μμ±μλ λ€λ₯Έ μ»΄ν¬λνΈμμ `dispatch(writePost({props λλ state})` ννλ‘ μ¬μ©νλ€.

## 2) handleActions()


```jsx {25,26,27,28,29,30,31,32}
import { createAction, handleActions } from 'redux-actions';

// μ‘μ νμ μ μΈ λ° μ‘μ μμ±μ μλ΅...

const initialState = {
    number: 0
};

// κΈ°μ‘΄ λ¦¬λμ μμ± λ°©μ
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

// handelActions()λ₯Ό νμ©ν λ¦¬λμ μμ± λ°©μ
const counter = handleActions(
  {
    [INCREASE]: (state, action) => ({ number: state.number + 1}),
    [DECREASE]: (state, action) => ({ number: state.number - 1}),
  },
  initialState,
);

```

`handleActions()`μ μ²« λ²μ§Έ νλΌλ―Έν°λ‘ κ° μ‘μμ λν μλ°μ΄νΈ ν¨μλ₯Ό λ£κ³ , λ λ²μ§Έ νλΌλ―Έν°λ‘ μ΄κΈ° μνλ₯Ό μ λ¬νλ€.

payloadλ₯Ό κ°λ μ‘μ κ°μ²΄κ° dispatchλμμ λ `handleActions()`μμ μ΄ payloadλ₯Ό λ€λ£¨λ λ°©λ²μ μλμ κ°λ€.
μμ `createAction()`μΌλ‘ λ§λ  μ‘μ κ°μ²΄κ° payloadλΌλ κ°μ²΄λ‘ title,bodyμ κ°μ μΆκ° λ°μ΄ν°λ₯Ό κ΄λ¦¬νκ³  μμμ νμΈνλ€. λ°λΌμ μ΄ λ°μ΄ν°μ μ κ·ΌνκΈ° μν΄μλ λ²κ±°λ‘­λλΌλ action.payloadμ κ°μ΄ μ¬μ©ν΄μΌ νλ€.


```jsx {14,15,16}
// μ΄κΈ°μν μ μ
const initialState = {
    title: '',
    body: '',
    tags: [],
    post: null,
    postError: null,
    originalPostId: null,
};

const wrtie = handleActions(
  {
    // ν¬μ€νΈ μμ± μ±κ³΅
    [WRITE_POST_SUCCESS]: (state, action}) => ({
        ...state,
        post: action.payload // here!
    }),
  }
);
```

μλμ κ°μ΄ κ°μ²΄ λΉκ΅¬μ‘°ν ν λΉμ ν΅ν΄μ action κ°μ payload μ΄λ¦μ μλ‘ μ€μ ν΄μ£Όλ©΄ action.payloadκ° μ΄λ€ κ°μ μλ―Ένλμ§ μ’ λ μ½κ² νμν  μ μλ€.

```jsx {4,5,6}
const wrtie = handleActions(
  {
    // ν¬μ€νΈ μμ± μ±κ³΅
    [WRITE_POST_SUCCESS]: (state, { payload: post }) => ({
        ...state,
        post,
    }),
  }
);
```


# 2. (λͺ¨λ) Immer

λ¦¬λμμμ μνλ₯Ό μλ°μ΄νΈν  λλ λΆλ³μ±μ μ§μΌμΌ νλ€.
μΌλ°μ μΈ μν©μμλ spread μ°μ°μ(...)μ λ°°μ΄ λ΄μ₯ ν¨μλ₯Ό νμ©ν΄μ λΆλ³μ±μ μ§ν¨λ€.
νμ§λ§ κ°μ²΄μ κΉμ΄κ° κΉμ΄μ§μλ‘ λΆλ³μ±μ μ§ν€κΈ°κ° κΉλ€λ‘μμ§λ€.

```jsx
// {somewhere:{...}, foo:1}
const object = {
  somewhere: {
    deep: {
      inside:3,
      array: [1,2,3,4]
    },
    bar: 2
  },
  foo: 1
};

// somewhere.deep.inside κ°μ 4λ‘ λ°κΎΈκΈ°
let nextObject = {
  ...object,
  somewhere: {
    ...object.somewhere,
    deep: {
      ...object.somewhere.deep,
      inside:4
    }
  }
};
```

μ΄λ κ² μ κ° μ°μ°μλ₯Ό μμ£Ό μ¬μ©ν κ²μ κΈ°μ‘΄μ κ°μ§κ³  μλ λ€λ₯Έ κ°μ μ μ§νλ©΄μ μνλ κ°μ μλ‘ μ§μ νκΈ° μν¨μ΄λ€.
κ·Έλ°λ° μ΄λ κ² μμνλ κ²μ λ²κ±°λ‘­λ€. κ°λμ± λν μ’μ§ μλ€.
immerλ₯Ό μ¬μ©νλ©΄ λ νΈλ¦¬νκ³  κ°λμ± μ’κ² μ½λλ₯Ό μμ±ν  μ μλ€.

`produce()`μ μ²« λ²μ§Έ νλΌλ―Έν°λ μμ νκ³  μΆμ μνμ΄κ³ , 
λ λ²μ§Έ νλΌλ―Έν°λ μνλ₯Ό μ΄λ»κ² μλ°μ΄νΈν μ§ μ μνλ ν¨μλ€.

λ λ²μ§Έ νλΌλ―Έν°λ‘ μ λ¬λλ ν¨μ λ΄λΆμμ κ°μ λ³κ²½νλ©΄,
`produce` ν¨μκ° λΆλ³μ± μ μ§λ₯Ό λμ ν΄ μ£Όλ©΄μ μλ‘μ΄ μνλ₯Ό μμ±ν΄ μ€λ€.
μμλ₯Ό μ΄ν΄λ³΄λλ‘ νμ.

```jsx
import produce from 'immer';

const originalState = [
  {
    id: 1,
    todo: 'μ κ° μ°μ°μμ λ°°μ΄ λ΄μ₯ ν¨μλ‘ λΆλ³μ± μ μ§νκΈ°',
    checked: true,
  },
  {
    id: 2,
    todo: 'immerλ‘ λΆλ³μ± μ μ§νκΈ°',
    checked: false,
  }
];

const nextState = produce(originalState, draft => {
  // idκ° 2μΈ ν­λͺ©μ checked κ°μ trueλ‘ λ³κ²½νκΈ°
  const todo = draft.find(t => t.id === 2); // idκ° 2μΈ ν­λͺ© μ°ΎκΈ°
  todo.check = true;

  // λ°°μ΄μ μλ‘μ΄ λ°μ΄ν° μΆκ°νκΈ°
  draft.push({
    id: 3,
    todo: 'μΌμ  κ΄λ¦¬ μ±μ immer μ μ©νκΈ°',
    checked: false,
  });
})
```

`immer` λΌμ΄λΈλ¬λ¦¬μ ν΅μ¬μ 'λΆλ³μ±μ μ κ²½ μ°μ§ μλ κ²μ²λΌ μ½λλ₯Ό μμ±νλ λΆλ³μ± κ΄λ¦¬λ μ λλ‘ ν΄μ£Όλ κ²'μ΄λ€.

`immer`λ λ³΅μ‘ν λ¦¬λμμ μ½λλ₯Ό κ°λ΅ννλ λ°λ μ μ©νλ€.

```jsx
// Immerλ₯Ό μ¬μ©νμ§ μμ κΈ°μ‘΄ λ¦¬λμ
const todos = handleActions(
  {
    [CHANGE_INPUT]: (state, { payload: input }) => ({ ...state, input}),
    [TOGGLE]: (state, { payload: id }) => ({
      ...state,
      todos: state.todos.map(todo =>
        todo.id === id ? {...todo, done: !todo.done } : todo
      ),
    }),
  },
  initialState,
);

import produce from 'immer';

// Immerλ₯Ό μ¬μ©ν λ¦¬λμ
const todos = handleActions(
  {
    [CHANGE_INPUT]: (state, { payload: input }) =>
      produce(state, draft => {
        draft.input = input;
      }),
    [TOGGLE]: (state, { payload: id }) =>
      produce(state, draft => {
        draft.todos.push(todo);
      }),
  },
  initialState,
);
```

λ€λ§ `[CHANGE_INPUT]`κ³Ό κ°μ΄ μ§§μ μλ°μ΄νΈ ν¨μμ κ²½μ°μλ immerλ₯Ό μ μ©νλ κ²μ΄ μ€νλ € κ°λμ±μ ν΄μΉ  μ μλ€.

# 3. (λͺ¨λ) Redux-toolkit

`Redux Toolkit`μ κΈ°μ‘΄ λ¦¬λμ€ λ‘μ§μ κ°μ μμ΄λ€.
λ¦¬λμ€λ₯Ό μ¬μ©νκΈ° μν΄ μμ±ν΄μΌλ§ νλ λ³΄μΌλ¬ νλ μ΄νΈ μ½λλ₯Ό λν­ μ€μ΄κ³  λ¨μννλ€.
λ¦¬λμ, μ‘μνμ, μ‘μ μμ±μ, μ΄κΈ°μνλ₯Ό `slice`λ‘ ν΅ν©νμ¬ λ¦¬λμ€ μ¬μ©μ νΈλ¦¬νκ² ν΄μ€λ€.
Redux Toolkitμ νμ©νλ©΄ μμ μκ°ν redux-actionsμ Immerκ° νμμμ΄μ§λ€.


```bash
yarn add react-redux @reduxjs/toolkit
```

```jsx
// app/store.js
import { configureStore } from '@reduxjs/toolkit'

export const store = configureStore({
  reducer: {},
})
```
`configureStore()`λ κΈ°μ‘΄ λ¦¬λμ€ `createStore()`μ λͺλͺ κΈ°λ₯μ λν΄μ κ°λ°μμ νΈμλ₯Ό λλͺ¨ν ν¨μλ€.
`configureStore()`λ₯Ό ν΅ν΄μ λ¦¬λμ€ μ€ν μ΄λ₯Ό λ§λ€λ©΄, Redux DevTools extensionμ΄ μλμΌλ‘ νμ±νλλ€.

```jsx {5,6,9}
// index.js
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import { store } from './app/store'
import { Provider } from 'react-redux'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

λ¦¬λμ€ μ€ν μ΄λ₯Ό `<Provider>` μ»΄ν¬λνΈμ λ£μμΌλ‘μ¨ λ¦¬μ‘νΈ νλ‘μ νΈμ λ¦¬λμ€ μ€ν μ΄λ₯Ό μ κ³΅νλ€.

```jsx
// features/counter/counterSlice.js
import { createSlice } from '@reduxjs/toolkit'

const initialState = {
  value: 0,
}

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      // Redux Toolkit allows us to write "mutating" logic in reducers. It
      // doesn't actually mutate the state because it uses the Immer library,
      // which detects changes to a "draft state" and produces a brand new
      // immutable state based off those changes
      state.value += 1
    },
    decrement: (state) => {
      state.value -= 1
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload
    },
  },
})

// Action creators are generated for each case reducer function
export const { increment, decrement, incrementByAmount } = counterSlice.actions

export default counterSlice.reducer
```

`Slice()`μ νλΌλ―Έν°λ‘ μ¬λΌμ΄μ€μ μ΄λ¦, μ΄κΈ° μν, μνλ₯Ό μ΄λ»κ² μλ°μ΄νΈν  κ²μΈμ§μ κ΄ν λ¦¬λμ ν¨μκ° νμνλ€. λ¦¬λμ ν¨μλ ν κ° λλ κ·Έ μ΄μμ΄ μ¬ μ μλ€.

μ¬λΌμ΄μ€κ° μμ±λλ©΄ μ‘μ μμ±μμ λ¦¬λμ ν¨μλ₯Ό λ΄λ³΄λΌ μ μλ€.

λ¦¬λμ€λ μν μλ°μ΄νΈ μ λΆλ³μ±μ μ§μΌμ€μΌ νλ€.
Redux Toolkitμ `createSlice()`μ `createReducer()`λ λ΄λΆμ `Immer`κ° λμνλ€.

```jsx
// app/store.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';

export const store = configureStore({
    reducer: {
        counter: counterReducer,
    },
});

```

μ΄ν `<counterSlice>`μ λ¦¬λμ ν¨μ `counterReducer`λ₯Ό κ°μ Έμ μ€ν μ΄μ μΆκ°νλ€.
μ€ν μ΄μκ² μ΄ slice reducer functionμ μ΄μ©ν΄μ μν μλ°μ΄νΈλ₯Ό κ΄λ¦¬νλΌκ³  λͺλ Ήνλ μμ΄λ€.

`configureSrore()`λ μ¬λ¬κ°μ λ¦¬λμ ν¨μκ° νλΌλ―Έν°λ‘ μ ν΄μ‘μ λ μλμΌλ‘ κ²°ν©(combine)νλ€.

μ΄μ  μ€ν μ΄λ μ΄ λ¦¬λμ ν¨μλ₯Ό ν΅ν΄μ μ μ­μμ μνλ₯Ό μλ°μ΄νΈν  μ μλ€.
λ€λ₯Έ λ¦¬μ‘νΈ μ»΄ν¬λνΈμμ `Redux Hooks`μΈ `useSelector()`, `useDispatch()`λ₯Ό μ¬μ©νμ¬ λ¦¬λμ€ μνμ μ‘μλ€μ μ μ΄ν  μ μλ€.




# 4. (μ»¨νμ΄λ) λ¦¬λμ€ μ€ν μ΄μ μ°λνκΈ°

UI(Presentational Components)μμ λ¦¬λμ€ μ€ν μ΄ λ΄λΆμ μνμ λ¦¬λμλ₯Ό μ¬μ©νκΈ° μν΄μλ μ°κ²°μ΄ νμνλ€.
μ΄ μ°κ²°μ `react-redux`μμ μ κ³΅νλ `connect()` νΉμ `Hooks`μ ν΅ν΄ (Container Componentsμμ) μ΄λ£¨μ΄μ§λ€.

## 1) connect()

`connect(mapStateToProps, mapDispatchToProps)(μ°λν  μ»΄ν¬λνΈ)`

`mapStateToProps`λ λ¦¬λμ€ μ€ν μ΄ λ΄λΆμ μνλ₯Ό μ»΄ν¬λνΈμ propsλ‘ λκ²¨μ£Όλ μ­ν μ ν¨μκ³ ,
`mapDispatchToProps`λ μ‘μ μμ±μ(νΉμ dispatch)λ₯Ό μ»΄ν¬λνΈμ propsλ‘ λκ²¨μ£ΌκΈ° μν΄ μ¬μ©νλ ν¨μλ€.

```jsx
import React from 'react';
import { connect } from 'react-redux';
import Counter from '../components/Counter';
import { increase, decrease } from '../modules/counter';

const CounterContainer = ({ number, increase, decrease }) => {
    return (
        <Counter number={number} onIncrease={increase} onDecrease={decrease} />
    );
};

// numberκ° <CounterContainer>μ propsλ‘ μ λ¬λλ€
const mapStateToProps = state => ({ 
    number: state.counter.number,
});

// increase(), decrease()κ° <CounterContainer>μ propsλ‘ μ λ¬λλ€
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

μλμ κ°μ΄ `connect()` λ΄λΆμμ μ΅λͺ ν¨μ ννλ‘ μ μΈνλ λ°©μλ μλ€.

```jsx
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

μ‘μ μμ± ν¨μλ₯Ό κ°κ° νΈμΆνκ³  dispatchλ‘ κ°μΈλ λ²κ±°λ‘μ΄ μμμ `bindActionCreators` μ νΈ ν¨μλ₯Ό ν΅ν΄μ μλ΅ν  μ μλ€.

```jsx
import { bindActionCreators }  from 'redux';

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

// κ΅³μ΄ bindActionCreators ν¨μλ₯Ό μ μΈνκ³  λͺμν  νμ μμ΄,
// μλμ κ°μ΄ μμ±νλ©΄ μ΄ μμμ connectκ° λμ ν΄ μ€λ€.
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

## 2) react-redux Hooks

`useSelector(μν μ ν ν¨μ)`λ `mapStateToProps()`μ μ μ¬νλ€.
λ¦¬λμ€ μ€ν μ΄ λ΄λΆμ μνλ₯Ό μ»΄ν¬λνΈμ propsλ‘ λκ²¨μ£Όλ μ­ν μ νλ€.

μλ μμ μμλ λ£¨νΈλ¦¬λμμμ `write`λ₯Ό κ°μ Έμ writeμ μνλ€μ λ³Έ μ»΄ν¬λνΈμ propsλ‘ λκ²¨μ£Όκ³  μλ€.

`useDispatch()`λ `mapDispatchToProps()`μ μ μ¬νλ€.
λ¦¬λμ€ μ€ν μ΄ λ΄μ₯ν¨μμΈ dispatchλ₯Ό κ°μ Έμ μ¬μ©ν  μ μκ² ν΄μ€λ€.

```jsx
import React from 'react';
import WriteActionButtons from '../../components/write/WriteActionButtons';
import { useSelector, useDispatch } from 'react-redux';
import { writePost, updatePost } from '../../modules/write';

const WriteActionButtonsContainer = ({ history }) => {
    const dispatch = useDispatch();
    const { title, body, tags, post, postError, originalPostId } = useSelector(
        ({ write }) => ({
            title: write.title,
            body: write.body,
            tags: write.tags,
            post: write.post,
            postError: write.posstError,
            originalPostId: write.originalPostId,
        }),
    );

    // ν¬μ€νΈ λ±λ‘
    const onPublish = () => {
        if (originalPostId) {
            dispatch(updatePost({ title, body, tags, id: originalPostId }));
            return;
        }
        dispatch(
            writePost({
                title,
                body,
                tags,
            }),
        );
    };
};
```

## 3) connectμ Hooksμ μ°¨μ΄

`connect` ν¨μλ₯Ό μ¬μ©νμ¬ μ»¨νμ΄λ μ»΄ν¬λνΈλ₯Ό λ§λ€μμ κ²½μ°,
ν΄λΉ μ»¨νμ΄λ μ»΄ν¬λνΈμ λΆλͺ¨ μ»΄ν¬λνΈκ° λ¦¬λ λλ§λ  λ ν΄λΉ μ»΄ν¬λνΈμ propsκ° λ°λμ§ μμλ€λ©΄ μλμΌλ‘ λ¦¬λ λλ§μ΄ λ°©μ§λμ΄ μ±λ₯μ΄ μ΅μ νλλ€.

νμ§λ§ `useSelector`λ₯Ό μ¬μ©νμ¬ λ¦¬λμ€ μνλ₯Ό μ‘°ννμ λλ μ΄ μ΅μ ν μμμ΄ μλμΌλ‘ μ΄λ£¨μ΄μ§μ§ μλλ€.
React.memoλ₯Ό μ»¨νμ΄λ μ»΄ν¬λνΈμ μ¬μ©νλ λ±μ μ κ²½μ μ¨ μ£Όμ΄μΌ νλ€.
<br>

# References

<λ¦¬μ‘νΈλ₯Ό λ€λ£¨λ κΈ°μ  κ°μ ν>(κΉλ―Όμ€, 2019)

[λ¦¬λμ€ μ μ°κ³  κ³μλμ?][1]

[Redux Toolkit API][2]

[1]:https://ridicorp.com/story/how-to-use-redux-in-ridi/
[2]:https://redux-toolkit.js.org/
