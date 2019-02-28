---
title: Webpack搭建es6和less开发环境
date: 2018-04-13 17:23:06
tags: webpack es6
---
# es6和less开发环境
由于主流浏览器还不支持es6语法,我们写样式也越来越习惯用less/scss等预处理形式，这里搭建一个简单的开发环境，供快速开始新小项目的开发

### 目录结构
- es6 *<span style='color:#999'>//es6语法的js源文件</span>*
- css *<span style='color:#999'>//less源文件</span>*
- src/index.html *<span style='color:#999'>//首页模版</span>*
- dist *<span style='color:#999'>//编译压缩后的文件</span>*
- webpack.config.js *<span style='color:#999'>//webpack配置文件</span>*

### 开始
`npm install`

`npm run webpack`

浏览器打开dist/index.html

### github
[https://github.com/GoldTao/webpack-es6-less](https://github.com/GoldTao/webpack-es6-less)

### 主要代码
- webpack.config.js

        const webpack = require('webpack');
        const HtmlWebpackPlugin = require('html-webpack-plugin');
        const ExtractTextPlugin = require('extract-text-webpack-plugin');
        const path = require('path');
        module.exports = {
            entry: './es6/main.js',
            output: {
                path: path.resolve(__dirname, 'dist'),
                filename: 'bundle.js'
            },
            devtool: "source-map",
            module: {
                rules: [
                    {
                        test: /\.(css|less)$/,
                        use: ExtractTextPlugin.extract({
                          fallback: "style-loader",
                          use: [
                              {
                                  loader: 'css-loader',
                                  options: {
                                      minimize: true
                                  }
                              },
                              {
                                  loader: 'less-loader'
                              }
                          ]
                        })
                    },
                    {
                        test: path.join(__dirname, 'es6'),
                        loader: 'babel-loader',
                        query: {
                            presets: ['es2015']
                        }
                    }
                ]
            },
            plugins: [
                new webpack.BannerPlugin('版权所有，翻版必究'),
                new HtmlWebpackPlugin(
                    {template: './src/index.html'} //自动生成html文件
                ),
                new webpack.optimize.UglifyJsPlugin({
                    compress: {
                        warnings: false
                    },
                    mangle: true, // 混淆
                    sourceMap: true, // 没有的话，sourceMap无效
                }),
                new ExtractTextPlugin("style.css") // 样式文件独立出来
            ]
        }

- package.json
        
        {
          "name": "webpack-es6-less",
          "version": "1.0.0",
          "description": "",
          "main": "index.js",
          "scripts": {
            "babel": "babel es6 --out-file js --presets es2015",
            "webpack": "webpack --watch"
          },
          "keywords": [],
          "author": "",
          "license": "ISC",
          "devDependencies": {
            "babel-cli": "^6.26.0",
            "babel-loader": "^7.1.4",
            "babel-preset-es2015": "^6.24.1",
            "css-loader": "^0.28.11",
            "extract-text-webpack-plugin": "^2.1.2",
            "html-webpack-plugin": "^3.2.0",
            "less-loader": "^4.1.0",
            "style-loader": "^0.20.3",
            "webpack": "^2.6.1"
          }
        }
