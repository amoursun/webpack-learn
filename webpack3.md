#webpack
##demo2  Output Management 产出管理
    
###设置HtmlWebpackPlugin
        首先安装插件并调整webpack.config.js文件：
        npm install --save-dev html-webpack-plugin
        
        webpack.config.js:
          const path = require('path');
          const HtmlWebpackPlugin = require('html-webpack-plugin');
        
          module.exports = {
            entry: {
              app: './src/index.js',
              print: './src/print.js'
            },
           plugins: [
             new HtmlWebpackPlugin({
               title: 'Output Management'
             })
           ],
            output: {
              filename: '[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
            }
          };
          
          HtmlWebpackPlugin默认情况下会生成自己的index.html文件，
          即使我们已经在dist/文件夹中有一个。这意味着它将index.html
          用新生成的文件替换我们的文件
          这将生成一个dist/index.html包含以下内容的文件：
          <！DOCTYPE html>
          < html >
            < head >
              < meta  charset = “ UTF-8 ” >
              < title > Webpack App </ title >
            </ head >
            < body >
              < script  src = “ index_bundle.js ” > < / script >
            </ body >
          </ html >
###清理/dist文件夹
            Webpack将生成文件并将它们放在/dist您的文件夹中，但它不会跟踪项目实际使用的文件
            一个流行的插件来管理这个是clean-webpack-plugin所以让我们来安装和配置它。
            npm install clean-webpack-plugin --save-dev
            
            webpack.config.js:
              const path = require('path');
              const HtmlWebpackPlugin = require('html-webpack-plugin');
              const CleanWebpackPlugin = require('clean-webpack-plugin');
            
              module.exports = {
                entry: {
                  app: './src/index.js',
                  print: './src/print.js'
                },
                plugins: [
                 new CleanWebpackPlugin(['dist']),
                  new HtmlWebpackPlugin({
                    title: 'Output Management'
                  })
                ],
                output: {
                  filename: '[name].bundle.js',
                  path: path.resolve(__dirname, 'dist')
                }
              };
              
              运行npm run build并检查/dist文件夹。如果一切顺利，
              您现在只能看到从构建生成的文件，不再有旧的文件
              
              
    为了更容易地跟踪错误和警告，JavaScript提供了源代码映射，将您的编译代码映射到原始的源代码。
    如果一个错误源自于b.js，源地图将会明确地告诉你
            webpack.config.js    配置 devtool: 'inline-source-map',
            
            
### npm run build每次要编译代码时，手动运行都很麻烦 
                Webpack中有几个不同的选项可以帮助您在更改代码时自动编译代码：
                webpack的Watch模式
                的WebPack-DEV-服务器
                的WebPack-DEV-中间件
                在大多数情况下，您可能想要使用webpack-dev-server，但是让我们探索上述所有选项

####webpack的Watch模式
        package.json
        
          {
            "name": "development",
            "version": "1.0.0",
            "description": "",
            "main": "webpack.config.js",
            "scripts": {
              "test": "echo \"Error: no test specified\" && exit 1",
        +     "watch": "webpack --watch",
              "build": "webpack"
            },
            "keywords": [],
            "author": "",
            "license": "ISC",
            "devDependencies": {
              "css-loader": "^0.28.4",
              "csv-loader": "^2.1.1",
              "file-loader": "^0.11.2",
              "html-webpack-plugin": "^2.29.0",
              "style-loader": "^0.18.2",
              "webpack": "^3.0.0",
              "xml-loader": "^1.2.1"
            }
          }
        命令行运行npm run watch : 缺点是您必须刷新浏览器才能看到更改
####webpack-dev-server 
        npm install --save-dev webpack-dev-server
        
        webpack.config.js
          const path = require('path');
          const HtmlWebpackPlugin = require('html-webpack-plugin');
        
          module.exports = {
            entry: {
              app: './src/index.js',
              print: './src/print.js'
            },
            devtool: 'inline-source-map',
        +   devServer: {
        +     contentBase: './dist'
        +   },
            plugins: [
              new CleanWebpackPlugin(['dist']),
              new HtmlWebpackPlugin({
                title: 'Development'
              })
            ],
            output: {
              filename: '[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
            }
          };
          webpack-dev-server您从dist目录中的文件提供服务localhost:8080
          
          package.json
            {
              "name": "development",
              "version": "1.0.0",
              "description": "",
              "main": "webpack.config.js",
              "scripts": {
                "test": "echo \"Error: no test specified\" && exit 1",
                "watch": "webpack --progress --watch",
          +     "start": "webpack-dev-server --open",
                "build": "webpack"
              },
              "keywords": [],
              "author": "",
              "license": "ISC",
              "devDependencies": {
                "css-loader": "^0.28.4",
                "csv-loader": "^2.1.1",
                "file-loader": "^0.11.2",
                "html-webpack-plugin": "^2.29.0",
                "style-loader": "^0.18.2",
                "webpack": "^3.0.0",
                "xml-loader": "^1.2.1"
              }
            }
            命令行运行npm start，我们将看到我们的浏览器自动加载我们的页面
            
####使用webpack-dev-middleware
webpack-dev-middleware是将webpack处理的文件发送到服务器的包装器。
这在webpack-dev-server内部使用，但是它可以作为单独的包使用，以允许更多的自定义设置

        安装express，webpack-dev-middleware
        npm install --save-dev express webpack-dev-middleware
        
        webpack.config.js中增加
        output: {
              filename: '[name].bundle.js',
              path: path.resolve(__dirname, 'dist'),
        +     publicPath: '/'
            }
        publicPath将在我们的服务器脚本中使用，以确保文件的正常运行http://localhost:3000，
        我们稍后将指定的端口号。下一步是设置我们的自定义express服务器
        server.js
        














