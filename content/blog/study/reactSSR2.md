---
title: 'π React SSR(2) - μ½λ μ€νλ¦¬ν'
date: 2021-07-05 16:57:13
category: 'study'
thumbnail: 'https://gatsby-blog-images.s3.ap-northeast-2.amazonaws.com/react_ssr.jpg'
description: 'SSRμ μ½λ μ€νλ¦¬ν μ μ©νκΈ°'
tags: ['React.lazy','λμ  import','Loadable Components']
draft: false
---

# μ½λ μ€νλ¦¬ν

λλΆλΆμ React μ±μ `Webpack`, `Rollup` λλ `Browserify` κ°μ ν΄μ μ¬μ©νμ¬ 
μ¬λ¬ νμΌμ νλλ‘ λ³ν©ν `λ²λ€ λ` νμΌμ μΉ νμ΄μ§μ ν¬ν¨νμ¬ ν λ²μ μ μ²΄ μ±μ λ‘λνλ λ°©μμ΄λ€.
CRA, Next.js νΉμ Gatsby κ°μ λκ΅¬λ₯Ό μ¬μ©νλ νλ‘μ νΈλΌλ©΄ Webpackμ΄ μ€μΉλμ΄ μ¬λ¬ νμΌμ νλμ νμΌλ‘ `λ²λ€λ§` νκ³  μμ κ²μ΄λ€.

A, B, C νμ΄μ§λ‘ κ΅¬μ±λ SPAλ₯Ό κ°λ°νλ€κ³  κ°μ νμ.
μ¬μ©μκ° A νμ΄μ§μ λ°©λ¬Ένλ€λ©΄ B νμ΄μ§μ C νμ΄μ§μμ μ¬μ©νλ μ»΄ν¬λνΈ μ λ³΄λ νμνμ§ μλ€.
μ¬μ©μκ° B νΉμ C νμ΄μ§λ‘ μ΄λνλ €κ³  ν  λλ§ νμνλ€.

νμ§λ§ λ¦¬μ‘νΈ νλ‘μ νΈμ λ³λλ‘ μ€μ νμ§ μμΌλ©΄ A,B,C μ»΄ν¬λνΈμ λν μ½λκ° λͺ¨λ ν νμΌμ μ μ₯(λ²λ€)λμ΄ λ²λ¦°λ€.
λ§μ½ μ±μ κ·λͺ¨κ° ν¬λ€λ©΄ μ§κΈ λΉμ₯ νμνμ§ μμ μ»΄ν¬λνΈ μ λ³΄λ λͺ¨λ λΆλ¬μ€λ©΄μ **νμΌ ν¬κΈ°κ° λ§€μ° μ»€μ§λ€.**
κ·Έλ¬λ©΄ λ‘λ©λ μ€λ κ±Έλ¦¬κ³  μ¬μ©μ κ²½νλ μ μ’μμ§κ³  νΈλν½λ λ§μ΄ λμ€κ² λλ€.

λ²λ€μ΄ λΉλν΄μ§λ κ²μ λ°©μ§νλ μ’μ λ°©λ²μ `λ²λ€μ λλλ` κ²μ΄λ€.
`μ½λ μ€νλ¦¬ν`μ ***λ°νμμ μ¬λ¬ λ²λ€μ λμ μΌλ‘ λ§λ€κ³  λΆλ¬μ€λ κ²***μΌλ‘ Webpack, Rollup, Browserifyκ°μ `λ²λ€λ¬`κ° μ§μνλ κΈ°λ₯μ΄λ€.
μ½λ μ€νλ¦¬νμ ν΅ν΄ μ°λ¦¬ μ±μ `μ§μ° λ‘λ©`νκ³  μ± μ¬μ©μμκ² νκΈ°μ μΈ μ±λ₯ ν₯μμ μκ²¨μ€ μ μλ€.
μ±μ μ½λ μμ μ€μ΄μ§ μκ³ λ **μ¬μ©μκ° νμνμ§ μμ μ½λλ₯Ό λΆλ¬μ€μ§ μκ² νλ©° μ±μ μ΄κΈ°ν λ‘λ©μ νμν λΉμ©μ μ€μ¬μ€λ€.**

<br>

