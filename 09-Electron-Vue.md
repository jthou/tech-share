## 开发环境配置

### NodeJS 版本管理

#### Windows

1. 安装nvm

   > 下载地址：https://wordpress.com/support/markdown-quick-reference/

2. 安装nodejs:

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
   
3. 查看

   ```
   > nvm list
   ```

   ```
       14.18.0
     * 10.16.0 (Currently using 64-bit executable)
   ```

4. 选择

   ```
   nvm use 14.18.0
   ```

   

#### Linux

1. 安装npm

   ```bash
   sudo apt install npm
   ```

2. 安装 n

   ```bash
   sudo npm install -g n
   ```

3. 安装nodejs

   ```bash
   sudo n install 14.18.0
   sudo n install latest
   ```

4. 查看

   ```bash
   n ls
   ```

   ```
   node/14.18.0
   node/16.11.0
   ```

5. 选择

   ```
   sudo n
   ```

   

## 参考文献

[1]. [yarn cli](https://classic.yarnpkg.com/en/docs/cli/)

