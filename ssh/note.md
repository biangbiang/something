ssh学习笔记
=========

### 登录

    ssh {username}@{host}
    ssh -l {username} {host}

---

### 上传文件

    scp {filename} {username}@{host}:{filepath}

---

### 下载文件

    scp {username}@{host}:{remotefile} {localfilepath}
    scp -r {username}@{host}:{remotefile} {localfilepath}

### ssh-keygen

ssh-keygen -t rsa #使用rsa加密

加密方式选 rsa|dsa均可以，默认dsa
