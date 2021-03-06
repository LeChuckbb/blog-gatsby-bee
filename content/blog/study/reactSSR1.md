---
title: '๐ React SSR(1)'
date: 2021-07-04 23:18:13
category: 'study'
thumbnail: 'https://gatsby-blog-images.s3.ap-northeast-2.amazonaws.com/react_ssr.jpg'
description: 'React CRA ํ๋ก์ ํธ์ SSR์ ๋์ํด๋ณด์'
tags: ['SSR','๋ฐ์ดํฐ ๋ก๋ฉ','์นํฉ']
draft: false
---

*๋ณธ ๊ฒ์๊ธ์ ์ฑ <๋ฆฌ์กํธ๋ฅผ ๋ค๋ฃจ๋ ๊ธฐ์  ๊ฐ์ ํ> 20์ฅ '์๋ฒ ์ฌ์ด๋ ๋ ๋๋ง'๋ฅผ ์ ๋ฆฌํ ๋ด์ฉ์๋๋ค*

# ์๋ฒ ์ฌ์ด๋ ๋ ๋๋ง(SSR)

SSR๊ณผ CSR์ ๋ํ ์ค๋ช์ [์ด์  ๊ธ](https://lechuck.netlify.app/study/CSRSSG)์ ์ฐธ๊ณ ํ๋๋ก ํ๊ณ ,
์ฌ๊ธฐ์๋ ๋ฆฌ์กํธ ํ๋ก์ ํธ์ SSR์ ์ ์ฉํ๋ ๋ฐฉ๋ฒ์ ์ง์คํด๋ณด์.

![](./images/ssr/capture1.JPG)

์ ํ๋ฉด์ `Create-React-App(CRA)`๋ก ๋ง๋  ํ๋ก์ ํธ์ ์คํ ํ๋ฉด์ด๋ค.
`CRA` ํค์๋๋ฅผ ์ด์ฉํด์ ๋ง๋  ํ๋ก์ ํธ๋ root ์๋ฆฌ๋จผํธ๊ฐ ๋น์ด ์๋ ๊ฒ์ ๋ณผ ์ ์๋ค.
์ด๋ ํด๋น ํ์ด์ง๊ฐ ์ฒ์์ ๋น ํ์ด์ง๋ผ๋ ๋ป์ด๊ณ ,
์ดํ์ ํด๋ผ์ด์ธํธ(๋ธ๋ผ์ฐ์ ) ์ธก์์ ์๋ฐ์คํฌ๋ฆฝํธ๊ฐ ์คํ๋๊ณ  ๋ฆฌ์กํธ ์ปดํฌ๋ํธ๊ฐ ๋ ๋๋ง๋๋ฉด์ ์ฌ์ฉ์๊ฐ UI๋ฅผ ์ ํ๋ ๊ตฌ์กฐ๋ค.
์ด์ฒ๋ผ ํด๋ผ์ด์ธํธ ์ธก์์ ๋ ๋๋ง๋๋ ๋ฐฉ์์ `CSR`์ ํน์ง์ด๋ค.

CRA๋ก ๋ง๋  ๋ฆฌ์กํธ ํ๋ก์ ํธ์ `SSR`์ ๊ตฌํํด๋ณด์.

## 1) ํ๋ก์ ํธ ์ค๋นํ๊ธฐ

```bash
yarn create react-app ssr-recipe
cd ssr-recipe
yarn add react-router-dom
```

SSR์ ๊ตฌํํ๊ธฐ ์ ์ `๋ฆฌ์กํธ ๋ผ์ฐํฐ`๋ฅผ ์ฌ์ฉํ์ฌ ๋ผ์ฐํํ  ์ ์๋ ๊ฐ๋จํ ํ๋ก์ ํธ๋ฅผ ๋ง๋ค์ด๋ณด์.

### - UI ์ปดํฌ๋ํธ ๋ง๋ค๊ธฐ
```jsx
// src/components/Red.js
import React from 'react';
import './Red.css';

const Red = () => {
    return <div className="Red">RED</div>
};

export default Red;
```

```jsx
// src/components/Blue.js
import React from 'react';
import './Blue.css';

const Blue = () => {
    return <div className="Blue">BLUE</div>
};

export default Blue;
```

```css
/* src/components/Blue.css */
.Blue{
    background: blue;
    font-size:1.5rem;
    color:white;
    width: 128px;
    height: 128px;
    display: flex;
    align-items: center;
    justify-content: center;
}
```

```css
/* src/components/Red.css */
.Red{
    background: red;
    font-size:1.5rem;
    color:white;
    width: 128px;
    height: 128px;
    display: flex;
    align-items: center;
    justify-content: center;
}
```

```jsx
// src/components/Menu.js
import React from 'react';
import { Link } from 'react-router-dom';
const Menu = () => {
    return (
        <ul>
            <li>
                <Link to="/red">Red</Link>
            </li>
            <li>
                <Link to="/blue">Blue</Link>
            </li>
        </ul>
    );
};

export default Menu;
```

๋นจ๊ฐ์๊ณผ ํ๋์ ๋ฐ์ค๋ฅผ ๋ณด์ฌ์ฃผ๋ ๊ฐ๋จํ Red/Blue ์ปดํฌ๋ํธ์ ๊ฐ ๋งํฌ๋ก ์ด๋ํ  ์ ์๊ฒ ํด์ฃผ๋ Menu ์ปดํฌ๋ํธ๋ฅผ ๋ง๋ ๋ค.

### - ํ์ด์ง ์ปดํฌ๋ํธ ๋ง๋ค๊ธฐ

```jsx
// src/pages/RedPage.js
import React from 'react';
import Red from '../components/Red';

const RedPage = () =>{
    return <Red />;
};

export default RedPage;
```

```jsx
// src/pages/BluePage.js
import React from 'react';
import Blue from '../components/Blue';

const BluePage = () =>{
    return <Blue />;
};

export default BluePage;
```

```jsx
// App.js
// App ์ปดํฌ๋ํธ์์ ๋ผ์ฐํธ ์ค์ ํ๊ธฐ
import './App.css';
import { Route } from 'react-router-dom';
import Menu from './components/Menu';
import RedPage from './pages/RedPage';
import BluePage from './pages/BluePage';

function App() {
  return (
    <div>
      <Menu />
      <hr />
      <Route path="/red" component={RedPage} />
      <Route path="/blue" component={BluePage} />
    </div>
  );
}

export default App;
```

```jsx
// index.js
// ํ๋ก์ ํธ์ ๋ฆฌ์กํธ ๋ผ์ฐํฐ ์ ์ฉํ๊ธฐ
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { BrowserRouter } from 'react-router-dom';

ReactDOM.render(
  <BrowserRouter>
    <React.StrictMode>
    <App />
    </React.StrictMode>
  </BrowserRouter>,
  document.getElementById('root')
);
```
`๋ฆฌ์กํธ ๋ผ์ฐํฐ`๋ฅผ ์ด์ฉํ์ฌ ๋นจ๊ฐ์, ํ๋์ ์ปดํฌ๋ํธ๋ฅผ ํ ๊ธํ  ์ ์๋ ํ์ด์ง๋ฅผ ๋ง๋ค์๋ค.

![](./images/ssr/capture.JPG)

## 2) SSR ๊ตฌํํ๊ธฐ

### - SSR์ฉ entry ๋ง๋ค๊ธฐ
`์ํธ๋ฆฌ(entry)`๋ ์นํฉ์์ ํ๋ก์ ํธ๋ฅผ ๋ถ๋ฌ์ฌ ๋ ๊ฐ์ฅ ๋จผ์  ๋ถ๋ฌ์ค๋ ํ์ผ์ด๋ค.
์๋ฅผ ๋ค์ด CRA๋ก ์์ฑ๋ ๋ฆฌ์กํธ ํ๋ก์ ํธ์์๋ index.js ํ์ผ์ ์ํธ๋ฆฌ๋ก ์ฌ์ฉํ๋ค.
SSR์ ํ  ๋๋ `์๋ฒ๋ฅผ ์ํ ์ํธ๋ฆฌ ํ์ผ`์ ๋ฐ๋ก ์์ฑํด์ผ ํ๋ค.
index.server.js๋ผ๋ ์ด๋ฆ์ผ๋ก ํ์คํธ์ฉ ์ํธ๋ฆฌ ํ์ผ์ ๋ง๋ค์ด๋ณด์.

```jsx
// src/index.server.js
import React from 'react';
import ReactDOMServer from 'react-dom/server';

const html = ReactDOMServer.renderToString(
    <div>Hello Server Side Rendering!</div>
);

console.log(html);
```

**์๋ฒ์์ ๋ฆฌ์กํธ ์ปดํฌ๋ํธ๋ฅผ ๋ ๋๋ง**ํ  ๋๋ `ReactDOMServer`์ `renderToString`์ด๋ผ๋ ํจ์๋ฅผ ์ฌ์ฉํ๋ค.
ReactDOMServer ๊ฐ์ฒด๋ ์ปดํฌ๋ํธ๋ฅผ ์ ์  ๋งํฌ์์ผ๋ก ๋ ๋๋งํ  ๋ ์ฃผ๋ก ์ฐ์ธ๋ค.
renderToString์ ๋งค๊ฐ๋ณ์๋ก ์ ๋ฌ๋ JSX ๊ธฐ๋ฐ์ ์ปดํฌ๋ํธ๋ฅผ HTML ๋ฌธ์์ด๋ก ๋ฐํํ๋ ํจ์๋ค. 


### - SSR์ ์ฉ ์นํฉ ํ๊ฒฝ ์ค์ ํ๊ธฐ

SSR ๊ตฌํ์ ์ํด์ `์นํฉ` ์ค์ ์ ์ปค์คํฐ๋ง์ด์ง ํ๋ค.
CRA๋ก ๋ง๋  ํ๋ก์ ํธ์์๋ ์นํฉ ๊ด๋ จ ์ค์ ์ด ์จ๊ฒจ์ ธ ์์ผ๋ `yarn eject` ๋ช๋ น์ ํตํด ๊บผ๋ด์ฃผ์ด์ผ ํ๋ค.
```bash
git add .
git commit -m "Commit before eject'
yarn eject
```

์์์ ์์ฑํ ์ํธ๋ฆฌ ํ์ผ์ `์นํฉ`์ผ๋ก ๋ถ๋ฌ์์ ๋น๋ํ๋ ค๋ฉด **์๋ฒ ์ ์ฉ ํ๊ฒฝ ์ค์ **์ ๋ง๋ค์ด ์ฃผ์ด์ผ ํ๋ค.
๊ทธ์ ์ ๋จผ์  config ํด๋ ๋ด ๊ธฐ์กด paths.js ํ์ผ์ ์์ ํ๋ค.

```jsx {5,6}
//config/paths.js
(...)
  appNodeModules: resolveApp('node_modules'),
  swSrc: resolveModule(resolveApp, 'src/service-worker'),
  ssrIndexJs: resolveApp('src/index.server.js'), // ์๋ฒ ์ฌ์ด๋ ๋ ๋๋ง ์ํธ๋ฆฌ
  ssrBuild: resolveApp('dist'), // ์นํฉ ์ฒ๋ฆฌ ํ ์ ์ฅ ๊ฒฝ๋ก
  publicUrlOrPath,
};
```

`ssrIndexJs`๋ ๋ถ๋ฌ์ฌ ํ์ผ์ ๊ฒฝ๋ก์ด๊ณ ,
`ssrBuild`๋ ์นํฉ์ผ๋ก ์ฒ๋ฆฌํ ๋ค ๊ฒฐ๊ณผ๋ฌผ์ ์ ์ฅํ  ๊ฒฝ๋ก๋ค.
๋ค์์ผ๋ก `์นํฉ ํ๊ฒฝ ์ค์ ` ํ์ผ์ ์์ฑํ๋ค.
config ํด๋์ webpack.config.server.js ํ์ผ์ ์์ฑํ๋ค.

```jsx
// config/webpack.config.server.js
const paths = require('./paths');

module.exports = {
    mode: 'production', // ํ๋ก๋์ ๋ชจ๋๋ก ์ค์ ํ์ฌ ์ต์ ํ ์ต์๋ค์ ํ์ฑํ
    entry: paths.ssrIndexJs, // ์ํธ๋ฆฌ ๊ฒฝ๋ก
    target: 'node', // node ํ๊ฒฝ์์ ์คํ๋  ๊ฒ์ด๋ผ๋ ์ ์ ๋ช์
    output: {
        path: paths.ssrBuild, // ๋น๋ ๊ฒฝ๋ก
        filename: 'server.js', // ํ์ผ ์ด๋ฆ
        chunkFilename: 'js/[name].chunk.js', // ์ฒญํฌ ํ์ผ ์ด๋ฆ
        publicPath: paths.publicUrlOrPath, // ์ ์  ํ์ผ์ด ์ ๊ณต๋  ๊ฒฝ๋ก
    }
};
```

๋ค์์ผ๋ก `๋ก๋`๋ฅผ ์ค์ ํ๋ค.
**์นํฉ์ ๋ก๋๋ ํ์ผ์ ๋ถ๋ฌ์ฌ ๋ ํ์ฅ์์ ๋ง๊ฒ ํ์ํ ์ฒ๋ฆฌ**๋ฅผ ํด ์ค๋ค.
์๋ฅผ ๋ค์ด ์๋ฐ์คํฌ๋ฆฝํธ๋ babel์ ์ฌ์ฉํ์ฌ ํธ๋์คํ์ผ๋ง์ ํด ์ฃผ๊ณ ,
CSS๋ ๋ชจ๋  CSS ์ฝ๋๋ฅผ ๊ฒฐํฉํด ์ฃผ๊ณ ,
์ด๋ฏธ์ง ํ์ผ์ ๋ค๋ฅธ ๊ฒฝ๋ก์ ๋ฐ๋ก ์ ์ฅํ๊ณ  ๊ทธ ํ์ผ์ ๋ํ ๊ฒฝ๋ก๋ฅผ ์๋ฐ์คํฌ๋ฆฝํธ์์ ์ฐธ์กฐํ  ์ ์๊ฒ ํด์ค๋ค.

```jsx
// config/webpack.config.server.js
const nodeExternals = require('webpack-node-externals');
const paths = require('./paths');
const getCSSModuleLocalIdent = require('react-dev-utils/getCSSModuleLocalIdent');
const webpack = require('webpack');
const getClientEnvironment = require('./env');

const cssRegex = /\.css$/;
const cssModuleRegex = /\.module\.css$/;
const sassRegex = /\.(scss|sass)$/;
const sassModuleRegex = /\.module\.(scss|sass)$/;

const env = getClientEnvironment(paths.publicUrlOrPath.slice(0, -1)); // ํ๊ฒฝ๋ณ์ ์ฃผ์ (3)

module.exports = {
    mode: 'production', // ํ๋ก๋์ ๋ชจ๋๋ก ์ค์ ํ์ฌ ์ต์ ํ ์ต์๋ค์ ํ์ฑํ
    entry: paths.ssrIndexJs, // ์ํธ๋ฆฌ ๊ฒฝ๋ก
    target: 'node', // node ํ๊ฒฝ์์ ์คํ๋  ๊ฒ์ด๋ผ๋ ์ ์ ๋ช์
    output: {
        path: paths.ssrBuild, // ๋น๋ ๊ฒฝ๋ก
        filename: 'server.js', // ํ์ผ ์ด๋ฆ
        chunkFilename: 'js/[name].chunk.js', // ์ฒญํฌ ํ์ผ ์ด๋ฆ
        publicPath: paths.publicUrlOrPath, // ์ ์  ํ์ผ์ด ์ ๊ณต๋  ๊ฒฝ๋ก
    },
    module:{
        rules:[
            {
                oneOf:[
                    // ์๋ฐ์คํฌ๋ฆฝํธ๋ฅผ ์ํ ์ฒ๋ฆฌ
                    // ๊ธฐ์กด webpack.config.js๋ฅผ ์ฐธ๊ณ ํ์ฌ ์์ฑ
                    {
                        test:/\.(js|mjs|jsx|ts|tsx)$/,
                        include: paths.appSrc,
                        loader:require.resolve('babel-loader'),
                        options:{
                            customize: require.resolve(
                                'babel-preset-react-app/webpack-overrides'
                            ),
                            presets:[
                                [
                                    require.resolve('babel-preset-react-app'),
                                    {
                                        runtime: 'automatic',
                                    },
                                ],
                            ],
                            plugins:[
                                [
                                    require.resolve('babel-plugin-named-asset-import'),
                                    {
                                        loaderMap:{
                                            svg:{
                                                ReactComponent:
                                                '@svgr/webpack?-svgo, +titleProp, +ref![path]',
                                            },
                                        },
                                    },
                                ],
                            ],
                            cacheDirectory:true,
                            cacheCompression:false,
                            compact: false,
                        },
                    },
                    // CSS๋ฅผ ์ํ ์ฒ๋ฆฌ
                    {
                        test:cssRegex,
                        exclude: cssModuleRegex,
                        // exportOnlyLocals: true ์ต์์ ์ค์ ํด์ผ ์ค์  css ํ์ผ์ ์์ฑํ์ง ์๋๋ค
                        loader: require.resolve('css-loader'),
                        options:{
                            importLoaders: 1,
                            modules:{
                                exportOnlyLocals: true,
                            },
                        },
                    },
                    // CSS Module์ ์ํ ์ฒ๋ฆฌ
                    {
                        test: cssModuleRegex,
                        loader: require.resolve('css-loader'),
                        options:{
                            importLoaders:1,
                            modules:{
                                exportOnlyLocals:true,
                                getLocalIdent: getCSSModuleLocalIdent,
                            },
                        },
                    },
                    // Sass๋ฅผ ์ํ ์ฒ๋ฆฌ
                    {
                        test:sassRegex,
                        exclude: sassModuleRegex,
                        use: [
                            {
                                loader: require.resolve('css-loader'),
                                options:{
                                    importLoaders:3,
                                    modules:{
                                        exportOnlyLocals:true,
                                    },
                                },
                            },
                            require.resolve('sass-loader'),
                        ],
                    },
                    // Sass + CSS Module์ ์ํ ์ฒ๋ฆฌ
                    {
                        test: sassRegex,
                        exclude: sassModuleRegex,
                        use:[
                            {
                                loader: require.resolve('css-loader'),
                                options:{
                                    importLoaders: 3,
                                    modules: {
                                        exportOnlyLocals: true,
                                        getLocalIdent: getCSSModuleLocalIdent,
                                    },
                                },
                            },
                            require.resolve('sass-loader'),
                        ],
                    },
                    // url-loader๋ฅผ ์ํ ์ค์ 
                    {
                        test: [/\.bmp$/, /\.gif$/, /\.jpe?g$/, /\.png$/],
                        loader: require.resolve('url-loader'),
                        options:{
                            emitFile: false, // ํ์ผ์ ๋ฐ๋ก ์ ์ฅํ์ง ์๋ ์ต์
                            limit: 10000, // ์๋๋ 9.76KB๊ฐ ๋์ด๊ฐ๋ฉด ํ์ผ๋ก ์ ์ฅํ๋๋ฐ
                            // emitFile ๊ฐ์ด false ์ผ๋ ๊ฒฝ๋ก๋ง ์ค๋นํ๊ณ  ํ์ผ์ ์ ์ฅํ์ง ์๋๋ค
                            name: 'static/media/[name].[hash:8].[ext]',
                        },
                    },
                    // ์์์ ์ค์ ๋ ํ์ฅ์๋ฅผ ์ ์ธํ ํ์ผ๋ค์
                    // file-loader๋ฅผ ์ฌ์ฉํ๋ค.
                    {
                        loader: require.resolve('file-loader'),
                        exclude: [/\.(js|mjs|jsx|ts|tsx)$/, /\.html$/, /\.json$/],
                        options:{
                            emitFile: false, // ํ์ผ์ ๋ฐ๋ก ์ ์ฅํ์ง ์๋ ์ต์
                            name: 'static/media/[name].[hash:8].[ext]',
                        },
                    },
                ],
            },
        ],
    },
    resolve: { // node_modules ๋ด๋ถ์ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ๋ถ๋ฌ์ฌ ์ ์๊ฒ ์ค์  (1)
        modules: ['node_modules']
    },
    externals: [ // webpack-node-exteranls ๋ผ์ด๋ธ๋ฌ๋ฆฌ ์ ์ฉํ๊ธฐ (2)
        nodeExternals({
            allowlist: [/@babel/],
        }),
    ],
};
```

(1)์ ํตํด react, react-dom/server ๊ฐ์ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ import ๊ตฌ๋ฌธ์ผ๋ก ๋ถ๋ฌ์ค๋ฉด node_modules์์ ์ฐพ์ ์ฌ์ฉํ  ์ ์๋๋ก ์ค์ ํ๋ค.
๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ๋ถ๋ฌ์ค๋ฉด ๋น๋ํ  ๋ ๊ฒฐ๊ณผ๋ฌผ ํ์ผ ์์ ํด๋น ๋ผ์ด๋ธ๋ฌ๋ฆฌ ๊ด๋ จ ์ฝ๋๊ฐ ํจ๊ป ๋ฒ๋ค๋ง๋๋ค.

`๋ธ๋ผ์ฐ์ `์์ ์ฌ์ฉํ  ๋๋ ๊ฒฐ๊ณผ๋ฌผ ํ์ผ์ ์ฑ ๊ด๋ จ ์ฝ๋ ์ธ์๋ ๋ฆฌ์กํธ ๋ผ์ด๋ธ๋ฌ๋ฆฌ ์ฝ๋๊ฐ ํฌํจ๋์ด์ผ ํ๋ค.
ํ์ง๋ง `์๋ฒ`์์๋ node_modules์์ ๋ฐ๋ก ๋ถ๋ฌ์ ์ฌ์ฉํ  ์ ์๊ธฐ ๋๋ฌธ์ ๊ทธ๋ด ํ์๊ฐ ์๋ค.

๋ฐ๋ผ์ ์๋ฒ๋ฅผ ์ํด ๋ฒ๋ค๋งํ  ๋๋ node_modules์์ ๋ถ๋ฌ์ค๋ ๊ฒ์ ์ ์ธํ๊ณ  ๋ฒ๋ค๋งํ๋๊ฒ์ด ์ข๋ค.
์ด๋ฅผ ์ํด `webpack-node-externals` ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ์ฌ์ฉํ๋ค. 
`yarn add webpack-node-externals` ๋ช๋ น์ด๋ฅผ ํตํด ์ค์น ํ, (2)์ ๊ฐ์ด ์ ์ฉํ๋ค.

์ฝ๋ ์๋จ env (3)์ ๊ฐ์ด `ํ๊ฒฝ๋ณ์`๋ฅผ ์ฃผ์ํ๋ฉด, ํ๋ก์ ํธ ๋ด์์ process.env.NODE_ENV ๊ฐ์ ์ฐธ์กฐํ์ฌ ํ์ฌ ๊ฐ๋ฐ ํ๊ฒฝ์ธ์ง ์๋์ง๋ฅผ ์ฐธ์กฐํ  ์ ์๋ค. ????

### - ๋น๋ ์คํฌ๋ฆฝํธ ์์ฑํ๊ธฐ

๋ฐฉ๊ธ ๋ง๋  `์นํฉ ํ๊ฒฝ ์ค์ `์ ์ฌ์ฉํ์ฌ ํ๋ก์ ํธ๋ฅผ `๋น๋`ํ๋ ์คํฌ๋ฆฝํธ๋ฅผ ์์ฑํด๋ณด์.
scripts ํด๋๋ฅผ ์ด์ด ๋ณด๋ฉด `build.js`๋ผ๋ ํ์ผ์ด ์๋ค.
์ด ์คํฌ๋ฆฝํธ๋ ํด๋ผ์ด์ธํธ์์ ์ฌ์ฉํ  ๋น๋ ํ์ผ์ ๋ง๋๋ ์์์ ํ๋ค.
์ด ์คํฌ๋ฆฝํธ์ ๋น์ทํ ํ์์ผ๋ก ์๋ฒ์์ ์ฌ์ฉํ  ๋น๋ ํ์ผ์ ๋ง๋๋ `build.server.js` ์คํฌ๋ฆฝํธ๋ฅผ ์์ฑํด๋ณด์.

```jsx
// scripts/build.server.js
process.env.BABEL_ENV = 'production';
process.env.NODE_ENV = 'production';

process.on('unhandledRejection', err=>{
    throw err;
});

require('../config/env');
const fs = require('fs-extra');
const webpack = require('webpack');
const config = require('../config/webpack.config.server');
const paths = require('../config/paths');

function build(){
    console.log('Creating server build...');
    fs.emptyDirSync(paths.ssrBuild);
    let compiler = webpack(config);
    return new Promise((resolve, reject) => {
        compiler.run((err, stats) => {
            if(err){
                console.log(err);
                return;
            }
            console.log(stats.toString());
        });
    });
}

build();
```

์ฝ๋๋ฅผ ๋ค ์์ฑํ ํ `node scripts/build.server.js` ๋ช๋ น์ด๋ฅผ ์๋ ฅํด์ `๋น๋`๊ฐ ์๋๋์ง ํ์ธ.
์ดํ `node dist/server.js` ๋ช๋ น์ด๋ฅผ ์๋ ฅํด์ ๊ฒฐ๊ณผ๋ฅผ ํ์ธํ๋ค. 
`entry(index.server.js)` ๋ด์ฉ์ด ์๋์ ๊ฐ์ด ์ถ๋ ฅ๋  ๊ฒ์ด๋ค.

```console
<div data-reactroot="">Hello Server Side Rendering!</div>
```

๋น๋ํ๊ณ  ์คํํ  ๋๋ง๋ค ๊ธด ์ปค๋งจ๋๋ฅผ ์๋ ฅํ๋ ๊ฒ ๋ถํธํ๋ค.
์๋์ ๊ฐ์ด ์ค์ ํ์ฌ ๋ ํธํ๊ฒ ์ฌ์ฉํ  ์ ์๋ค.
`yarn build:server`, 
`yarn start:server`


```json
// package.json
"scripts": {
    (...)
    "start:server": "node dist/server.js",
    "build:server": "node scripts/build.server.js"
  },
```

### - ์๋ฒ ์ฝ๋ ์์ฑํ๊ธฐ
**SSR์ ์ฒ๋ฆฌํ ** ์๋ฒ๋ฅผ ์์ฑํด์ผ ํ๋ค.
Express, Koa, Hapi Node.js ์น ํ๋ ์์ํฌ ์ค์์ `Express`๋ฅผ ์ฌ์ฉํด๋ณด๊ฒ ๋ค.
Express ํ๋ ์์ํฌ๋ ์ฌ์ฉ๋ฅ ์ด ๊ฐ์ฅ ๋๊ณ , ์ถํ ์ ์  ํ์ผ๋ค์ ํธ์คํํ  ๋๋ ์ฝ๊ฒ ๊ตฌํํ  ์ ์๋ค.

`yarn add express`

๋ค์์ผ๋ก ๊ธฐ์กด์ test ์ฉ์ผ๋ก ์์ฑํด ๋์๋ `์ํธ๋ฆฌ ํ์ผ`์ ์์ ํ๋ค.

```jsx
// src/index.server.js
import React from 'react';
import ReactDOMServer from 'react-dom/server';
import express from 'express';
import { StaticRouter } from 'react-router-dom';
import App from './App';

const app = express();

// SSR์ ์ฒ๋ฆฌํ  ํธ๋ค๋ฌ ํจ์
const serverRender = (req, res, next) => {
    // ์ด ํจ์๋ 404์ด ๋ ์ผ ํ๋ ์ํฉ์ 404๋ฅผ ๋์ฐ์ง ์๊ณ  SSR์ ํด ์ค๋ค.
    const context = {};
    const jsx = (
        <StaticRouter location={req.url} context={context}>
            <App />
        </StaticRouter>
    );
    const root = ReactDOMServer.renderToString(jsx); // ๋ ๋๋ง์ ํ๊ณ 
    res.send(root); // ํด๋ผ์ด์ธํธ์๊ฒ ๊ฒฐ๊ณผ๋ฌผ์ ์๋ตํ๋ค.
};

app.use(serverRender);

app.listen(5000, () => {
    console.log('Running on http://localhost:5000');
});

// ๊ธฐ์กด ์ฝ๋
// const html = ReactDOMServer.renderToString(
//     <div>Hello Server Side Rendering!</div>
// );
```

๋ฆฌ์กํธ ๋ผ์ฐํฐ ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ `<StaticRouter>`๊ฐ ์ฌ์ฉ๋ ๊ฒ์ ๋ณผ ์ ์๋ค.
์ด ๋ผ์ฐํฐ ์ปดํฌ๋ํธ๋ ์ฃผ๋ก SSR ์ฉ๋๋ก ์ฌ์ฉ๋๋ ๋ผ์ฐํฐ๋ค.
props๋ก ๋ฃ์ด ์ฃผ๋ location ๊ฐ์ ๋ฐ๋ผ ๋ผ์ฐํํด์ค๋ค.
์ง๊ธ์ req.url์ ๋ฃ์ด ์ฃผ๊ณ  ์๋๋ฐ, req ๊ฐ์ฒด๋ ์์ฒญ์ ๋ํ ์ ๋ณด๋ฅผ ์ง๋๋ค.

`<StaticRouter>`์ context ๊ฐ์ ์ด์ฉํด์ ๋์ค์ ๋ ๋๋งํ ์ปดํฌ๋ํธ์ ๋ฐ๋ผ HTTP ์ํ ์ฝ๋๋ฅผ ์ค์ ํด ์ค ์ ์๋ค.

JS ํ์ผ๊ณผ CSS ํ์ผ์ ์น ํ์ด์ง์ ๋ถ๋ฌ์ค๋ ์ผ์ ์ ์ ๋ฏธ๋ค๋๊ณ , 
SSR์ ํตํด ๋ง๋ค์ด์ง ๊ฒฐ๊ณผ๋ง ๋ณด์ฌ ์ฃผ๋๋ก ์ฒ๋ฆฌํ๋ค. 
์๋ฒ๋ฅผ ๋ค์ ๋น๋ํ๊ณ  ์คํํด ๋ณด์.

```bash
yarn build:server
yarn start:server
```

CSS๊ฐ ์ ์ฉ๋์ง ์์ ๊ฒฐ๊ณผ ํ๋ฉด์ ๋ณผ ์ ์๋ค.
๊ทธ๋ฆฌ๊ณ  ๊ฐ๋ฐ์ ๋๊ตฌ๋ฅผ ์ด์ด์ CSR์ด ์๋ SSR์ด ์ ์ฉ๋ ๊ฒ์ ํ์ธํ๋ค.


![](./images/ssr/capture2.JPG)

### - ์ ์  ํ์ผ ์ ๊ณตํ๊ธฐ

Express์์ ์ด๋ฏธ์ง, CSS ํ์ผ ๋ฐ JS ํ์ผ๊ณผ ๊ฐ์ `์ ์  ํ์ผ`์ ์ ๊ณตํ๊ธฐ ์ํด์๋ `static ๋ฏธ๋ค์จ์ด ํจ์`๋ฅผ ์ฌ์ฉํด์ผ ํ๋ค. 
`express.static()` ํจ์์ ๋งค๊ฐ๋ณ์๋ก ์ ์  ํ์ผ์ด ํฌํจ๋ ํด๋๋ช์ ์ ๋ฌํ๋ฉด ํด๋น ํด๋๋ช์ ํฌํจ๋์ด ์๋ ์ ์  ํ์ผ๋ค์ ์ ๊ทผํ  ์ ์๋ค.
์๋ฅผ ๋ค์ด, public ํด๋์ ์ ์  ํ์ผ์ ์ ๊ทผํ  ํ์๊ฐ ์๋ ๊ฒฝ์ฐ์ ๋ค์๊ณผ ๊ฐ์ด ์์ฑํ๋ค. 

```jsx
app.use(express.static('public'));
```

public ํด๋์ ํฌํจ๋ ์ ์  ํ์ผ์ ์๋์ ๊ฐ์ด ๋ก๋ํ  ์ ์๊ฒ๋๋ค.

```
http://localhost:3000/images/kitten.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/images/bg.png
http://localhost:3000/hello.html
```

์ฐ๋ฆฌ ํ๋ก์ ํธ๋ build ํด๋์ JS, CSS ํ์ผ๋ค์ ์ ๊ทผํ๊ฒ๋ ์ค์ ํด์ฃผ์ด์ผ ํ๋ค. 

```jsx {7,24,25,26,28}
// index.server.js
import React from 'react';
import ReactDOMServer from 'react-dom/server';
import express from 'express';
import { StaticRouter } from 'react-router-dom';
import App from './App';
import path from 'path';

const app = express();

// SSR์ ์ฒ๋ฆฌํ  ํธ๋ค๋ฌ ํจ์
const serverRender = (req, res, next) => {
    // ์ด ํจ์๋ 404์ด ๋ ์ผ ํ๋ ์ํฉ์ 404๋ฅผ ๋์ฐ์ง ์๊ณ  SSR์ ํด ์ค๋ค.
    const context = {};
    const jsx = (
        <StaticRouter location={req.url} context={context}>
            <App />
        </StaticRouter>
    );
    const root = ReactDOMServer.renderToString(jsx); // ๋ ๋๋ง์ ํ๊ณ 
    res.send(root); // ํด๋ผ์ด์ธํธ์๊ฒ ๊ฒฐ๊ณผ๋ฌผ์ ์๋ตํ๋ค.
};

const serve = express.static(path.resolve('./build'),{
    index: false // "/" ๊ฒฝ๋ก์์ index.html์ ๋ณด์ฌ ์ฃผ์ง ์๋๋ก ์ค์ 
});

app.use(serve); // ์์๊ฐ ์ค์ํ๋ค . serverRender ์ ์ ์์นํด์ผ ํ๋ค. 
app.use(serverRender);

app.listen(5000, () => {
    console.log('Running on http://localhost:5000');
});
```

๊ทธ๋ค์์๋ JS์ CSS ํ์ผ์ ๋ถ๋ฌ์ค๋๋ก html์ ์ฝ๋๋ฅผ ์ฝ์ํด ์ฃผ์ด์ผ ํ๋ค.
๋ถ๋ฌ์์ผ ํ๋ **ํ์ผ ์ด๋ฆ์ ๋งค๋ฒ ๋น๋ํ  ๋๋ง๋ค ๋ฐ๋๊ธฐ ๋๋ฌธ**์ ๋น๋ํ๊ณ  ๋์ ๋ง๋ค์ด์ง๋ build ํด๋ ๋ด์ `asset-manifest.json` ํ์ผ์ ์ฐธ๊ณ ํ์ฌ ๋ถ๋ฌ์ค๋๋ก ์์ฑํ๋ค.

```bash
yarn build
```

```json {4,5,7,9}
// build/asset-manifest.json
{
  "files": {
    "main.css": "/static/css/main.dba607d9.chunk.css",
    "main.js": "/static/js/main.f9ec9847.chunk.js",
    "main.js.map": "/static/js/main.f9ec9847.chunk.js.map",
    "runtime-main.js": "/static/js/runtime-main.7297b291.js",
    "runtime-main.js.map": "/static/js/runtime-main.7297b291.js.map",
    "static/js/2.52cc9574.chunk.js": "/static/js/2.52cc9574.chunk.js",
    "static/js/2.52cc9574.chunk.js.map": "/static/js/2.52cc9574.chunk.js.map",
    "static/js/3.2b7ba7c9.chunk.js": "/static/js/3.2b7ba7c9.chunk.js",
    "static/js/3.2b7ba7c9.chunk.js.map": "/static/js/3.2b7ba7c9.chunk.js.map",
    "index.html": "/index.html",
    "static/css/main.dba607d9.chunk.css.map": "/static/css/main.dba607d9.chunk.css.map",
    "static/js/2.52cc9574.chunk.js.LICENSE.txt": "/static/js/2.52cc9574.chunk.js.LICENSE.txt"
  },
  "entrypoints": [
    "static/js/runtime-main.7297b291.js",
    "static/js/2.52cc9574.chunk.js",
    "static/css/main.dba607d9.chunk.css",
    "static/js/main.f9ec9847.chunk.js"
  ]
}
```

์ ์ฝ๋์์ ํ์ด๋ผ์ดํ๋ ํ์ผ๋ค์ html ๋ด๋ถ์ ์ฝ์ํด ์ฃผ์ด์ผ ํ๋ค.
์๋ฒ ์ฝ๋๋ฅผ ์๋์ ๊ฐ์ด ์์ ํด๋ณด์.


```jsx
// src/index.server.js
import React from 'react';
import ReactDOMServer from 'react-dom/server';
import express from 'express';
import { StaticRouter } from 'react-router-dom';
import App from './App';
import path from 'path';
import fs from 'fs';

// asset-manifest.json์์ ํ์ผ ๊ฒฝ๋ก๋ค์ ์กฐํํ๋ค
const manifest = JSON.parse(
    fs.readFileSync(path.resolve('./build/asset-manifest.json'), 'utf8')
);

const chunks = Object.keys(manifest.files)
    .filter(key => /chunk\.js$/.exec(key)) // chunk.js๋ก ๋๋๋ ํค๋ฅผ ์ฐพ์์
    .map(key => `<script src="${manifest.files[key]}"></script>`) // ์คํฌ๋ฆฝํธ ํ๊ทธ๋ก ๋ณํํ๊ณ 
    .join(''); // ํฉ์นจ

function createPage(root){
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
        <link href="${manifest.files['main.css']}" rel="stylesheet" />
    </head>
    <body>
        <noscript>You need to enable JavaScript to run this app.</noscript>
        <div id="root">
            ${root}
        </div>
        <script src="${manifest.files['runtime-main.js']}"></script>
        ${chunks}
        <script src="${manifest.files['main.js']}"></script>
    </body>
    </html>
    `;
}
const app = express();

