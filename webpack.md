# Webpack

相關的設定都從建立```webpack.config.js```開始

## webpack核心概念
### Entry & Output
webpack 4預設entry point為專案的```src/index```目錄下; output result則放在```dist/main.js```，可自行調整更動或引入多個entry point
```javascript
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle後的檔案名.js'
  }
//...
}
```
### Loaders
webpack只認得javascript檔案，如果需使用其他類型檔案需透過loader（如：html-loader, style-loader, css-loader, file-loader...等）。
有兩個屬性值，分別為test和use
* test：定義哪些檔案類型需要被轉換
* use：哪個loader需要被使用
```javascript
const path = require('path');

module.exports = {
  output: {
    filename: 'my-first-webpack.bundle.js'
  },
  module: { //所有loader都是放在module之下
    rules: [
      {
        test: /\.css$/,
        use: 'style-loader' //若有多個檔案可使用array[]
        // use: ['style-load', 'css-loader', 'postcss-loader']
      }
    ]
  }
};
```
※ [更多loader概念](https://webpack.js.org/concepts/loaders/)

### Plugins
Plugins可以執行更廣泛的任務，比如說：優化bundle後的檔案、資源管理或注入環境變量。
* 所有plugin都需要先require，再加到plugins的設定裡
```javascript
module.exports = {
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({template: './src/index.html'})
    //使用前要先建立instance
  ]
};
```
※ HtmlWebpackPlugin這個plugin會產生一個自動注入我們bundle後的js檔案的html檔
※ [更多plugin](https://webpack.js.org/plugins/)

### Mode
我們可以設定```development```, ```production``` or ```none```來優化對應的環境，default是```production```
```javascript
module.exports = {
  mode: 'production'
};
```