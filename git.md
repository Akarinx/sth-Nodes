# 更新代码

```
cd Code
cd pc-client
git pull origin dev --rebase //拉取最新代码
light setup 
```

# 调试项目

```
cd app
yarn watch
yarn electron
yarn start // pc-client/larklets/byted-larklet-maill 目录下启动

----staging环境测试
yarn staging-watch
yarn staging-electron

----prerelease环境测试
yarn prerelease-watch
yarn prerelease-electron
```

使用cmd+r刷新

# 提交修改

```
git commit -m "" // 按规范提交
git commit --amend // 对之前提交内容进行修改，也可以修改提交说明
git reset --hard id //回滚版本
git checkout -b [feature-项目名]新开分支并进入新分支
git rebase dev // 将dev分支变基到新分支（先切dev分之pull新代码）
-----rust 
拉取最新pb
git fetch //拉取最新rusk sdk代码 --master分支
切分支
make mail_pb //合并最新pb
```

# 缓存区

```
git stash  //存入缓存区
git stash pop //拉回现分支
```