// SSR์ ์ฒ๋ฆฌํ  ํธ๋ค๋ฌ ํจ์
const serverRender = (req, res, next) => {
    // ์ด ํจ์๋ 404์ด ๋ ์ผ ํ๋ ์ํฉ์ 404๋ฅผ ๋์ฐ์ง ์๊ณ  SSR์ ํด ์ค๋ค.
    const context = {};
    const jsx = (
        <StaticRouter location={req.url} context={context}>
            <App />
        </StaticRouter>
    );
    const root = ReactDOMServer.renderToString(jsx); // ๋ ๋๋ง์ ํ๊ณ 
    res.send(createPage(root)); // ํด๋ผ์ด์ธํธ์๊ฒ ๊ฒฐ๊ณผ๋ฌผ์ ์๋ตํ๋ค.
};

const serve = express.static(path.resolve('./build'),{
    index: false // "/" ๊ฒฝ๋ก์์ index.html์ ๋ณด์ฌ ์ฃผ์ง ์๋๋ก ์ค์ 
});

app.use(serve); // ์์๊ฐ ์ค์ํ๋ค . serverRender ์ ์ ์์นํด์ผ ํ๋ค. 
app.use(serverRender);

app.listen(5000, () => {
    console.log('Running on http://localhost:5000');
});
```

```bash
yarn build
yarn build:server
yarn start:server
```


![](./images/ssr/capture3.JPG)

SSR์ด ์ ์ฉ๋ ํ๋ก์ ํธ๋ ์์ ๊ฐ์ด HTML ๊ตฌ์กฐ๋ฅผ ํ์ธํ  ์ ์์ด์ผ ํ๋ค.

์ด์ ๊ฐ์ด SSR์ ๊ตฌํํ๋ฉด ์ฒซ ๋ฒ์งธ ๋ ๋๋ง(ํ์ด์ง ๋ก๋ฉ)์ด ์๋ฒ์ธก์์ ์ด๋ฃจ์ด์ง๋ฉฐ ๋ก๋ฉ ์๋๊ฐ ๋จ์ถ๋๋ ์ด์ ์ ๊ฐ๋๋ค.
๊ทธ๋ฌ๋ฉด์๋ ์ฒซ ๋ฒ์งธ ๋ ๋๋ง ์ดํ์ ์ฒ๋ฆฌ๋ ๋ธ๋ผ์ฐ์ ์์ ๋ด๋นํ์ฌ ํ์ด์ง ์ด๋์ ๋ฐ์ํ๋ ํ๋ฉด ๊น๋นก์ ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ  ์ ์๊ฒ ๋๋ค.
์ฆ, ์ฒซ ๋ฒ์งธ ๋ ๋๋ง์๋ `SSR`์ด ์ ์ฉ๋๊ณ  ์ดํ์๋ `CSR`์ด ์ ์ฉ๋๋ ๊ฒ์ด๋ค.
์ค์ ๋ก Red๋ Blue๊ฐ์ ๋งํฌ๋ฅผ ํด๋ฆญํ์ฌ ์ด๋ํ  ๋ CSR์ด ์ ์ฉ๋๋ ๋ชจ์ต์ ๋ณผ ์ ์๋ค.(ํ๋ฉด์ด ๊น๋นก์ด์ง ์์) ์ด๋ ๋คํธ์ํฌ ์์ฒญ์ ๋ฐ์์ํค์ง ์๋๋ค.


์ด๋ก์จ ๊ธฐ๋ณธ์ ์ธ SSR์ ์์ฑํ๋ค.

## 3) ๋ฐ์ดํฐ ๋ก๋ฉ

๋ฐ์ดํฐ ๋ก๋ฉ์ SSR ๊ตฌํ ์ ๊น๋ค๋ก์ด ๊ฑธ๋ฆผ๋์ด ๋๋ค.
๋ฐ์ดํฐ ๋ก๋ฉ์ ํ๋ค๋ ๊ฒ์ `API ์์ฒญ`์ ์๋ฏธํ๋ค.
์ผ๋ฐ์ ์ธ ๋ธ๋ผ์ฐ์  ํ๊ฒฝ์์๋ API๋ฅผ ์์ฒญํ๊ณ  ์๋ต์ ๋ฐ์์์ ๋ฆฌ์กํธ state ํน์ ๋ฆฌ๋์ค store์ ๋ฃ์ผ๋ฉด ์๋์ผ๋ก ๋ฆฌ๋ ๋๋ง์ด ๋์ด์ ํฐ ๊ฑฑ์ ์ด ์๋ค.
ํ์ง๋ง ์๋ฒ์ ๊ฒฝ์ฐ `๋ฌธ์์ด ํํ`๋ก ๋ ๋๋งํ๋ ๊ฒ์ด๋ฏ๋ก state๋ store์ ์ํ๊ฐ ๋ฐ๋๋ค๊ณ  ํด์ ์๋์ผ๋ก ๋ฆฌ๋ ๋๋ง๋์ง ์๋๋ค.
๊ทธ ๋์  ์ฐ๋ฆฌ๊ฐ `renderToString` ํจ์๋ฅผ ํ ๋ฒ ๋ ํธ์ถํด ์ฃผ์ด์ผ ํ๋ค. 
(entry์์ renderToStaticMarkup์ ์ฌ์ฉํ ๋ค renderToString์ ๋ค์ ํ ๋ฒ ํธ์ถํ๋ ์์ผ๋ก ์์ฑํ  ์์ )

<!-- ???? -->
SSR ์ ๋ฐ์ดํฐ ๋ก๋ฉ์ ํด๊ฒฐํ๋ ๋ฐฉ๋ฒ์ ์ฌ๋ฌ๊ฐ์ง๊ฐ ์๋ค.
๊ทธ ์ค์์๋ ๊น๋ํ๊ณ  ํธํ ๋ฐฉ๋ฒ์ ์ฌ์ฉํด๋ณด๊ฒ ๋ค.

**redux-thunk, redux-saga ๋ฏธ๋ค์จ์ด๋ฅผ ์ฌ์ฉํ์ฌ API๋ฅผ ํธ์ถํ๋ ํ๊ฒฝ์์ SSR์ ์ ์ฉํ๋ ๋ฐฉ๋ฒ**์ ์์๋ณด์.

### - redux-thunk ์ฝ๋ ์ค๋นํ๊ธฐ

์ฐ์  redux-thunk๋ฅผ ์ฌ์ฉํ์ฌ API ํธ์ถ ํ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ ธ์ค๋ ์ฝ๋๋ฅผ ์์ฑํด๋ณด์.

```bash
yarn add redux react-redux redux-thunk axios
```

```jsx
// src/modeuls/users.js
import axios from 'axios';

