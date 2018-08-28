#webpack
##demo1
##基本设置
     首先我们创建一个目录，初始化npm并在本地安装webpack：
         mkdir webpack-demo && cd webpack-demo
         npm init -y
         npm install --save-dev webpack
##创建捆绑包
        要捆绑lodash依赖关系index.js，我们需要在本地安装库
        npm install --save lodash
        然后在我们的脚本中导入...
        SRC / index.js
##使用配置 webpack.config.js
        const path = require('path');
        
        module.exports = {
          entry: './src/index.js',
          output: {
            filename: 'bundle.js',
            path: path.resolve(__dirname, 'dist')
          }
        };
        
        运行:
        ./node_modules/.bin/webpack --config webpack.config.js
###我们通过添加一个npm脚本来调整我们的package.json：
        {
          ...
          "scripts": {
            "build": "webpack"
          },
          ...
        }
        
        运行 npm run build
        scripts我们可以通过名称引用本地安装的npm软件包，而不是写出整个路径。这个约定是大多数基于npm的项目中的标准，并允许我们直接调用webpack，而不是./node_modules/.bin/webpack
        
##处理 图像/css/字体
### 载入css
        载入CSS
        为了import从JavaScript模块中获取CSS文件，
        您需要安装并将style-loader和css-loader添加到您的module配置中：
        npm install --save-dev style-loader css-loader
        
        webpack.config.js: 
          const path = require('path');
        
          module.exports = {
            entry: './src/index.js',
            output: {
              filename: 'bundle.js',
              path: path.resolve(__dirname, 'dist')
            },
            module: {
              rules: [
                {
                  test: /\.css$/,
                  use: [
                    'style-loader',
                    'css-loader'
                  ]
                }
              ]
            }
          };
### 载入图像
        npm install --save-dev file-loader
        
        webpack.config.js:
          const path = require('path');
        
          module.exports = {
            entry: './src/index.js',
            output: {
              filename: 'bundle.js',
              path: path.resolve(__dirname, 'dist')
            },
            module: {
              rules: [
                {
                  test: /\.css$/,
                  use: [
                    'style-loader',
                    'css-loader'
                  ]
                },
               {
                 test: /\.(png|svg|jpg|gif)$/,
                 use: [
                   'file-loader'
                 ]
               }
              ]
            }
          };
### 加载字体
        npm install --save-dev file-loader
        
        const path = require('path');
        module.exports = {
            entry: './src/index.js',
            output: {
                filename: 'bundle.js',
                path: path.resolve(__dirname, 'dist')
            },
            module: {
              rules: [
                 {
                   test: /\.css$/,
                   use: [
                     'style-loader',
                     'css-loader'
                   ]
                },
                {
                  test: /\.(png|svg|jpg|gif)$/,
                  use: [
                     'file-loader'
                   ]
                },
                {
                  test: /\.(woff|woff2|eot|ttf|otf)$/,
                  use: [
                      'file-loader'
                    ]
                }
             ]
           }
        };
### 加载数据中
   可以加载的另一个有用的资产是数据，如JSON文件，CSV，TSV和XML。
   支持JSON实际上是内置的，类似于NodeJS，意思import Data from './data.json'
   是默认情况下工作。要导入CSV，TSV和XML，您可以使用csv-loader和xml-loader。
   我们来处理所有三个文件：
   
   npm install --save-dev csv-loader xml-loader
   webpack.config.js
   
     const path = require('path');
   
     module.exports = {
       entry: './src/index.js',
       output: {
         filename: 'bundle.js',
         path: path.resolve(__dirname, 'dist')
       },
       module: {
         rules: [
           {
             test: /\.css$/,
             use: [
               'style-loader',
               'css-loader'
             ]
           },
           {
             test: /\.(png|svg|jpg|gif)$/,
             use: [
               'file-loader'
             ]
           },
           {
             test: /\.(woff|woff2|eot|ttf|otf)$/,
             use: [
               'file-loader'
             ]
           },
          {
            test: /\.(csv|tsv)$/,
            use: [
              'csv-loader'
            ]
          },
          {
            test: /\.xml$/,
            use: [
              'xml-loader'
            ]
          }
         ]
       }
     };

 
##demo2

