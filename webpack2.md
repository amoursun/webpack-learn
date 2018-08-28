#webpack
##demo3  Hot Module Replacement 热模块更换
###启用HMR
这个功能非常适合生产力。所有我们需要做的是更新我们的webpack-dev-server配置，并使用webpack的内置HMR插件。
我们还将删除模块print.js现在将使用的入口点index.js
        
        webpack.config.js
        
          const path = require('path');
          const HtmlWebpackPlugin = require('html-webpack-plugin');
        + const webpack = require('webpack');
        
          module.exports = {
            entry: {
        -      app: './src/index.js',
        -      print: './src/print.js'
        +      app: './src/index.js'
            },
            devtool: 'inline-source-map',
            devServer: {
              contentBase: './dist',
        +     hot: true
            },
            plugins: [
              new HtmlWebpackPlugin({
                title: 'Hot Module Replacement'
              }),
        +     new webpack.HotModuleReplacementPlugin()
            ],
            output: {
              filename: '[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
            }
          };
          
          还可以 修改webpack-dev-server配置：webpack-dev-server --hotOnly
          
          更新index.js文件，以便当print.js检测到内部的变化时，我们告诉webpack接受更新的模块
          
          index.js
            import _ from 'lodash';
            import printMe from './print.js';
          
            function component() {
              var element = document.createElement('div');
              var btn = document.createElement('button');
          
              element.innerHTML = _.join(['Hello', 'webpack'], ' ');
          
              btn.innerHTML = 'Click me and check the console!';
              btn.onclick = printMe;
          
              element.appendChild(btn);
          
              return element;
            }
          
            document.body.appendChild(component());
          +
          + if (module.hot) {
          +   module.hot.accept('./print.js', function() {
          +     console.log('Accepting the updated printMe module!');
          +     printMe();
          +   })
          + }
  陷阱:
       控制台正在打印旧printMe功能,按钮的onclick事件处理程序仍然绑定到原始printMe函数
       为了使这个工作与HMR，我们需要更新绑定到新的printMe功能使用module.hot.accept
                
                index.js
                
                  import _ from 'lodash';
                  import printMe from './print.js';
                
                  function component() {
                    var element = document.createElement('div');
                    var btn = document.createElement('button');
                
                    element.innerHTML = _.join(['Hello', 'webpack'], ' ');
                
                    btn.innerHTML = 'Click me and check the console!';
                    btn.onclick = printMe;  // onclick event is bind to the original printMe function
                
                    element.appendChild(btn);
                
                    return element;
                  }
                
                - document.body.appendChild(component());
                + let element = component(); // Store the element to re-render on print.js changes
                + document.body.appendChild(element);
                
                  if (module.hot) {
                    module.hot.accept('./print.js', function() {
                      console.log('Accepting the updated printMe module!');
                -     printMe();
                +     document.body.removeChild(element);
                +     element = component(); // Re-render the "component" to update the click handler
                +     document.body.appendChild(element);
                    })
                  }
                  
####HMR与样式表   npm install --save-dev style-loader css-loader
            webpack.config.js
            
              const path = require('path');
              const HtmlWebpackPlugin = require('html-webpack-plugin');
              const webpack = require('webpack');
            
              module.exports = {
                entry: {
                  app: './src/index.js'
                },
                devtool: 'inline-source-map',
                devServer: {
                  contentBase: './dist',
                  hot: true
                },
            +   module: {
            +     rules: [
            +       {
            +         test: /\.css$/,
            +         use: ['style-loader', 'css-loader']
            +       }
            +     ]
            +   },
                plugins: [
                  new HtmlWebpackPlugin({
                    title: 'Hot Module Replacement'
                  }),
                  new webpack.HotModuleReplacementPlugin()
                ],
                output: {
                  filename: '[name].bundle.js',
                  path: path.resolve(__dirname, 'dist')
                }
              };


    




