const GET_USERS_PENDING = 'users/GET_USERS_PENDING';
const GET_USERS_SUCCESS = 'users/GET_USERS_SUCCESS';
const GET_USERS_FAILURE = 'users/GET_USERS_FAILURE';

const getUsersPending = () => ({ type: GET_USERS_PENDING });
const getUsersSuccess = payload => ({ type: GET_USERS_SUCCESS, payload});
const getUsersFailure = payload => ({
    type: GET_USERS_FAILURE,
    error: true,
    payload
});

export const getUsers = () => async dispatch =>{
    try{
        dispatch(getUsersPending()); 
        const response = await axios.get(
            'https://jsonplaceholder.typicode.com/users'
        );
        dispatch(getUsersSuccess(response));
    } catch (e){
        dispatch(getUsersFailure(e));
        throw e;
    }
};

const initialState = {
    users: null,
    user: null,
    loading: {
        users: false,
        user: false,
    },
    error: {
        users: null,
        user: null
    }
};

function users(state = initialState, action){
    switch(action.type){
        case GET_USERS_PENDING:
            return { ...state, loading: {...state.loading, users: true}};
        case GET_USERS_SUCCESS:
            return{
                ...state,
                loading: {...state.loading, users: false},
                users: action.payload.data
            };
        case GET_USERS_FAILURE:
            return{
                ...state,
                loading: {...state.loading, users: false},
                error: {...state.error, users:action.payload }
            };
        default:
            return state;
    }
}