---
**μ§μ° λ‘λ©(lazy loadig)**μ΄λ μ΄κΈ° νμ΄μ§ λ‘λ© μμ μ΄ μλ, νλ©΄μ λ³΄μ¬μ§ λ λ‘λ©μ μμνκ²λ ν΄μ£Όλ κΈ°λ²μ΄λ€.
μ£Όλ‘ νμ΄μ§ μ΄κΈ° λ‘λ© μ±λ₯μ λ§μ μμν₯μ λΌμΉλ μ΄λ―Έμ§μ μ§μ° λ‘λ©μ μ μ©νλ€.
---

<br>


μ±μ μ½λ μ€νλ¦¬νμ λμνλ λ°©λ²μ ν¬κ² μΈ κ°μ§λ‘ λΆλ₯ν  μ μλ€.

<!-- lazy Loading -> λ²λ€ νμΌμ ν¬κΈ°κ° ν° κ²½μ°μλ μλ΅ μλκ° λλ¦¬λ€.
λμ  μν¬νΈ + prefect, preloadλ₯Ό μ΄μ©ν΄μ lazy Loadingμ λ¨μ μ λ³΄μ.
prefetchλ κ°κΉμ΄ λ―Έλμ νμν νμΌμ΄λΌκ³  λΈλΌμ°μ μκ² μλ €μ£Όλ κ². (λΈλΌμ°μ κ° λ°μμ§ μμ λ λ―Έλ¦¬ λ€μ΄λ‘λλλ€.) -> lazy Loadingμ λλ¦° μλ΅ μλλ₯Ό λ³΄μ
preloadλ μ§κΈ λΉμ₯ νμν νμΌμ΄λΌκ³  λΈλΌμ°μ μκ² μλ¦¬λ κ². (μ²« νμ΄μ§ λ‘λ© μ μ¦μ λ€μ΄λλ€.) -> preload λ¨λ° μ μ²« νμ΄μ§ λ‘λ©μ΄ λλ €μ§ μ μλ€. -->
## 1) λμ  importλ₯Ό ν΅ν μ½λ λΆν 
λ³Έκ²©μ μΈ λ¦¬μ‘νΈ μ»΄ν¬λνΈ μ½λ μ€νλ¦¬νμ μμ μΌλ° μλ°μ€ν¬λ¦½νΈ ν¨μλ₯Ό μ€νλ¦¬νν΄λ³΄μ.

```javascript
// src/notify.js
export default function notify(){
    alert('μλνμΈμ!');
}
```

```jsx
// src/App.js
import React from 'react';
import logo from './logo.svg';
import './App.css';
import notify from './notify';

function App(){
    const onClick = () => {
        notify();
    };
    return(
        <div className="App">
            <header className="App-header">
                <img src={logo} className="App-logo" alt="logo" />
                <p onClick={onClick}>Hello React!</p>
            </header>
        </div>
    );
}

export default App;
```

![](./images/ssr/capture6.JPG)

Hello React! λ¬Έκ΅¬λ₯Ό ν΄λ¦­νλ©΄ notify ν¨μκ° μ€νλμ΄ alertμ μΆλ ₯νλ κ°λ¨ν κ΅¬μ‘°λ€.
μ΄λ κ² μ½λλ₯Ό μμ±νκ³  λΉλνλ©΄ notify.jsμ μ½λ λν νλμ λ²λ€ νμΌ(main.chunk.js)μ ν¬ν¨λλ€.

νμ§λ§ μλμ κ°μ΄ μ½λ μλ¨μμ importλ₯Ό μ μ μΈ ννλ‘ μ μΈνμ§ μκ³ , import() ν¨μ ννλ‘ λ©μλ μμμ μ¬μ©νλ©΄ νμΌμ λ°λ‘ λΆλ¦¬μμΌμ μ μ₯νλ€.
κ·Έλ¦¬κ³  μ€μ  ν¨μκ° νμν μ§μ μ νμΌμ λΆλ¬μμ ν¨μλ₯Ό μ¬μ©ν  μ μλ€.
μ΄λ₯Ό `λμ  import`λΌ νλ€.


