## 开发环境配置

### NodeJS 版本管理

开发环境往往需要多种node版本切换，这时候需要用到node的版本管理工具，在Windows下用`nvm`的Windows版本，在Linux下有更多的选择，这里我们选择一个叫做`n`的工具。

#### nvm (Windows)

1. 安装nvm

   > 下载地址：https://wordpress.com/support/markdown-quick-reference/

2. 安装`nodejs`:

   ```cmd
   nvm install 14.18.0
   nvm install last
   nvm install 10.16.
   ```

   > - 设置淘宝镜像：
   >
   >   ```
   >   nvm node_mirror https://npm.taobao.org/mirrors/node/
   >   nvm npm_mirror https://npm.taobao.org/mirrors/npm/ 
   >   ```
   >
   > - 如果在内网，还可能需要设置proxy，nvm 依赖于curl下载，可设置proxy到`.curlrc`文件中
   >
   >   ```
   >   > cat .curlrc
   >   proxy = http://name:pass@host:port
   >   ```

3. 查看已经安装的版本

   ```
   > nvm list
   ```
       14.18.0
     * 10.16.0 (Currently using 64-bit executable)
   ```

4. 选择node版本

   ```
   nvm use 14.18.0
   ```
> **已知问题**
> 
> 在普通用户模式下，用nvm use切换可能会失败，报错如下:
> 
> ```
> exit status 1: �ܾ����ʡ�
> ```
> 
> 用管理员身份运行cmd后可以成功切换，之后再用普通用户身份运行node（如果继续用管理员身份执行，可能会导致以后某些文件同样需要管理员权限才能删除或者覆盖）。


#### n (Linux )

1. 安装`npm`
   ```bash
   sudo apt install npm
   ```

2. 安装 n

   ```bash
   sudo npm install -g n
   ```


3. 安装`nodejs`

   ```bash
   sudo n install 14.18.0
   sudo n install latest
   ```

4. 查看已经安装的版本

   ```
   n ls
   ```
   ```
   node/14.18.0
   node/16.11.0
   
   ```
5. 选择node版本

   ```
   sudo n
   ```
   ```
       node/14.18.0
     ο node/16.11.0
   
   Use up/down arrow keys to select a version, return key to install, d to delete, q to quit
   
   ```


#### `npx`

`npm` 从5.2版开始，增加了 `npx` 命令，用于调用项目内部安装的模块。如果没有安装，就手动安装一下：

```bash
npm install -g npx
```

`npx` 的主要使用场景：
- 调用项目内部安装的模块
  调用项目内部安装的模块
  npx 运行的时候，会到node_modules/.bin路径和环境变量$PATH里面，检查命令是否存在。
  
  比如，项目内部安装了测试工具 Mocha。
  ```bash
  npm install -D mocha
  ```
  
  一般来说，调用 Mocha ，只能在项目脚本和 package.json 的scripts字段里面， 如果想在命令行下调用，必须像下面这样。
  
  ```
  $ node-modules/.bin/mocha --version
  ```
  
  npx 就是想解决这个问题，让项目内部安装的模块用起来更方便，只要像下面这样调用就行了。

```bash
$ npx mocha --version
```

- 避免全局安装模块

  比如：
  
  ```bash
  $ npx create-react-app my-react-app
  ```
  
  上面代码运行时，npx 将create-react-app下载到一个临时目录，使用以后再删除。所以，以后再次执行上面的命令，会重新下载create-react-app。
  
  只要 `npx` 后面的模块无法在本地发现，就会下载同名模块。比如，本地没有安装http-server模块，下面的命令会自动下载该模块，在当前目录启动一个 Web 服务。
  
  ```bash
  $ npx http-server
  ```
  
  如果想让 `npx` 强制使用本地模块，不下载远程模块，可以使用--no-install参数。如果本地不存在该模块，就会报错。
  
  ```bash
  npx --no-install http-server
  ```
  反过来，如果忽略本地的同名模块，强制安装使用远程模块，可以使用--ignore-existing参数。
  
- 使用不同版本的 node

  利用 `npx` 可以下载模块这个特点，可以指定某个版本的 Node 运行脚本。
  
  ```bash
  $ npx node@0.12.8 -v
  ```
  ```
  v0.12.8
  ```
  
  上面命令会使用 0.12.8 版本的 Node 执行脚本。原理是从 `npm` 下载这个版本的 node，使用后再删掉。
  
  某些场景下，这个方法用来切换 Node 版本，要比 nvm 那样的版本管理器方便一些。[^1]

## 参考文献

[^1]: [阮一峰 npx 使用教程](https://www.ruanyifeng.com/blog/2019/02/npx.html)