export default users;
```

์ ๋ชจ๋์์ ์์ฒญํ๋ API ๋ฐ์ดํฐ๋ ์๋์ ๊ฐ๋ค. 


```json
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
  },
  (...)
}
```

๋ชจ๋์ ๋ค ์์ฑํ ๋ค ๋ฃจํธ ๋ฆฌ๋์๋ฅผ ๋ง๋ค๊ณ , Provider ์ปดํฌ๋ํธ๋ฅผ ์ฌ์ฉํ์ฌ ํ๋ก์ ํธ์ ๋ฆฌ๋์ค๋ฅผ ์ ์ฉํ์.

```jsx
// src/modules/index.js
import { combineReducers } from 'redux';
import users from './users';

const rootReducer = combineReducers({ users });
export default rootReducer;
```

```jsx
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import { BrowserRouter } from 'react-router-dom';
import { createStore, applyMiddleware } from 'redux';
import { Provider } from 'react-redux   ';
import thunk from 'redux-thunk';
import rootReducer from './modules';

const store = createStore(rootReducer, applyMiddleware(thunk));

ReactDOM.render(
  <Provider store={store}>
    <BrowserRouter>
      <React.StrictMode>
      <App />
      </React.StrictMode>
    </BrowserRouter>
  </Provider>,
  document.getElementById('root')
);
```

### - Users, UserContainer ์ปดํฌ๋ํธ ์ค๋นํ๊ธฐ

์ด์  ์ฌ์ฉ์์ ๋ํ ์ ๋ณด๋ฅผ ๋ณด์ฌ ์ค ์ปดํฌ๋ํธ๋ฅผ ๋ง๋ค์ด๋ณด์.

```jsx
// src/components/Users.js
import React from 'react';
import { Link } from 'react-router-dom';

