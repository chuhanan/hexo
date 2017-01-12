---
title: Electron环境配置 
date: 2017年1月11日
categories: 
     - JS   
toc: true
---
## 简单的Electron环境配置
<!-- more -->
1. 新建目录,打开命令行执行 `npm init -y`
2. 复制如下到 `package.json` 中,注意字段覆盖
```
	  "scripts": {
	    "test": "echo \"Error: no test specified\" && exit 1",
	    "start": "electron .",
	    "watch": "watchify app/appEntry.js -t babelify -o public/js/bundle.js --debug --verbose",
	    "package": "electron-packager ./ DemoApps --overwrite --app-version=1.0.0 --platform=win32 --arch=all --out=../DemoApps --version=1.2.1 --icon=./public/img/app-icon.icns"
	  },
	  "devDependencies": {
	    "babel": "^6.5.2",
	    "babel-plugin-transform-es2015-spread": "^6.8.0",
	    "babel-plugin-transform-object-rest-spread": "^6.8.0",
	    "babel-preset-es2015": "^6.9.0",
	    "babel-preset-react": "^6.5.0",
	    "babelify": "^7.3.0",
	    "browserify": "^13.0.1",
	    "electron-packager": "^7.0.3",
	    "electron-reload": "^1.0.0",
	    "watchify": "^3.7.0"
	  },
	  "dependencies": {
	    "electron-prebuilt": "^1.4.13",
	    "react": "^15.1.0",
	    "react-color": "2.1.0",
	    "react-dom": "^15.1.0"
	  },
```
3. 执行 `npm i electron-prebuilt -g` 全局安装预编译依赖
如果下载很慢需要翻墙,或者去 `https://npm.taobao.org/mirrors/electron` 手动下载安装包

4. 执行 `npm i ` 安装依赖(时间稍长,设置 [淘宝源](https://npm.taobao.org/) 加快下载速度) 
5. 执行 `npm run watch` 
6. 上一步成功后,再开启一个 `cmd` 窗口执行 `npm run start`
7. 看到自动弹出桌面,搭建成功