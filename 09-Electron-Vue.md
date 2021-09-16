## 前言

### Nodejs

Node.js 是一个开源与跨平台的 JavaScript 运行时环境。 它是一个可用于几乎任何项目的流行工具！

在浏览器外运行 V8 JavaScript 引擎（Google Chrome 的内核）。

Node.js 应用程序运行于单个进程中，无需为每个请求创建新的线程。同时Node.js 在其标准库中提供了一组异步的 I/O 原生功能，用以防止 JavaScript 代码被阻塞。当 Node.js 执行 I/O 操作时（例如从网络读取、访问数据库或文件系统），Node.js 会在响应返回时恢复操作，而不是阻塞线程并浪费 CPU 循环等待。

这使 Node.js 可以在一台服务器上处理数千个并发连接，而无需引入管理线程并发的负担（这可能是重大 bug 的来源）。



## yarn

yarn是一个代码包管理器，用它可以使用其他开发者分享的代码，也可以讲代码分享给他人。代码包的描述包含在`package.json`文件中。

### 安装

推荐通过npm安装。

```bash
npm install --global yarn
yarn --version
```



### 配置

#### 设置 key value

语法：

```bash
yarn config set <key> <value> [-g|--global]
```

举例：

```bash
$ yarn config set init-license BSD-2-Clause
yarn config vx.x.x
success Set "init-license" to "BSD-2-Clause".
✨  Done in 0.05s.
```

#### 查看 key

语法：

```bash
yarn config get <key>
```

举例：

```bash
$ yarn config get init-license
BSD-2-Clause
```

#### 删除 key

语法：

```bash
yarn config delete <key>
```

举例：

```bash
$ yarn config delete test-key
yarn config vx.x.x
success Deleted "test-key".
✨  Done in 0.06s.
```

#### 显示当前配置

语法：

```bash
yarn config list
```

举例：

```bash
$ yarn config list
yarn config vx.x.x
info yarn config
{ 'version-tag-prefix': 'v',
  'version-git-tag': true,
  'version-git-sign': false,
  'version-git-message': 'v%s',
  'init-version': '1.0.0',
  'init-license': 'MIT',
  'save-prefix': '^',
  'ignore-scripts': false,
  'ignore-optional': true,
  registry: 'https://registry.yarnpkg.com',
  'user-agent': 'yarn/0.15.0 npm/? node/v6.2.1 darwin x64' }
info npm config
{ registry: 'https://registry.npmjs.org/',
  '//localhost:4873/:_authToken': 'some-auth-token' }
✨  Done in 0.05s.
```





## 参考文献

[1]. [yarn cli](https://classic.yarnpkg.com/en/docs/cli/)