const Users = ({ users }) => { // users๋ API๋ก ๋ฐ์์จ ๋ฐ์ดํฐ(JSON)๋ฅผ ์๋ฏธ.
    if(!users) return null; // users๊ฐ ์ ํจํ์ง ์๋ค๋ฉด ์๋ฌด๊ฒ๋ ๋ณด์ฌ์ฃผ์ง ์์
    return (
        <div>
            <ul>
                {users.map(user => (
                    <li key={user.id}>
                        <Link to={`/users/${user.id}`}>{user.username}</Link>
                    </li>
                ))}
            </ul>
        </div>
    );
};

export default Users;
```

```jsx
// src/containers/UsersContainer.js
import React, {useEffect} from 'react';
import Users from '../components/Users'
import { connect } from 'react-redux';
import { getUsers } from '../modules/users';

const UsersContainer = ({ users, getUsers }) => {
    // ์ปดํฌ๋ํธ๊ฐ ๋ง์ดํธ๋๊ณ  ๋์ ํธ์ถ
    useEffect(() => {
        if(users)
            return; // users๊ฐ ์ด๋ฏธ ์ ํจํ๋ค๋ฉด ์์ฒญํ์ง ์์
        getUsers(); // API ํธ์ถ
    }, [getUsers, users]);

    return <Users users={users} />;
};