```jsx {8}
// src/App.js
import React from 'react';
import logo from './logo.svg';
import './App.css';

function App(){
    const onClick = () => {
        import('./notify').then(result => result.default());
    };
    return(
        <div className="App">
            <header className="App-header">
                <img src={logo} className="App-logo" alt="logo" />
                <p onClick={onClick}>Hello React!</p>
            </header>
        </div>
    );
}

export default App;
```

![](./images/ssr/capture7.JPG)

importλ₯Ό ν¨μ ννλ‘ μ¬μ©νλ©΄ Promiseλ₯Ό λ°ννλ€.
μ΄ ν¨μλ₯Ό ν΅ν΄μ λͺ¨λμ λΆλ¬μ¬ λ notify.jsμ κ°μ΄ defaultλ‘ λ΄λ³΄λΈ λͺ¨λμ result.default()μ κ°μ΄ μ°Έμ‘°ν΄μ μ¬μ©νλ€.

λμ  importλ₯Ό μ μ©νκΈ° λλ¬Έμ Hello React!λΌλ λ¬Έκ΅¬λ₯Ό ν΄λ¦­νλ©΄ κ·Έμ μμΌ notify.jsλ₯Ό importνκΈ° μμνλ€.
μ΄λ λ²λ€λ§ μ΄νμ νν΄μ§λ λμμ΄κΈ° λλ¬Έμ notify κ΄λ ¨ μ½λλ λ²λ€ νμΌμ ν¬ν¨λμ§ μκ³  0.chunkλΌλ μλ‘μ΄ μλ°μ€ν¬λ¦½νΈ νμΌμ ν¬ν¨λ κ²μ νμΈν  μ μλ€.


## 2) React.lazyμ Suspenseλ₯Ό ν΅ν μ½λ λΆν 

λ¦¬μ‘νΈ λ΄μ₯ μ νΈ ν¨μμΈ `React.lazy`μ μ»΄ν¬λνΈμΈ `Suspense`μΌλ‘ μ½λ μ€νλ¦¬νμ ν  μ μλ€.
μ΄λ λ¦¬μ‘νΈ 16.6 λ²μ μμ λμλ κΈ°λ₯μΌλ‘, μ΄μ μλ import ν¨μλ₯Ό ν΅ν΄ λΆλ¬μ¨ λ€μ μ»΄ν¬λνΈ μμ²΄λ₯Ό stateμ λ£λ λ°©μμΌλ‘ κ΅¬νν΄μΌ νμλ€.

```jsx
// SplitMe
import React from 'react';

const SplitMe = () => {
    return <div>SplitMe</div>;
};

export default SplitMe;
```

```jsx {5,17,18,19}
// App.js
import React, {useState, Suspense} from 'react';
import logo from './logo.svg';
import './App.css';
const SplitMe = React.lazy(() => import('./SplitMe.js'));

function App(){
    const [visible, setVisible] = useState(false);
    const onClick = () => {
      setVisible(true);
    };
    return(
        <div className="App">
            <header className="App-header">
                <img src={logo} className="App-logo" alt="logo" />
                <p onClick={onClick}>Hello React!</p>
                <Suspense fallback={<div>loading...</div>}>
                  {visible && <SplitMe />}
                </Suspense>
            </header>
        </div>
    );
}

export default App;
```

Hello React! λ¬Έκ΅¬λ₯Ό ν΄λ¦­νλ©΄ visible μνκ° trueκ° λμ΄ `<SplitMe>` lazy μ»΄ν¬λνΈλ₯Ό λ λλ§νλ κ΅¬μ‘°λ€.

`React.lazy`λ `λμ  import()`λ₯Ό νΈμΆνλ ν¨μλ₯Ό μΈμλ‘ κ°λλ€.
μ΄ ν¨μλ React μ»΄ν¬λνΈλ₯Ό ν¬ν¨νλ©° default exportλ₯Ό κ°μ§ λͺ¨λλ‘ κ²°μ λλ Promiseλ‘ λ°νν΄μΌ νλ€.
λμ  importλ Promiseλ₯Ό λ°ννκΈ° λλ¬Έμ React.lazy ν¨μμ μΈμλ‘ μ λ¬λ  μ μλ κ²μ΄λ€.

