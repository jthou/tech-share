## 基本操作

### 01. 如何将GitHub仓库的代码推送到Gitee 

- 从github下载仓库

  ```bash
  git clone https://github.com/yourid/your_repo
  ```

- 在gitee上新建仓库

  ![image-20210907104321224](img/03-Git%E7%AE%80%E6%98%93%E6%95%99%E7%A8%8B/image-20210907104321224.png)

- 为代码添加远程仓库

  ```bash
  git remote add gitee https://gitee.com/yourid/your_repo
  ```

- 将代码推送到gitee

  ```bash
  git push gitee master
  ```

- 将代码推送到github

  ```bash
  git push origin master
  ```

> 同理，可以添加多个远程仓库。