export default connect(
    state => ({
        users: state.users.users
    }),
    {
        getUsers
    }
)(UsersContainer);
```

SSR ์ ์ฉ์์๋ **์ด๋ฏธ ์๋ ์ ๋ณด๋ฅผ ์ฌ์์ฒญํ์ง ์๊ฒ ์ฒ๋ฆฌํ๋ ์์**์ด ์ค์ํ๋ค.
์ ์ฝ๋์์๋ `useEffect` ์์ if๋ฌธ์ด ๊ทธ ์ญํ ์ ๋งก๊ณ  ์๋ค.
SSR ํ ๋ธ๋ผ์ฐ์ ์์ ํ์ด์ง๋ฅผ ํ์ธํ  ๋ ์ด๋ฏธ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ง๊ณ  ์์์๋ ๋ถํ์ํ API๋ฅผ ํธ์ถํ๋ ๊ฒฝ์ฐ๋ฅผ ๋ฐฉ์งํ๊ธฐ ์ํด์๋ค.

์ปจํ์ด๋ ์ปดํฌ๋ํธ๋ฅผ ๋ชจ๋ ์์ฑํ์ผ๋ฉด ์ด ์ปดํฌ๋ํธ๋ฅผ ๋ณด์ฌ์ค ํ์ด์ง ์ปดํฌ๋ํธ๋ฅผ ๋ง๋ค๊ณ , ๋ผ์ฐํธ ์ค์ ์ ํด ์ฃผ์.

```jsx
// src/pages/UsersPage.js
import React from 'react';
import UsersContainer from '../containers/UsersContainer';

const UsersPage = () => {
    return <UsersContainer />
};

export default UsersPage;
```

```jsx {7,16}
// App.js
import './App.css';
import { Route } from 'react-router-dom';
import Menu from './components/Menu';
import RedPage from './pages/RedPage';
import BluePage from './pages/BluePage';
import UsersPage from './pages/UsersPage';

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

๋ธ๋ผ์ฐ์ ์์ ํด๋ฆญ์ ํตํด /users ๊ฒฝ๋ก๋ก ์ด๋ํ  ์ ์๋๋ก Menu ์ปดํฌ๋ํธ๋ฅผ ์์ ํ์.

```jsx {13,14,15}
// src/components/Menu.js
import React from 'react';
import { Link } from 'react-router-dom';
const Menu = () => {
    return (
        <ul>
            <li>
                <Link to="/red">Red</Link>
            </li>
            <li>
                <Link to="/blue">Blue</Link>
            </li>
            <li>
                <Link to="/users">Users</Link>
            </li>
        </ul>
    );
};

export default Menu;
```

users ๋ฒํผ์ ๋๋ ์ ๋ ์๋์ ๊ฐ์ ํ๋ฉด์ด ๋์จ๋ค.

![](./images/ssr/capture4.JPG)

### - PreloadContext ๋ง๋ค๊ธฐ

ํ์ฌ `getUsers` ํจ์๋ UsersContainer.js์ `useEffect` ๋ถ๋ถ์์ ํธ์ถ๋๊ณ  ์๋ค.
๋ง์ฝ ํด๋์คํ์ผ๋ก ์์ฑํ๋ค๋ฉด componentDidMount ๋ถ๋ถ์์ ํธ์ถํ์ ๊ฒ์ด๋ค.
**ํ์ง๋ง SSR ์์๋ useEffect๋ componentDidMount์์ ์ค์ ํ ์์์ด ํธ์ถ๋์ง ์๋๋ค.**

`๋ ๋๋งํ๊ธฐ ์ ์ API๋ฅผ ์์ฒญํ ๋ค ์คํ ์ด์ ๋ด์์ผ ํ๋๋ฐ`,
์๋ฒ ํ๊ฒฝ์์ ์ด๋ฌํ ์์์ ํ๋ ค๋ฉด ํด๋์คํ ์ปดํฌ๋ํธ๊ฐ ์ง๋๊ณ  ์๋ constructor ๋ฉ์๋๋ฅผ ์ฌ์ฉํ๊ฑฐ๋ render ํจ์ ์์ฒด์์ ์ฒ๋ฆฌํด์ผ ํ๋ค.
๊ทธ๋ฆฌ๊ณ  ์์ฒญ์ด ๋๋  ๋๊น์ง ๋๊ธฐํ๋ค๊ฐ ๋ค์ ๋ ๋๋งํด์ผ ํ๋ค.

์ฐ๋ฆฌ๋ ์ด ์์์ PreloadContext๋ฅผ ๋ง๋ค๊ณ , ์ด๋ฅผ ์ฌ์ฉํ๋ Preloader ์ปดํฌ๋ํธ๋ฅผ ๋ง๋ค์ด์ ์ฒ๋ฆฌํด๋ณด์.

```jsx
// src/lib/PreloadContext.js
import { createContext, useContext } from 'react';

// ํด๋ผ์ด์ธํธ ํ๊ฒฝ: null
// ์๋ฒ ํ๊ฒฝ: { done: false, promise: [] }
const PreloadContext = createContext(null);
export default PreloadContext;

// resolve๋ ํจ์ ํ์์ด๋ค
export const Preloader = ({ resolve }) => {
    const preloadContext = useContext(PreloadContext);
    if (!preloadContext) return null; // context ๊ฐ์ด ์ ํจํ์ง ์๋ค๋ฉด ์๋ฌด๊ฒ๋ ํ์ง ์์
    if (preloadContext.done) return null; // ์ด๋ฏธ ์์์ด ๋๋ฌ๋ค๋ฉด ์๋ฌด๊ฒ๋ ํ์ง ์์

    // promise ๋ฐฐ์ด์ ํ๋ก๋ฏธ์ค ๋ฑ๋ก
    // ์ค๋ น resolve ํจ์๊ฐ ํ๋ก๋ฏธ์ค๋ฅผ ๋ฐํํ์ง ์๋๋ผ๊ณ , ํ๋ก๋ฏธ์ค ์ทจ๊ธ์ ํ๊ธฐ ์ํด
    // Promise.resolve ํจ์ ์ฌ์ฉ
    preloadContext.promise.push(Promise.resolve(resolve()));
    return null;
};
```
<!-- ???? -->
`PreloadContext`๋ SSR ๊ณผ์ ์์ ์ฒ๋ฆฌํด์ผ ํ  ์์๋ค์ ์คํํ๊ณ ,
๋ง์ฝ ๊ธฐ๋ค๋ ค์ผ ํ๋ Promise๊ฐ ์๋ค๋ฉด Promise๋ฅผ ์์งํ๋ค.
๋ชจ๋  Promise๋ฅผ ์์งํ ๋ค, 
์์งํ Promise๋ค์ด ๋๋  ๋๊น์ง ๊ธฐ๋ค๋ ธ๋ค๊ฐ ๊ทธ๋ค์์ ๋ค์ ๋ ๋๋งํ์ฌ ๋ฐ์ดํฐ๊ฐ ์ฑ์์ง ์ํ๋ก ์ปดํฌ๋ํธ๋ค์ด ๋ํ๋๊ฒ ๋๋ค.


`Preloader` ์ปดํฌ๋ํธ๋ resolve๋ผ๋ ํจ์๋ฅผ props๋ก ๋ฐ์ ์ค๋ฉฐ,
์ปดํฌ๋ํธ๊ฐ ๋ ๋๋ง๋  ๋ ์๋ฒ ํ๊ฒฝ์์๋ง resolve ํจ์๋ฅผ ํธ์ถํด ์ค๋ค.
์ด resolve์๋ ์๋์์ ํ์ธํด๋ณผ ์ ์๋ฏ์ด API๋ฅผ ํธ์ถํ๋ `getUsers` ํจ์๊ฐ ๋ด๊ธด๋ค.

<!-- ์๋ ์ฝ๋์์ preloader๋ ๋ฌด์จ ์ญํ ??? -->

```jsx {6,18}
// src/containers/UserContainer.js
import React, {useEffect} from 'react';
import Users from '../components/Users'
import { connect } from 'react-redux';
import { getUsers } from '../modules/users';
import { Preloader } from '../lib/PreloadContext';

const UsersContainer = ({ users, getUsers }) => {
    // ์ปดํฌ๋ํธ๊ฐ ๋ง์ดํธ๋๊ณ  ๋์ ํธ์ถ
    useEffect(() => {
        if(users)
            return; // Users๊ฐ ์ด๋ฏธ ์ ํจํ๋ค๋ฉด ์์ฒญํ์ง ์์
        getUsers(); // API ํธ์ถ
    }, [getUsers, users]);
    return(
        <>
            <Users users={users}/>
            <Preloader resolve={getUsers} />
        </>
    )
};

export default connect(
    state => ({
        users: state.users.users
    }),
    {
        getUsers
    }
)(UsersContainer);
```

### - ์๋ฒ์์ ๋ฆฌ๋์ค ์ค์  ๋ฐ PreloadContext ์ฌ์ฉํ๊ธฐ

์๋ฒ์์ ๋ฆฌ๋์ค๋ฅผ ์ค์ ํ๋ ๋ฐฉ๋ฒ์ ๋ธ๋ผ์ฐ์ ์์ ํ  ๋์ ํฐ ์ฐจ์ด๊ฐ ์๋ค.