`lazy μ»΄ν¬λνΈ`μΈ SpliteMeλ `<Suspense>` μ»΄ν¬λνΈ νμμμ λ λλ§λμ΄μΌ νλ€.
`<Suspense>`λ λ¦¬μ‘νΈ λ΄μ₯ μ»΄ν¬λνΈλ‘μ μ½λ μ€νλ¦¬νλ μ»΄ν¬λνΈλ₯Ό λ‘λ©νλλ‘ λ°λμν¬ μ μκ³ ,
`fallback props`λ₯Ό ν΅ν΄ lazy μ»΄ν¬λνΈκ° λ‘λλκΈΈ κΈ°λ€λ¦¬λ λμ(λ‘λ© μ€μ) λ³΄μ¬μ€ UIλ₯Ό μ€μ ν  μ μλ€.

μ μ½λλ₯Ό μ€νν΄λ³΄λ©΄ λμ  import λμ λ§μ°¬κ°μ§λ‘ onClick μ΄λ²€νΈκ° λ°μνμ λ chunk νμΌμ΄ μκ²¨λκ³ ,
κ·Έ chunk νμΌμ SplitMe μ½λκ° λΆν λμ΄ μλ κ²μ νμΈν  μ μλ€.
λν κ°λ°μ λκ΅¬μμ λ€νΈμν¬ μλλ₯Ό μμ£Ό λλ¦¬κ² μ§μ ν λ€ onClick μ΄λ²€νΈλ₯Ό λ°μμν€λ©΄ loading...μ΄λ λ¬Έκ΅¬κ° μ μ μΆλ ₯λλκ²μ λ³Ό μ μλ€.

## 3) Loadable Componentsλ₯Ό ν΅ν μ½λ λΆν 
`Loadable Components`λ μ½λ μ€νλ¦¬νμ νΈνκ² νλλ‘ λμμ£Όλ μλνν° λΌμ΄λΈλ¬λ¦¬λ€.
`React.lazy`μ `Suspense`λ SSRμ μ§μνμ§ μμ§λ§ μ΄ λΌμ΄λΈλ¬λ¦¬λ `SSR`μ μ§μνλ€.
λν λ λλ§νκΈ° μ μ νμν  λ μ€νλ¦¬νλ νμΌμ λ―Έλ¦¬ λΆλ¬μ¬ μ μλ κΈ°λ₯λ μλ€.

```bash
yarn add @loadable/component
```

```jsx {5,6,19}
// app.js
import React, {useState} from 'react';
import logo from './logo.svg';
import './App.css';
import loadable from '@loadable/component';
const SplitMe = loadable(() => import('./SplitMe'));
// const SplitMe = React.lazy(() => import('./SplitMe.js'));

function App(){
    const [visible, setVisible] = useState(false);
    const onClick = () => {
      setVisible(true);
    };
    return(
        <div className="App">
            <header className="App-header">
                <img src={logo} className="App-logo" alt="logo" />
                <p onClick={onClick}>Hello React!</p>
                {visible && <SplitMe />}
            </header>
        </div>
    );
}

export default App;
```

μ¬μ©λ²μ React.lazyμ μ μ¬νλ€.
Suspenseλ₯Ό μ¬μ©νμ§ μκΈ° λλ¬Έμ λ κ°κ²°ν΄μ‘λ€.
Suspenseμ fallback κΈ°λ₯μ μ¬μ©νμ¬ λ‘λ© μ€μ λ€λ₯Έ UIλ₯Ό μΆλ ₯νκ³  μΆλ€λ©΄ μλμ κ°μ΄ μμ±νλ©΄ λλ€.

```jsx
const SplitMe = loadable(() => import('./SplitMe'), {
    fallback: <div>loading...</div>
});
```

loadable componentλ₯Ό μ΄μ©νλ©΄ μ»΄ν¬λνΈλ₯Ό λ―Έλ¦¬ λΆλ¬μ¬(preload)μλ μλ€.

