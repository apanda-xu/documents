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


## 1.3 将本地项目上传到github




<br>

# 2 进阶