```jsx {9,10,11,12,21,23}
// ./index.server.js
import React from 'react';
import ReactDOMServer from 'react-dom/server';
import express from 'express';
import { StaticRouter } from 'react-router-dom';
import App from './App';
import path from 'path';
import fs from 'fs';
import { createStore, applyMiddleware } from 'redux';
import { Provider } from 'react-redux';
import thunk from 'redux-thunk';
import rootReducer from './modules';

(...)
const app = express();

// SSR์ ์ฒ๋ฆฌํ  ํธ๋ค๋ฌ ํจ์
const serverRender = (req, res, next) => {
    // ์ด ํจ์๋ 404์ด ๋ ์ผ ํ๋ ์ํฉ์ 404๋ฅผ ๋์ฐ์ง ์๊ณ  SSR์ ํด ์ค๋ค.
    const context = {};
    const store = createStore(rootReducer, applyMiddleware(thunk));
    const jsx = (
        <Provider store={store}>
            <StaticRouter location={req.url} context={context}>
                <App />
            </StaticRouter>
        </Provider>
    );
    const root = ReactDOMServer.renderToString(jsx); // ๋ ๋๋ง์ ํ๊ณ 
    res.send(createPage(root)); // ํด๋ผ์ด์ธํธ์๊ฒ ๊ฒฐ๊ณผ๋ฌผ์ ์๋ตํ๋ค.
};

const serve = express.static(path.resolve('./build'),{
    index: false // "/" ๊ฒฝ๋ก์์ index.html์ ๋ณด์ฌ ์ฃผ์ง ์๋๋ก ์ค์ 
});

app.use(serve); // ์์๊ฐ ์ค์ํ๋ค . serverRender ์ ์ ์์นํด์ผ ํ๋ค. 
app.use(serverRender);

app.listen(5000, () => {
    console.log('Running on http://localhost:5000');
});
```

์ฌ๊ธฐ์ ์ฃผ์ํ  ์ ์ ์๋ฒ๊ฐ ์คํ๋  ๋ ์คํ ์ด๋ฅผ ํ ๋ฒ๋ง ๋ง๋๋ ๊ฒ์ด ์๋๋ผ, **์์ฒญ์ด ๋ค์ด์ฌ ๋๋ง๋ค ์๋ก์ด ์คํ ์ด๋ฅผ ๋ง๋ ๋ค**๋ ๊ฒ์ด๋ค.

์ด์  `PreloadContext`๋ฅผ ์ฌ์ฉํ์ฌ **ํ๋ก๋ฏธ์ค๋ค์ ์์งํ๊ณ  ๊ธฐ๋ค๋ ธ๋ค๊ฐ ๋ค์ ๋ ๋๋ง**ํ๋ ์์์ ์ํํด๋ณด์.

```jsx {3,7,12,13,14,15,18,27,28,29,30,31,32,33}
// ./index.server.js
(...)
import PreloadContext from './lib/PreloadContext';

(...)
// SSR์ ์ฒ๋ฆฌํ  ํธ๋ค๋ฌ ํจ์
const serverRender = async (req, res, next) => {
    // ์ด ํจ์๋ 404์ด ๋ ์ผ ํ๋ ์ํฉ์ 404๋ฅผ ๋์ฐ์ง ์๊ณ  SSR์ ํด ์ค๋ค.
    const context = {};
    const store = createStore(rootReducer, applyMiddleware(thunk));

    const preloadContext = {
        done: false,
        promise: []
    };
    // .Provider๋ Context์ value๋ฅผ ๋ณ๊ฒฝํ  ๋ ์ฐ์ธ๋ค.
    const jsx = (
        <PreloadContext.Provider value={preloadContext}> 
            <Provider store={store}>
                <StaticRouter location={req.url} context={context}>
                    <App />
                </StaticRouter>
            </Provider>
        </PreloadContext.Provider>
    );

    ReactDOMServer.renderToStaticMarkup(jsx); // renderToStaticMarkup์ผ๋ก ํ๋ฒ ๋ ๋๋งํ๋ค
    try{
        await Promise.all(preloadContext.promise); // ๋ชจ๋  ํ๋ก๋ฏธ์ค๋ฅผ ๊ธฐ๋ค๋ฆฐ๋ค
    } catch (e){
        return res.status(500);
    }
    preloadContext.done = true;
    const root = ReactDOMServer.renderToString(jsx); // ๋ ๋๋ง์ ํ๊ณ 
    res.send(createPage(root)); // ํด๋ผ์ด์ธํธ์๊ฒ ๊ฒฐ๊ณผ๋ฌผ์ ์๋ตํ๋ค.
};

const serve = express.static(path.resolve('./build'),{
    index: false // "/" ๊ฒฝ๋ก์์ index.html์ ๋ณด์ฌ ์ฃผ์ง ์๋๋ก ์ค์ 
});

app.use(serve); // ์์๊ฐ ์ค์ํ๋ค . serverRender ์ ์ ์์นํด์ผ ํ๋ค. 
app.use(serverRender);

app.listen(5000, () => {
    console.log('Running on http://localhost:5000');
});
```

์ฒซ ๋ฒ์งธ๋ก ๋ ๋๋ง์ ํ  ๋๋ `renderToString` ๋์  `renderToStaticMarkup` ์ด๋ผ๋ ํจ์๋ฅผ ์ฌ์ฉํ๋ค.
renderToStaticMarkup์ ๋ฆฌ์กํธ๋ฅผ ์ฌ์ฉํ์ฌ ์ ์ ์ธ ํ์ด์ง๋ฅผ ๋ง๋ค ๋ ์ฌ์ฉํ๋ค.
์ด ํจ์๋ก ๋ง๋  ๋ฆฌ์กํธ ๋ ๋๋ง ๊ฒฐ๊ณผ๋ฌผ์ ํด๋ผ์ด์ธํธ ์ชฝ์์ HTML DOM ์ธํฐ๋์์ ์ง์ํ๊ธฐ ํ๋ค๋ค.

์ง๊ธ ๋จ๊ณ์์ renderToString ๋์  renderToStaticMarkup ํจ์๋ฅผ ์ฌ์ฉํ ์ด์ ๋ ๊ทธ์  Preloader๋ก ๋ฃ์ด ์ฃผ์๋ ํจ์๋ฅผ ํธ์ถํ๊ธฐ ์ํด์๋ค.
๋ ์ด ํจ์์ ์ฒ๋ฆฌ ์๋๊ฐ renderToString๋ณด๋ค ์ข ๋ ๋น ๋ฅด๊ธฐ ๋๋ฌธ์ด๋ค.
renderToStaticMarkup์ renderToString๊ณผ ์ ์ฌํ์ง๋ง ์ข ๋ ์ถ์ฝ๋ ํจ์๋ก ๋ณด์ฌ์ง๋ค.

### - ์คํฌ๋ฆฝํธ๋ก ์คํ ์ด ์ด๊ธฐ ์ํ ์ฃผ์ํ๊ธฐ

์ง๊ธ๊น์ง ์์ฑํ ์ฝ๋๋ API๋ฅผ ํตํด ๋ฐ์ ์จ ๋ฐ์ดํฐ๋ฅผ ๋ ๋๋งํ์ง๋ง,
๋ ๋๋งํ๋ ๊ณผ์ ์์ ๋ง๋ค์ด์ง ์คํ ์ด์ ์ํ๋ฅผ ๋ถ๋ผ์ฐ์ ์์ ์ฌ์ฌ์ฉํ์ง ๋ชปํ๋ ์ํฉ์ด๋ค.
`์๋ฒ์์ ๋ง๋ค์ด ์ค ์ํ๋ฅผ ๋ธ๋ผ์ฐ์ ์์ ์ฌ์ฌ์ฉํ๋ ค๋ฉด`,
ํ์ฌ `์คํ ์ด ์ํ๋ฅผ ๋ฌธ์์ด๋ก ๋ณํ`ํ ๋ค ์คํฌ๋ฆฝํธ๋ก ์ฃผ์ํด ์ฃผ์ด์ผ ํ๋ค.

```jsx {2,21,37,38,39,40}
// ./index.server.js
function createPage(root, stateScript){
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
        <link href="${manifest.files['main.css']}" rel="stylesheet" />
    </head>
    <body>
        <noscript>You need to enable JavaScript to run this app.</noscript>
        <idv id="root">
            ${root}
        </div>
        ${stateScript}
        <script src="${manifest.files['runtime-main.js']}"></script>
        ${chunks}
        <script src="${manifest.files['main.js']}"></script>
    </body>
    </html>
    `;
}
const app = express();

// SSR์ ์ฒ๋ฆฌํ  ํธ๋ค๋ฌ ํจ์
const serverRender = async (req, res, next) => {
    (...)
    const root = ReactDOMServer.renderToString(jsx); // ๋ ๋๋ง์ ํ๊ณ 
    // JSON ๋ฌธ์์ด๋ก ๋ณํํ๊ณ  ์์ฑ ์คํฌ๋ฆฝํธ๊ฐ ์คํ๋๋ ๊ฒ์ ๋ฐฉ์งํ๊ธฐ ์ํด <๋ฅผ ์นํ ์ฒ๋ฆฌ
    // https://redux.js.org/recipes/server-rendering#security-considerations
    const stateString = JSON.stringify(store.getState()).replace(/</g, '\\u003c');
    // ๋ฆฌ๋์ค ์ด๊ธฐ ์ํ๋ฅผ ์คํฌ๋ฆฝํธ๋ก ์ฃผ์ํ๋ค
    const stateScript = `<script>__PRELOADED_STATE__ = ${stateString}</script>`; 
    res.send(createPage(root, stateScript)); // ํด๋ผ์ด์ธํธ์๊ฒ ๊ฒฐ๊ณผ๋ฌผ์ ์๋ตํ๋ค.
};

(...)
```

๋ธ๋ผ์ฐ์ ์์ ์ํ๋ฅผ ์ฌ์ฌ์ฉํ  ๋๋ ์๋์ ๊ฐ์ด ์คํ ์ด ์์ฑ ๊ณผ์ ์์ window.__PRELOAD_STATE__๋ฅผ ์ด๊น๊ฐ์ผ๋ก ์ฌ์ฉํ๋ฉด ๋๋ค.

```jsx {6}
// ./index.js
(...)