```jsx {16,17,18,23}
// app.js
import React, {useState} from 'react';
import logo from './logo.svg';
import './App.css';
import loadable from '@loadable/component';

const SplitMe = loadable(() => import('./SplitMe'), {
  fallback: <div>loading...</div>
});

function App(){
    const [visible, setVisible] = useState(false);
    const onClick = () => {
      setVisible(true);
    };
    const onMouseOver = () => {
      SplitMe.preload();
    }
    return(
        <div className="App">
            <header className="App-header">
                <img src={logo} className="App-logo" alt="logo" />
                <p onClick={onClick} onMouseOver={onMouseOver}>Hello React!</p>
                {visible && <SplitMe />}
            </header>
        </div>
    );
}

export default App;
```

Hello React! λ¬Έκ΅¬ μμ λ§μ°μ€ μ»€μλ₯Ό μ¬λ¦¬κΈ°λ§ ν΄λ λ‘λ©μ΄ μμλλ€. (chunk νμΌμ΄ μκΈ΄λ€)
κ·Έλ¦¬κ³  ν΄λ¦­νλ©΄ λ λλ§λλ€.

## 4) SSR + μ½λ μ€νλ¦¬ν μ λνλλ μΆ©λ
SSRκ³Ό μ½λ μ€νλ¦¬νμ ν¨κ» μ μ©νλ κ² μ½μ§ μλ€.
λ³λμ νΈν μμ μμ΄ λ κΈ°μ μ ν¨κ» μ μ©νλ©΄ λ€μκ³Ό κ°μ νλ¦μΌλ‘ μλνλ©΄μ νμ΄μ§μ κΉλ°μμ΄ λ°μνλ€.

1. μλ² μ¬μ΄λ λ λλ§λ κ²°κ³Όλ¬Όμ΄ λΈλΌμ°μ μ λνλ¨ 
2. μλ°μ€ν¬λ¦½νΈ νμΌ λ‘λ© μμ
3. μλ°μ€ν¬λ¦½νΈκ° μ€νλλ©΄μ μμ§ λΆλ¬μ€μ§ μμ μ»΄ν¬λνΈλ₯Ό nullλ‘ λ λλ§ν¨
4. νμ΄μ§μμ μ½λ μ€νλ¦¬νλ μ»΄ν¬λνΈλ€μ΄ μ¬λΌμ§.
5. μ½λ μ€νλ¦¬νλ μ»΄ν¬λνΈλ€μ΄ λ‘λ©λ μ΄ν μ λλ‘ λνλ¨

μμ κ°μ λ¬Έμ  ν΄κ²°μ μν΄μλ λΌμ°νΈ κ²½λ‘λ§λ€ **μ½λ μ€νλ¦¬νλ νμΌ μ€μμ νμν λͺ¨λ  νμΌμ** λΈλΌμ°μ μμ λ λλ§νκΈ° μ μ λ―Έλ¦¬ λΆλ¬μμΌ νλ€.
μ¬κΈ°μλ `Loadable Components` λΌμ΄λΈλ¬λ¦¬μμ μ κ³΅νλ κΈ°λ₯(μΉν© + babel νλ¬κ·ΈμΈ)μ μ¨μ SSR ν νμν νμΌμ κ²½λ‘λ₯Ό μΆμΆνμ¬ λ λλ§ κ²°κ³Όμ `μ€ν¬λ¦½νΈ/μ€νμΌ νκ·Έ`λ₯Ό μ½μνλ λ°©λ²μΌλ‘ λ¬Έμ λ₯Ό ν΄κ²°ν  κ²μ΄λ€.

# SSRκ³Ό μ½λ μ€νλ¦¬ν
μμ λ§νλ― λ¦¬μ‘νΈ λ΄μ₯ μ½λ μ€νλ¦¬ν κΈ°λ₯μΈ `React.lazy`μ `Suspense`λ SSRμ μ§μνμ§ μλλ€.
λ°λΌμ `Loadable Components` λΌμ΄λΈλ¬λ¦¬λ₯Ό μ¬μ©νλ€.

Loadable Componentsμμ SSRμ νμ©νκΈ° μν΄ νμν μλ² μ νΈ ν¨μ, μΉν© νλ¬κ·ΈμΈ, babel νλ¬κ·ΈμΈμ μ€μΉνλ€.
[μ΄μ  κΈ][1]μμ μ¬μ©νλ CRA + SSR νλ‘μ νΈλ₯Ό κ°μ Έμ μ¬μ©νλ€.

