# tsconfigとwebpackの設定

# next.jsの環境構築
- 虎ハックさんのgit hubリンク
`https://github.com/deatiger/ts-basic-demo/tree/bug-husky-not-working`

> 作業用フォルダにを作成  
> 作業用フォルダに移動  
`cd 作業用フォルダ`

-------------------これいる？？-----------------------------------

- npmの初期化
> ターミナルで以下こコマンドを実行  
`npm init`
これで`package.json`ファイルが作成される

- パッケージのインストール
> ターミナルで以下こコマンドを実行  
`npm install --save-dev typescript ts-loader webpack webpack-cli webpack-dev-server`
これで`package-lock.json`と`node_modules`が作成される

-------------------↑↑↑↑↑↑↑---------------------------------

# webpackの設定
- `"/"`ディレクトリにファイルを作成
`webpack.config.js`
> `webpack.config.js`に以下の内容を記述  
```js
const path = require('path');

module.exports = {
  entry: {
    bundle: './src/index.ts'
  },
  output: {
    path: path.join(_dirname, 'dist'),
    flename: '[name].js'
  },
  resolve: {
    extensions: ['.ts', '.js']
  },
  devServer: {
    contentBase: path.join(_dirname, 'dist'),
    open: true
  },
  module: {
    rules: [
      {
        loader: 'ts-loader',
        test: /\.ts$/
      }
    ]
  }
}
```

- `package.json`の編集
```json
{
  "name": "ts-basic",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1", //ここを消す
    "build": "webpack --mode=production", //追加
    "start": "webpack-cil serve --mode development" //追加
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "ts-loader": "^9.2.5",
    "typescript": "^4.4.2",
    "webpack": "^5.51.1",
    "webpack-cli": "^4.8.0",
    "webpack-dev-server": "^4.1.0"
  }
}
```

# `TypeScript`の`tsconfig`ファイルの作成
> ターミナルで以下こコマンドを実行  
`tsc --init`
これで`tsconfig.json`が作成される

> `tsc: command not found`となる場合↓  
`npm i typescript -g`
> 再度`tsc --init`を実行  

> tsconfigの基本的な設定
<!-- tsconfigの写真 -->

------------------------------------------------------------------
- プロジェクトの作成方法
> 作業用ディレクトリに移動し、以下のコマンドを実行し、プロジェクトを作成  
> 以下はトラハックさんが使ってるコマンド  
`npx create-next-app nextjs-blog --use-npm --example "https://github.com/vercel/next-learn-starter/tree/master/dynamic-routes-starter"`

> 以下はドキュメントに記載のコマンドでプロジェクト作成(なぜかeslintが入ってない)  
`npx create-next-app nextjs-blog --use-npm --example "https://github.com/vercel/next-learn-starter/tree/master/learn-starter"`

> 以下はyarnコマンドでのプロジェクト作成   
`yarn create next-app --typescript`

------------------------------------------------------------------

- ESLintとPrettierのCI環境を構築
> 作業用プロジェクトで、以下のコマンドを実行でパッケージをインストール  
`npm install --save-dev eslint eslint-config-prettier prettier @typescript-eslint/parser @typescript-eslint/eslint-plugin husky lint-staged`

- パッケージ内容
<img src="info.png">

- prettierの設定
> ルートディレクトリ下に`.prettierrc`ファイルを作成する  
> `.prettierrc`の内容に以下を記入する  
```json
{
  "printWidth": 120, //自動で改行してくれる文字数
  "singleQuote": true, //シングルクォートを使用する
  "semi": false, //セミコロンを自動で入れる
}
```

- ESLintの設定
> ルートディレクトリ下に`.eslintrc.js`ファイルを作成する  
> `.eslintrc.js`の内容に以下を記入する  
```js
module.exports = {
  env: {
    browser: true,
    es6: true
  },
  extends: [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended", // TypeScriptでチェックされる項目をLintから除外する設定
    "prettier", // prettierのextendsは他のextendsより後に記述する
    "prettier/@typescript-eslint",
  ],
  plugins: ["@typescript-eslint"],
  parser: "@typescript-eslint/parser",
  parserOptions: {
    "sourceType": "module",
    "project": "./tsconfig.json" // TypeScriptのLint時に参照するconfigファイルを指定
  },
  root: true, // 上位ディレクトリにある他のeslintrcを参照しないようにする
  rules: {}
}
```

- package.jsonに`"Lint-fix": "eslint --fix './src/**/*.{js.ts}' "`を追加する
```js
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "Lint-fix": "eslint --fix './src/**/*.{js.ts}' " //⇦追加する
  },
```