const store = createStore(
  rootReducer,
  window.__PRELOADED_STATE__, // ์ด ๊ฐ์ ์ด๊ธฐ ์ํ๋ก ์ฌ์ฉํจ
  applyMiddleware(thunk)
);

(...)
```

```bash
yarn build
yarn build:server
yarn start:server
```

์๋์ ๊ฐ์ ๊ฒฐ๊ณผ๋ฅผ ํ์ธํ  ์ ์๋ค.

![](./images/ssr/capture5.JPG)

<!-- ### - redux-saga ์ฝ๋ ์ค๋นํ๊ธฐ

```bash
yarn add redux-saga
```

์ ๋ช๋ น์ด๋ก redux-saga๋ฅผ ์ค์นํ ํ, 
users ๋ฆฌ๋์ค ๋ชจ๋์์ redux-saga๋ฅผ ์ฌ์ฉํ์ฌ ํน์  ์ฌ์ฉ์์ ์ ๋ณด๋ฅผ ๊ฐ์ ธ์ค๋ ์์์ ๊ด๋ฆฌํด๋ณด์.

```jsx {3,9,10,11,12,22,23,24,25,26,27,28,29,44,45,46,48,49,50,51,52,53,54,55,56,57,58,60,61,62,63,94,95,96,97,98,99,100,101,102,103,104,105,106,107,108,109,110,111,112}
// modules/users.js
import axios from 'axios';
import { call, put, takeEvery } from 'redux-saga/effects';

const GET_USERS_PENDING = 'users/GET_USERS_PENDING';
const GET_USERS_SUCCESS = 'users/GET_USERS_SUCCESS';
const GET_USERS_FAILURE = 'users/GET_USERS_FAILURE';

// ์๋ก์ด ์ก์ ํ์ ์์ฑ
const GET_USER = 'users/GET_USER'
const GET_USER_SUCCESS = 'users/GET_USER_SUCCESS';
const GET_USER_FAILURE = 'users/GET_USER_FAILURE';

const getUsersPending = () => ({ type: GET_USERS_PENDING });
const getUsersSuccess = payload => ({ type: GET_USERS_SUCCESS, payload});
const getUsersFailure = payload => ({
    type: GET_USERS_FAILURE,
    error: true,
    payload
});

// ์๋ก์ด ์ก์ ์์ฑ์
export const getUser = id => ({ type: GET_USER, payload: id });
const getUserSuccess = data => ({type: GET_USER_SUCCESS, payload: data});
const getUserFailure = error => ({
    type: GET_USER_FAILURE,
    payload: error,
    error: true
});

export const getUsers = () => async dispatch =>{
    try{
        dispatch(getUsersPending()); 
        const response = await axios.get(
            'https://jsonplaceholder.typicode.com/users'
        );
        dispatch(getUsersSuccess(response));
    } catch (e){
        dispatch(getUsersFailure(e));
        throw e;
    }
};

// ํน์  ์ฌ์ฉ์์ ์ ๋ณด๋ฅผ ๊ฐ์ ธ์ค๋ ๋น๋๊ธฐ ์์
const getUserById = id =>
    axios.get(`https://jsonplaceholder.typicode.com/users/${id}`);

// ์ ๋ค๋ ์ดํฐ ํจ์(saga)
function* getUserSaga(action){
    try{
        // call์ ์ฌ์ฉํ์ฌ Promise๋ฅผ ๋ฐํํ๋ ํจ์ getUserById๋ฅผ ํธ์ถํ๋ค. ์ด๋ action.payload๋ฅผ ํ๋ผ๋ฏธํฐ๋ก ์ ๋ฌํ๋ค.
        // put์ ํตํด ์ก์์ ๋์คํจ์นํ๋ค.
        const response = yield call(getUserById, action.payload);
        yield put(getUserSuccess(response.data));
    } catch(e){
        yield put(getUserFailure(e));
    }
}

export function* usersSaga(){
    // takeEvery๋ ๋ค์ด์ค๋ ๋ชจ๋  GET_USER ์ก์์ ๋ํด getUserSaga ํจ์๋ฅผ ์คํํ๋๋ก ์ฒ๋ฆฌํ๋ค.
    yield takeEvery(GET_USER, getUserSaga);
}

const initialState = {
    users: null,
    user: null,
    loading: {
        users: false,
        user: false,
    },
    error: {
        users: null,
        user: null
    }
};

function users(state = initialState, action){
    switch(action.type){
        case GET_USERS_PENDING:
            return { ...state, loading: {...state.loading, users: true}};
        case GET_USERS_SUCCESS:
            return{
                ...state,
                loading: {...state.loading, users: false},
                users: action.payload.data
            };
        case GET_USERS_FAILURE:
            return{
                ...state,
                loading: {...state.loading, users: false},
                error: {...state.error, users:action.payload }
            };
        // ๊ธฐ์กด ๋ฆฌ๋์์ GET_USER, GET_USER_SUCCESS, GET_USER_FAILURE ์ถ๊ฐ
        case GET_USER:
            return{
                ...state,
                loading: { ...state.loading, user:true},
                error: {...state.error, user:null}
            };
        case GET_USER_SUCCESS:
            return {
                ...state,
                loading: {...state.loading, user: false},
                user: action.payload
            };
        case GET_USER_FAILURE:
            return{
                ...state,
                loading: { ...state.loading, user: false},
                error: { ...state.error, user: action.payload }
            };
        default:
            return state;
    }
}

export default users;
```

๋ชจ๋ ์์ ์ด ๋๋ฌ์ผ๋ฉด ๋ฃจํธ ์ฌ๊ฐ๋ฅผ ๋ง๋ค์.

```jsx {4,6,7,8,9,10}
// modules/index.js
import { combineReducers } from 'redux';
import users, {usersSaga} from './users';
import { all } from 'redux-saga/effects';

export function* rootSaga(){
    // all ํจ์๋ฅผ ์คํํ์ฌ ์ฌ๋ฌ ์ฌ๊ฐ๋ฅผ ํฉ์น๋ค.
    // ์ ๋ค๋ ์ดํฐ ํจ์๋ฅผ ๋ฐฐ์ด์ ํํ๋ก ๋ฃ์ด์ฃผ์ด ์ ๋ค๋ ์ดํฐ ํจ์๋ค์ด ๋ณํ์ ์ผ๋ก ๋์์ ์คํ๋๊ณ , ์ ๋ถ resolve๋  ๋๊น์ง ๊ธฐ๋ค๋ฆฌ๋ ๊ฒ.
    yield all([usersSaga()]);
}

const rootReducer = combineReducers({ users });
export default rootReducer;
```

๋ฆฌ๋์ค ์คํ ์ด๋ฅผ ์์ฑํ  ๋ redux-saga ๋ฏธ๋ค์จ์ด๋ฅผ ์ ์ฉํ์.

```jsx {10,11,13,18,21}
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import { BrowserRouter } from 'react-router-dom';
import { createStore, applyMiddleware } from 'redux';
import { Provider } from 'react-redux';
import thunk from 'redux-thunk';
import rootReducer, { rootSaga } from './modules';
import createSagaMiddleware from 'redux-saga';

const sagaMiddleware = createSagaMiddleware();

const store = createStore(
  rootReducer,
  window.__PRELOADED_STATE__, // ์ด ๊ฐ์ ์ด๊ธฐ ์ํ๋ก ์ฌ์ฉํจ
  applyMiddleware(thunk, sagaMiddleware)
);

sagaMiddleware.run(rootSaga);

ReactDOM.render(
  <Provider store={store}>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </Provider>,
  document.getElementById('root')
);
```

### - User, UserContainer ์ปดํฌ๋ํธ ์ค๋นํ๊ธฐ

์ด์  ํน์  ์ฌ์ฉ์์ ์ ๋ณด๋ฅผ ๋ณด์ฌ ์ค User ์ปดํฌ๋ํธ๋ฅผ ๋ง๋ค์.

```jsx
// components/User.js
import React from 'react';

const User = ({ user }) => {
    const { email, name, username } = user;
    return (
        <div>
            <h1>
                {username} ({name})
            </h1>
            <p>
                <b>e-mail:</b> {email}
            </p>
        </div>
    );
};

export default User;
```

user ๊ฐ์ด null์ธ์ง ๊ฐ์ฒด์ธ์ง ํ์ธํ๋ ์ ํจ์ฑ ๊ฒ์ฌ ๊ด๋ จ ์ฝ๋๋ฅผ ์ถ๊ฐํด์ผ ํ๋ค.
์ ํจ์ฑ ๊ฒ์ฌ๋ ์ปจํ์ด๋ ์ปดํฌ๋ํธ์์ ์ํํ๋ค.

### - redux-saga๋ฅผ ์ํ SSR ์์ -->

<br>
<br>

---

PreloadContext, renderToString ๋ฑ์ ๋ํด ์ ์ดํดํ์ง ๋ชปํ๋ค.
๋ถ์กฑํ ๋ถ๋ถ์ ๋์ค์ ๊ผญ ๋ณด๊ฐํ๊ธฐ๋ก ํ์.


<!-- ### - redux-saga ์ฝ๋ ์ค๋นํ๊ธฐ

### - User, UserContaienr ์ปดํฌ๋ํธ ์ค๋นํ๊ธฐ

### - redux-saga๋ฅผ ์ํ SSR ์์ -->

<!-- ### - usePreloader Hook ๋ง๋ค์ด์ ์ฌ์ฉํ๊ธฐ -->

# References

[ReactDOMServer ๋ฌธ์](https://ko.reactjs.org/docs/react-dom-server.html)

[Express ์ ์  ํ์ผ ๋ฌธ์](https://expressjs.com/ko/starter/static-files.html)

