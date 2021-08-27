## Apache配置

### 1.  目录访问控制

为主目录或虚拟目录设置权限，这些语句仅对被设置目录及其子目录起作用，比如：

![image-20210827102631004](img/05-Apache%E7%AE%80%E6%98%8E%E6%95%99%E7%A8%8B/image-20210827102631004.png)

#### Options

Options选项用于定义目录使用哪些特性，包括Indexes、MultiViews和ExecCGI等，如下。

| 命令           | 说明                                                         |
| :------------- | ------------------------------------------------------------ |
| indexes        | 允许目录浏览                                                 |
| MultiViews     | 允许内容协商的多重视图；举例，如访问目录下并不存在的/test/a文件，但有/test/a.gif文件，apache将将其返回。 |
| All            | 除MultiViews之外的所有特性，如果没有Options语句，默认为All   |
| ExecCGI        | 允许在该目录下执行CGI脚本                                    |
| FollowSymLinks | 可以在该目录中使用符号连接                                   |
| Includes       | 允许服务器端包含功能                                         |
| IncludesNoExec | 允许服务器端包含功能，但禁用执行CGI脚本                      |

> 一旦定义允许目录浏览，就会将Web站点的文件夹和文件名结构暴露给黑客。目录浏览还会允许黑客浏览文件并掌握服务器配置信息，所以指定该权限往往带来安全性上的隐患。除非有充足的理由要使用目录浏览，否则应该禁用它。

#### AllowOverride
```xml
AllowOverride None
```
`AllowOverride`选项用于定义位于每个目录下`.htaccess`（访问控制）文件中的指令类型。

基于安全和效率的原因，虽然可以通 过`.htaccess`来设置目录的访问权限，但应尽可能地避免使用`.htaccess`文件，所以一般将`AllowOverride`设置为”None”，即 禁止使用`.htaccess`文件，而将目录权限的设置放在主配置文件`httpd.conf`的`<Directory>` 和`</Directory>`语句之间。

#### Order / Require

在Apache2.2版本中，访问控制是基于客户端的主机名、IP地址以及客户端请求中的其他特征，使用Order, Allow Deny,Satisfy(满足)指令来实现。

在Apache2.4版本中，使用mod_authz_host这个新的模块，来实现访问控制，其他授权检查也以同样的方式来完成。旧的访问控制语句应当被新的授权认证机制所取代，即便Apache已经提供了mod_access_compat这一新模块来兼容旧语句。

在Apache2.2中，设置缺省的访问权限与Allow和Deny语句的处理顺序。Allow和Deny语句可以针对客户机的域名或IP地址进行设置，以决定哪些客户机能够访问服务器。通常设置为以下两种值之一。

- `allow, deny`：缺省禁止所有客户机的访问。如果某条件既匹配Deny语句又匹配Allow语句，则Deny语句会起作用（因为Deny语句覆盖了Allow语句）。

- `deny, allow`：缺省允许所有客户机的访问。如果某条件既匹配Deny语句又匹配Allow语句，则Allow语句会起作用（因为Allow语句覆盖了Deny语句）。

举例：

**允许所有客户机的访问**

```
# apache2.2
Order allow deny
Allow from all
```
```
# apache2.4
Require all granted 
```

**除了来自hacker.com域和IP地址为192.168.16.111的客户机外，允许所有客户机的访问。**

```
# apache2.2
Order deny, allow
Deny from hacker.com
Deny from 192.168.16.111
```
```
# apache2.4
Require not hacker.com
Require not 192.168.16.111
```
**仅允许来自网络192.168.16.0/24客户机的访问。**

```
# apache2.2
Order allow, deny
Allow from 192.168.16.0/24 # 除192.168.16.0/24一概禁止
```
```
# apache2.4
Require all denied
Require 192.168.16.0/24
```



### 2. 认证访问

有的时候，只允许授权用户访问特定的目录，比如文件下载等，这时候可以对特定的目录配置授权访问，步骤如下：

- 配置文件夹访问控制参数

```xml
<Directory "/share/soft/">
        Options Indexes FollowSymLinks
        AllowOverride None
        AuthUserFile /usr/share/apache.passwd
        AuthName "input your password"
        AuthType Basic
        Require valid-user
</Directory>
```

- 建立用户密码文件

```bash
# 第一次运行，创建文件
htpasswd  -c  /share/soft/apache.passwd  user1  
# 再运行，添加用户
htpasswd  -m  /share/soft/apache.passwd  user2
```



## 参考文献

[1]. [https://www.cnblogs.com/leezhxing/p/3298060.html](https://www.cnblogs.com/leezhxing/p/3298060.html)