```bash
yarn add @loadable/component @loadable/server @loadable/webpack-plugin @loadable/babel-plugin 
```

## 1) λΌμ°νΈ μ»΄ν¬λνΈ μ€νλ¦¬ννκΈ°

νμ¬ νλ‘μ νΈμμ λΌμ°νΈλ₯Ό μν΄ μ¬μ©νκ³  μλ BluePage, RedPage, UserPageλ₯Ό μλμ κ°μ΄ μ€νλ¦¬ννμ.

```jsx {}
// App.js
import './App.css';
import { Route } from 'react-router-dom';
import Menu from './components/Menu';
import loadable from '@loadable/component';
const RedPage = loadable(() => import('./pages/RedPage'));
const BluePage = loadable(() => import('./pages/BluePage'));
const UsersPage = loadable(() => import('./pages/UsersPage'));
// import RedPage from './pages/RedPage';
// import BluePage from './pages/BluePage';
// import UsersPage from './pages/UsersPage';

function App() {
  return (
    <div>
      <Menu />
      <hr />
      <Route path="/red" component={RedPage} />
      <Route path="/blue" component={BluePage} />
      <Route path="/users" component={UsersPage} />
    </div>
  );
}

export default App;
```

μ΄ν νλ‘μ νΈλ₯Ό λΉλνκ³  SSR μλ²λ μ¬μμνλ€.
κ·Έλ¦¬κ³  κ°λ°μ λκ΅¬μ Network ν­μμ μΈν°λ· μλλ₯Ό Slow 3Gλ‘ μ νν ν μλ‘κ³ μΉ¨νμ λ μ΄λ€ νμμ΄ μΌμ΄λλμ§λ₯Ό μ§μΌλ³΄μ.
μ²μμ νμ΄μ§κ° λνλ¬λ€κ° μ΄λ΄ μ¬μλ¦¬κ³ , λ λ€μ λνλλ κ²μ λ³Ό μ μλ€.
μ΄κ²μ΄ μμ μΈκΈνλ SSRμ μ½λ μ€νλ¦¬νμ μ μ©νμ λ λ°μνλ λ¬Έμ λ€.

## 2) μΉν©κ³Ό babel νλ¬κ·ΈμΈ μ μ©
Loadable Componentsμμ μ κ³΅νλ `μΉν©`κ³Ό `babel νλ¬κ·ΈμΈ`μ μ μ©νλ©΄ ***κΉλΉ‘μ νμ***μ ν΄κ²°ν  μ μλ€.

λ¨Όμ  babel νλ¬κ·ΈμΈμ μ μ©ν΄λ³΄μ.
paackage.jsonμ μ΄μ΄μ babel λΆλΆμ μ°Ύμ λ€ λ€μκ³Ό κ°μ΄ μμ νλ€.

```json
// package.json
(...)
"babel": {
    "presets": [
      "react-app"
    ],
    "plugins":[
      "@loadable/babel-plugin"
    ]
  }
```

μ΄ν webpack.config.jsλ₯Ό μ΄μ΄μ μλ¨μμ LoadablePluginμ λΆλ¬μ€κ³  νλ¨μμλ pluginsλ₯Ό μ°Ύμμ ν΄λΉ νλ¬κ·ΈμΈμ μ μ©νλ€.

```jsx {2}
// webpack.config.js
const LoadablePlugin = require('@loadable/webpack-plugin');
(...)
    plugins: [
        new LoadablePlugin(),
        // Generates an 'index.html' file with the <script> injectd.
        new HTMLWebpackPlugin(),
        (...)
    ].filter(Boolean),
    (...)
```

μ΄ν yarn buildλ₯Ό μ€ννκ³  build λλ ν λ¦¬μ `loadable-stats.json`μ΄λΌλ νμΌμ΄ μκ²Όλμ§λ₯Ό νμΈνλ€.
μ΄ νμΌμ κ° μ»΄ν¬λνΈμ μ½λκ° μ΄λ€ μ²­ν¬(chunk) νμΌμ λ€μ΄κ° μλμ§μ λν μ λ³΄λ₯Ό κ°κ³  μλ€.
SSRμ ν  λ μ΄ νμΌμ μ°Έκ³ νμ¬ μ΄λ€ μ»΄ν¬λνΈκ° λ λλ§λμλμ§μ λ°λΌ ***μ΄λ€ νμΌλ€μ μ¬μ μ λΆλ¬μμΌ ν μ§*** μ€μ ν  μ μλ€. 


