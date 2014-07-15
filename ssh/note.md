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
