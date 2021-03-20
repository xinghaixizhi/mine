### 查看全局 npm 包

浅层查看（自己安装的），否则太多？

```
npm ls -g --depth=0
```



### 清除缓存

```
npm cache clean -f
```



### 切换 npm 源

使用国内的淘宝源

```
npm config set registry https://registry.npm.taobao.org
```



### nrm

node register manager，切换 npm 源

1. 安装

   ```
   npm i nrm -g
   ```

2. 查看源列表

   ```
   nrm ls
   ```

3. 添加源

	```
	nrm add taobao https://registry.npm.taobao.org/
	```

1. 删除源

   ```
   nrm del taobao
   ```

6. 切换源，如：使用 taobao 源

   ```
   nrm use taobao
   ```

tips:

1. nrm 报错，`code: 'ERR_INVALID_ARG_TYPE`，找到 Node 全局 `node_modules` 下的 `/nrm/cli.js` 文件，将第 17 行注释修改为下边内容，

   ```
   // const NRMRC = path.join(process.env.HOME, '.nrmrc');
   const NRMRC = path.join(process.env[(process.platform == 'win32') ? 'USERPROFILE' : 'HOME'], '.nrmrc')
   ```


### nvm

node version manager，切换 Node 版本，

1. 下载安装

   卸载已安装的 Node，否则会发生冲突，然后下载 nvm-windows 最新版本

   https://github.com/coreybutler/nvm-windows/releases

2. 配置安装源

   由于官方的太慢，这里配置淘宝源，打开安装目录下 `settings.txt` 文件

   ```
root: E:\programs\nvm
   arch: 64
   proxy: none
   originalpath: .
   originalversion: 
   node_mirror: https://npm.taobao.org/mirrors/node/
   npm_mirror: https://npm.taobao.org/mirrors/npm/
   ```
   
3. 安装指定版本的 Node，如：`14.16.0`

   ```
   nvm install 14.16.0
   ```

4. 切换 Node 版本

   ```
   nvm use 14.16.0
   ```

5. 删除指定版本

   ```
   nvm uninstall 14.16.0
   ```

   