## 3) νμν μ²­ν¬ νμΌ κ²½λ‘ μΆμΆνκΈ°
SSR ν λΈλΌμ°μ μμ μ΄λ€ νμΌμ μ¬μ μ λΆλ¬μμΌ ν μ§ μμλ΄κ³  ν΄λΉ νμΌλ€μ κ²½λ‘λ₯Ό μΆμΆνκΈ° μν΄ 
`Loadable Componenets`μμ μ κ³΅νλ `ChunkExtractor`μ `ChunkExtractorManage`λ₯Ό μ¬μ©νλ€.

μλ² μνΈλ¦¬ μ½λλ₯Ό μλμ κ°μ΄ μμ νμ.

```jsx
// index.server.js
(...)
import { ChunkExtractor, ChunkExtractorManager } from '@loadable/server';

const statsFile = path.resolve('./build/loadable-stats.json');

// asset-manifest.jsonμμ νμΌ κ²½λ‘λ€μ μ‘°ννλ€
const manifest = JSON.parse(
    fs.readFileSync(path.resolve('./build/asset-manifest.json'), 'utf8')
);

const chunks = Object.keys(manifest.files)
    .filter(key => /chunk\.js$/.exec(key)) // chunk.jsλ‘ λλλ ν€λ₯Ό μ°Ύμμ
    .map(key => `<script src="${manifest.files[key]}"></script>`) // μ€ν¬λ¦½νΈ νκ·Έλ‘ λ³ννκ³ 
    .join(''); // ν©μΉ¨

function createPage(root, tags){
    return `<!DOCTYPE html>
    <html lang="en"
    <head>
        <meta charset="utf-8" />
        <link rel="shortcut icon" href="/favicon.ico" />
        <meta
            name="viewport"
            content="width=device-width, initial-scale=1,shrink-to-fit=no"
        />
        <meta name="theme-color" content="#000000" />
        <title>React App</title>
        ${tags.styles}
        ${tags.links}
    </head>
    <body>
        <noscript>You need to enable JavaScript to run this app.</noscript>
        <div id="root">
            ${root}
        </div>
        ${tags.scripts}
    </body>
    </html>
    `;
}
const app = express();

// SSRμ μ²λ¦¬ν  νΈλ€λ¬ ν¨μ
const serverRender = async (req, res, next) => {
    // μ΄ ν¨μλ 404μ΄ λ μΌ νλ μν©μ 404λ₯Ό λμ°μ§ μκ³  SSRμ ν΄ μ€λ€.
    const context = {};
    const sagaMiddleware = createSagaMiddleware();

    const store = createStore(
        rootReducer,
        applyMiddleware(thunk, sagaMiddleware)
    );

    const sagaPromise = sagaMiddleware.run(rootSaga).toPromise();

    const preloadContext = {
        done: false,
        promise: []
    };

    // νμν νμΌμ μΆμΆνκΈ° μν ChunkExtractor
    const extractor = new ChunkExtractor({ statsFile });

    const jsx = (
        <ChunkExtractorManager extractor={extractor}>
            <PreloadContext.Provider value={preloadContext}>
                <Provider store={store}>
                    <StaticRouter location={req.url} context={context}>
                        <App />
                    </StaticRouter>
                </Provider>
            </PreloadContext.Provider>
        </ChunkExtractorManager>
    );

    ReactDOMServer.renderToStaticMarkup(jsx); // renderToStaticMarkupμΌλ‘ νλ² λ λλ§νλ€
    store.dispatch(END); // redux-sagaμ END μ‘μμ λ°μμν€λ©΄ μ‘μμ λͺ¨λν°λ§νλ μ¬κ°λ€μ΄ λͺ¨λ μ’λ£λλ€.
    try{
        await sagaPromise; // κΈ°μ‘΄μ μ§ν μ€μ΄λ μ¬κ°λ€μ΄ λͺ¨λ λλ  λκΉμ§ κΈ°λ€λ¦°λ€.
        await Promise.all(preloadContext.promise); // λͺ¨λ  νλ‘λ―Έμ€λ₯Ό κΈ°λ€λ¦°λ€.
    } catch (e){
        return res.status(500);
    }
    preloadContext.done = true;
    const root = ReactDOMServer.renderToString(jsx); // λ λλ§μ νκ³ 
    // JSON λ¬Έμμ΄λ‘ λ³ννκ³  μμ± μ€ν¬λ¦½νΈκ° μ€νλλ κ²μ λ°©μ§νκΈ° μν΄ <λ₯Ό μΉν μ²λ¦¬
    // https://redux.js.org/recipes/server-rendering#security-considerations
    const stateString = JSON.stringify(store.getState()).replace(/</g, '\\u003c');
    // λ¦¬λμ€ μ΄κΈ° μνλ₯Ό μ€ν¬λ¦½νΈλ‘ μ£Όμνλ€
    const stateScript = `<script>__PRELOADED_STATE__ = ${stateString}</script>`; 
    
    // λ―Έλ¦¬ λΆλ¬μμΌ νλ μ€νμΌ/μ€ν¬λ¦½νΈλ₯Ό μΆμΆνκ³ 
    const tags={
        scripts: stateScript + extractor.getScriptTags(), // μ€ν¬λ¦½νΈ μλΆλΆμ λ¦¬λμ€ μν λ£κΈ°
        links: extractor.getLinkTags(),
        styles: extractor.getStyleTags()
    };

    res.send(createPage(root, tags)); // ν΄λΌμ΄μΈνΈμκ² κ²°κ³Όλ¬Όμ μλ΅νλ€.
};

const serve = express.static(path.resolve('./build'),{
    index: false // "/" κ²½λ‘μμ index.htmlμ λ³΄μ¬ μ£Όμ§ μλλ‘ μ€μ 
});

app.use(serve); // μμκ° μ€μνλ€ . serverRender μ μ μμΉν΄μΌ νλ€. 
app.use(serverRender);

app.listen(5000, () => {
    console.log('Running on http://localhost:5000');
});
```




