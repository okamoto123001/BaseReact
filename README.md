# BasReact

Reactの開発環境を整える。\
基礎となる部分は目的に関わらず、変わらないと思うので再利用できるような形にしたい。\
Create-React-Appは使用しない。結局webpackなど基礎的な部分を理解しなければならず、Create-React-Appだと何が行われているのかわからない。

使用する主なモジュール
* Typescript
* webpack

Create-React-Appを使用しない理由参考サイト：https://dev.to/nikhilkumaran/don-t-use-create-react-app-how-you-can-set-up-your-own-reactjs-boilerplate-43l0
Webpackやbabelを使うようになった理由参考サイト：https://medium.com/the-node-js-collection/modern-javascript-explained-for-dinosaurs-f695e9747b70

# 作成内容

## 作業開始

npmを開始。\
package.jsonファイルが作成される。\
-yを指定しないと、質問に答えながら設定できる。

```
npm init -y
```

## Gitの設定
Gitを開始。
```
git init
```

.gitignoreを作成し、以下を記述。
```
node_modules
dist
```
* node_modules：モジュールがインストールされるフォルダ。
* dist：ビルドで作成されたものが保存されるフォルダ。ビルド出力先は変更できるので、「build」とかの名前が良い場合は変更する。


## react、react-domのインストール
reactのインストール。
```
npm install react react-dom
```

* react-dom：reactとブラウザのつなぎ役。仮想DOMの差分をDOMに反映する。Webアプリケーション開発でReactを使うなら必須。モバイルアプリケーション開発でReactを使うなら「react-native」になる。\
参考サイト：https://zenn.dev/nkzn/articles/diff-between-react-dom-and-react-native


## webpackのインストール
webpackのインストール。webpackは、JavaScriptのbundlerです。様々なものをstatic assetsにbundleしてくれます。
```
npm install -D webpack webpack-cli webpack-dev-server
```

* webpack：webpackそのものです。
* webpack-cli：webpackをコマンドラインで操作するためのツールです。コマンドで操作することが多いので必須。
* webpack-dev-server：webpackの開発用のサーバー。

### webpackのconfigureファイル（webpack.config.js）の作成

```
npx webpack-cli init
```
上記を実行する質問形式に回答することでwebpack.config.jsが作成できる。\
追加で設定が必要な場合は下記サイトが参考例になる。\
https://createapp.dev/webpack/no-library--babel--typescript


## （babelのインストール）
babelのインストール。旧式のJavaScriptに合わせて、トランスパイルしてくれる。\
IE11にも対応したい場合はインストールする。\
webpackでビルドする場合は下記になる。

* babel公式サイトのインストール方法。Build systemsでWebpackを選択する。
https://babeljs.io/setup

```
npm install --save-dev babel-loader @babel/core
```

* presetsのインストール

presetsとは、Babelが変換処理を行う際に利用するプラグインのコレクション。4種類ある。
```
npm install --save-dev @babel/preset-env @babel/preset-react @babel/preset-typescript
```

* webpack.config.jsのruleを追加
```
{
    test: /\.m?js$/,
    exclude: /node_modules/,
    use: {
        loader: "babel-loader",
        options: {
            presets: ['@babel/preset-env', '@babel/preset-react', '@babel/preset-typescript']
        }
    }
}
```

* babel.config.jsonの作成

babel.config.jsonはbabelの設定ファイル。
webpack.config.jsかどちらかに設定していればよいらしいが、公式な推奨はbabel.config.jsonになるので両方設定しておく。

```
{
  "presets": ["@babel/preset-env", "@babel/preset-react", "@babel/preset-typescript"]
}
```

## Typescriptのインストール

```
npm install --save-dev typescript ts-loader
```

* typescript：typeセットできるJavascript。型定義したものはJavascriptにコンパイルしてくれる。
* ts-loader：


## html-webpack-plugin、mini-css-extract-pluginのインストール

```
npm install --save-dev html-webpack-plugin mini-css-extract-plugin
```

* html-webpack-plugin：bundleするときにhtmlファイルを作成してくれる。
* mini-css-extract-plugin：CSSファイルをJSファイルごとに抽出して作成してくれる


## 実行を試す

### src/index.tsの作成
srcフォルダ（Source Codeフォルダ）は作成する。
srcフォルダ内にはソースコードを保存する。
```
import _ from 'lodash';

function component() {
    const element = document.createElement('div');
  
    // Lodash, currently included via a script, is required for this line to work
    element.innerHTML = _.join(['Hello', 'webpack'], ' ');
  
    return element;
  }
  
document.body.appendChild(component());
```

### src/index.htmlの作成
```
<!DOCTYPE html>
 <html>
   <head>
     <meta charset="utf-8" />
     <title>Getting Started</title>
   </head>
   <body>
    <script src="main.js"></script>
   </body>
 </html>
 ```

### lodashのインストール

lodashは、様々な関数をまとめて提供しているライブラリ。
Reactのベースを作ることとは関係はない。
index.tsで使用しているのでインストールする。
```
npm install --save lodash
```