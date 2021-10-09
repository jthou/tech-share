## 开发环境配置

### NodeJS 版本管理

开发环境往往需要多种node版本切换，这时候需要用到node的版本管理工具，在Windows下用`nvm`的Windows版本，在Linux下有更多的选择，这里我们选择一个叫做`n`的工具。

#### Windows

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
   >
   >   

3. 查看已经安装的版本

   ```
   > nvm list
   ```

   ```
       14.18.0
     * 10.16.0 (Currently using 64-bit executable)
   ```

4. 选择node版本

   ```
   nvm use 14.18.0
   ```

   

#### Linux

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

   ```bash
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
   
   

## 参考文献

[1]. [yarn cli](https://classic.yarnpkg.com/en/docs/cli/)

