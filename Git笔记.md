# Git 学习笔记

history 查看历史命令

## Git配置

```bash
# 查看系统有哪些配置
git config --system --list

# 查看本地有哪些配置
git config --global --list

# 设置配置信息
git config --global user.name 用户名
git config --global user.email 邮箱
```

三个工作区域：

- 工作目录
- 暂存区
- 资源库

![](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=1310625249,210024168&fm=26&gp=0.jpg)

![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1597647754836&di=cb14dffdb0d86222839423ddbd74689b&imgtype=0&src=http%3A%2F%2Fstatic.zybuluo.com%2Fcoder-pig%2Fget64rj0pj9n68icaogq9mzn%2F75062926.png)

![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1597647785945&di=0b0550d7a9e1a473859355653da50bcb&imgtype=0&src=http%3A%2F%2Faliyunzixunbucket.oss-cn-beijing.aliyuncs.com%2Fjpg%2F05f9c2f5db24d87c0e0c53a87e654e97.jpg%3Fx-oss-process%3Dimage%2Fresize%2Cp_100%2Fauto-orient%2C1%2Fquality%2Cq_90%2Fformat%2Cjpg%2Fwatermark%2Cimage_eXVuY2VzaGk%3D%2Ct_100)

## 文件四种状态

- 未跟踪
- 暂存状态
- 未修改
- 修改

## 命令

```bash
# 分支
git branch

# 查看远程分支
git branch -r

# 新建分支
git branch 分支名

# 新建并切换分支 
git checkout -b 分支名

# 删除分支
git branch -d 分支名

# 删除远程分支
git push origin --delete 分支名
git branch -dr 远程分支

# 合并分支
git merge 分支
```



