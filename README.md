# WebpackでVue.js(TypeScript, Pug, Stylus対応)を構築

## 気をつける点

1. `tsconfig.json`を追加

```json
{
  "compilerOptions": {
    "sourceMap": true,
    // TSはECMAScript 5に変換
    "target": "es5",
    // TSのモジュールはES Modulesとして出力
    "module": "es2015",
    // import Vue from 'vue' の書き方を許容する
    "allowSyntheticDefaultImports": true,
    "lib": [
      "dom",
      "es2019"
    ],
    "moduleResolution": "node",
    // デコレーターを有効に設定
    "experimentalDecorators": true
  }
}
```

2. Vue.jsを読み出すディレクトリに`shims-vue.d.ts`拡張子*.vue用に型定義ファイルを置く

```ts
declare module '*.vue' {
  import Vue from 'vue'
  export default Vue
}
```

3. Webpackの書き方になれる

```js
const VueLoaderPlugin = require('vue-loader/lib/plugin')

module.exports = {
  mode: 'development',
  entry: "./src/main.ts",
  // ファイルの出力設定
  output: {
    //  出力ファイルのディレクトリ名
    path: `${__dirname}/dist`,
    // 出力ファイル名
    filename: "[name].js"
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader'
      },
      // this will apply to both plain `.js` files
      // AND `<script>` blocks in `.vue` files
      {
        test: /\.js$/,
        loader: 'babel-loader'
      },
      // this will apply to both plain `.css` files
      // AND `<style>` blocks in `.vue` files
      {
        test: /\.styl(us)?$/,
        use: [
          'vue-style-loader',
          'css-loader',
          'stylus-loader'
        ]
      },
      {
        test: /\.ts$/,
        loader: 'ts-loader',
        options: { appendTsSuffixTo: [/\.vue$/] }
      },
      {
        test: /\.pug$/,
        loader: 'pug-plain-loader'
      }
    ]
  },
  plugins: [
    // make sure to include the plugin for the magic
    new VueLoaderPlugin()
  ]
}
```