## 4) loadableReadyμ hydrate

```jsx
// index.js
(...)
import { loadableReady } from '@loadable/component';

const sagaMiddleware = createSagaMiddleware();

const store = createStore(
  rootReducer,
  window.__PRELOADED_STATE__, // μ΄ κ°μ μ΄κΈ° μνλ‘ μ¬μ©ν¨
  composeWithDevTools(applyMiddleware(thunk, sagaMiddleware))
);

sagaMiddleware.run(rootSaga);

// κ°μ λ΄μ©μ μ½κ² μ¬μ¬μ©ν  μ μλλ‘ λ λλ§ν  λ΄μ©μ νλμ μ»΄ν¬λνΈλ‘ λ¬Άμ
const Root = () => {
  return (
    <Provider store={store}>
      <BrowserRouter>
        <App />
      </BrowserRouter>
    </Provider>
  );
};

const root = document.getElementById('root');

// νλ‘λμ νκ²½μμλ loadableReadyμ hydrateλ₯Ό μ¬μ©νκ³ 
// κ°λ° νκ²½μμλ κΈ°μ‘΄ λ°©μμΌλ‘ μ²λ¦¬
if(process.env.NODE_ENV === 'production'){
  loadableReady(() => {
    ReactDOM.hydrate(<Root />,root);
  });
} else {
  ReactDOM.render(<Root />, root);
}
```

μλμ κ°μ΄ λ λλ§ κ²°κ³Όλ¬Όμ μ²­ν¬ νμΌμ΄ μ£Όμλ κ²μ λ³Ό μ μλ€.

![](./images/ssr/capture8.JPG)



# References

[React κ³΅μ νμ΄μ§ : μ½λ λΆν ](https://ko.reactjs.org/docs/code-splitting.html)

[webpack κ³΅μ νμ΄μ§ : μ½λ μ€νλ¦¬ν](https://webpack.kr/guides/code-splitting/)

[μΉ μ±λ₯ μ΅μ νλ₯Ό μν Image Lazy Loading κΈ°λ²](https://helloinyong.tistory.com/297)

[1]:https://lechuck.netlify.app/study/reactSSR1
