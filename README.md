# webpack-demo

 ### 使用webpack
 1. 用babel-loader将ES6转译为ES5
 2. 用sass-loader将SCSS转译为CSS


 ### 引入webpack

 #####   创建目录、进入目录

 ` mkdir webpack-demo && cd webpack-demo `

 #####   创建package.json文件

 ` npm init -y `

 #####   安装webpack、webpack-cli

 ` npm install webpack webpack-cli --save-dev `

 #####  使用配置

 ` touch webpack.config.js `

 #####   编辑文件


```javascript
/* 将src/index.js文件拷贝到dist/main.js中 */
const path = require('path');

module.exports = {
  //入口
  entry: './src/index.js',
  //输出
  output: {
     filename: 'main.js',
     path: path.resolve(__dirname, 'dist')
  }
};
```

 #####   运行webpack.config.js文件，将entry文件拷贝到output中的文件

 ` npx webpack `

#####   创建新的文件

 `mkdir -p src/js`

 `cd src/js`

 `touch app.js module-1.js module-2.js`

 #####  编辑新创建的文件的内容

 module-1.js:
 ```javascript
 function fn(){
    alert('这是模块一')
}
/*当有人调用我时，我就把fn传给他*/
export default fn
 ```
 module-2.js
 ```javascript
 function fn(){
    alert('这是模块二')
}
export default fn
```
app.js:
```javascript
/*这里module1就代表了fn*/
import module1 from './module-1'
import module2 from './module-2'
alert('这是进口文件')
module1()
module2()
```

 ##### 修改webpack.config.js文件

```javascript
/* 将src/js/app.js文件拷贝到dist/js/bundle.js中 */
const path = require('path');

module.exports = {
  //入口
  entry: './src/js/app.js',
  //输出
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist/js')
  }
};
```
##### 重新运行一下webpack.config.js文件

这次运行入口文件是`./src/js/app`,所以它会将该文件的内容拷贝到`dist/js/bundle.js`文件中

` npx webpack`

##### 创建主页，并引入经过处理后的js文件
` touch index.html`

` vi index.html`

```html
<body>
    <script src="./dist/js/bundle.js"></script>
</body>
```

##### 运行webpack-demo项目
` http-server -c-1`

由于引入的文件中是app.js文件中拷贝过去的，所以运行的内容为app.js中的内容。这里会显示三个`alert box`,内容依次为这是入口文件、这是模块一、这是模块二

结果如下:

<img src="http://pg7gx692c.bkt.clouddn.com/Screenshot_1.png" alt="app" width="500" height="150">

<img src="http://pg7gx692c.bkt.clouddn.com/Screenshot_2.png" alt="app" width="500" height="150">

<img src="http://pg7gx692c.bkt.clouddn.com/Screenshot_3.png" alt="app" width="500" height="150">

### 引入babel-loader

##### 安装
> webpack 4.x | babel-loader 7.x | babel 6.x

`npm install -D babel-loader@7 babel-core babel-preset-env webpack`

##### 使用loader
给webpack.config.js文件添加如下代码:

```javascript
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['env']
        }
      }
    }
  ]
}
```
##### 运行webpack.config.js文件
`npx webpack`

这时,src/js下的所有文件都被翻译到dist/js/bundle.js文件,使用babel-loader,可以将ES6的语法转换成ES5

### 引入sass-loader

##### 安装
`npm install sass-loader node-sass webpack --save-dev`

`npm install style-loader css-loader --save-dev`

##### 编辑webpack.config.js文件

```javascript
// webpack.config.js
module.exports = {
	...
    module: {
        rules: [{
            test: /\.scss$/,
            use: [
                "style-loader", // creates style nodes from JS strings
                "css-loader", // translates CSS into CommonJS
                "sass-loader" // compiles Sass to CSS, using Node Sass by default
            ]
        }]
    }
};
```

##### 添加一个scss文件
`mkdir -p src/css`

`cd src/css`

`touch main.scss`

`vi main.scss`

```css
body{
    p{
        color:red;
    }
}
```
##### 在入口文件app.js中添加新模块
```javascript
import '../css/main.scss'
```

##### 运行webpack.config.js文件
`npx webpack`

这时`sass-loader`、`css-loader`、`style-loader`协作运行就会将`main.scss`翻译进`dist/js/bundle.js`中,其中有一系列处理又会使主页的`body`下的`p`元素的字体颜色设置为红色


