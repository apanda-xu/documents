# 1 初级（ubuntu20.04）
## 1.1 安装git
### 使用APT安装
    sudo apt update         // 更新apt
    sudo apt install git    // 安装git
    git --version           // 查看git版本，当前是 git version 2.25.1
### 配置姓名和邮件
    git config --global user.name  "Your Name"                  
    git config --global user.email "youremail@yourdomain.com"
                                

### 配置文件被存储在~/.gitconfig文件
    [user]
        name = Your Name
        email = youremail@yourdomain.com
### 参考
#### [如何在 Ubuntu 20.04 上安装 Git](https://zhuanlan.zhihu.com/p/137578868)
<br>

## 1.2 git设置HTTP(S)代理，如果能正常使用可以不管
### 设置代理
    git config --global http.proxy 'socks5://127.0.0.1:1080' 
    git config --global https.proxy 'socks5://127.0.0.1:1080'
### 查看代理
    git config --global --get http.proxy
    git config --global --get https.proxy
### 取消代理
    git config --global --unset http.proxy
    git config --global --unset https.proxy
### 参考
#### [git设置、查看、取消代理](https://www.cnblogs.com/yongy1030/p/11699086.html)
<br>


## 1.3 生成SSH密钥（一对公钥和私钥），并将公钥上传到github服务器
### 本地生成SSH密钥
    先检查~/.ssh目录下有无 id_rsa（私钥）和id_rsa.pub（公钥）
    若已存在，则不需要生成，防止覆盖；若无，则使用下面的命令生成：
    ssh-keygen -t rsa       // 在~/.ssh目录下生成密钥
### 将公钥id_rsa.pub内容拷贝到github服务器
![ssh](./ssh.png)
<br>

## 1.4 将本地项目上传到github
### 首次上传
    cd {workspace}                  // 进入项目所在的文件夹
    git init                        // 初始化项目文件夹作为本地仓库
    git checkout -b main            // 新建main分支，并切换到main分支
    git remote add origin xxx       // 连接远程仓库。可以使用SSH协议（推荐）：git@github.com:apanda-xu/documents.git,也可以使用HTTPS协议（有墙，容易出现连接不上的情况）：https://github.com/apanda-xu/documents.git
    git pull origin main            // push之前先从远程仓库拉取
    git add .                       // 将变化的文件（修改、新建、删除）加入暂存区
    git commit -m "commit info"     // 提交变更
    git push -u origin main         // push到远端仓库
### 后续上传 （由于第一次push使用了-u参数，所以后续上传可以简单push）
    git pull                     
    git push
<br>

## 1.5 [解决Github连接慢或者无法连接(通过修改hosts文件解决)](https://cloud.tencent.com/developer/article/2023920)
## 1.6 常用git命令




<br>
<br>

# 2 进阶