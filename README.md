# 创建github repository并上传代码

1.生成ssh

![Screenshot from 2024-04-28 01-13-14](https://github.com/countsp/byd_autoware/assets/102967883/d74c8752-5f11-49fe-9db5-a198f2ef89ca)

2.打开生成的 /home/chopin/.ssh/id_rsa.pub 公钥,并复制

进入 https://github.com/settings/keys ，粘贴公钥

添加成功后就会显示出来，配置好之后，Github 远程仓库就可以和你的电脑通过 ssh 进行连接。

![Screenshot from 2024-04-28 01-19-41](https://github.com/countsp/byd_autoware/assets/102967883/3e8f61fc-7dd6-4d44-8567-53b2e4f5432b)

3. 在github中新建repository,复制ssh

![Screenshot from 2024-04-28 01-20-58](https://github.com/countsp/byd_autoware/assets/102967883/b6bd1767-b522-4c90-a5e2-f368c8c7a228)


4. 进入本地，新建 workspace

```
git init

git add -A

git status

git commit -m "adding a file"

git remote add origin git@github.com:countsp/byd_autoware.git

git remote set-url origin git@github.com:countsp/byd_autoware.git

git pull --rebase origin master

git push origin master

```

git init - 初始化一个新的Git仓库。这将在当前目录下创建一个.git文件夹，里面包含了所有的Git配置文件。

git add -A - 将当前目录下的所有更改（包括新文件、修改过的文件、删除的文件）添加到暂存区。这是准备将这些更改提交到仓库的一个步骤。

git status - 显示当前仓库的状态，包括哪些文件被修改但未提交，哪些文件已经暂存等待下次提交等。

git commit -m "adding a file" - 将暂存区中的更改提交到本地仓库。这里的"adding a file"是提交信息，用来描述本次提交的内容。

git remote add origin git@github.com:countsp/byd_autoware.git - 添加一个名为origin的远程仓库，并设置其URL为git@github.com:countsp/byd_autoware.git。这样你就可以将本地的更改推送到这个GitHub仓库。

git remote set-url origin git@github.com:countsp/byd_autoware.git - 修改名为origin的远程仓库的URL。这条命令通常用于远程仓库的URL更改后，更新本地仓库的远程配置。

git pull --rebase origin master - 从名为origin的远程仓库的master分支拉取最新的更改，并通过rebase的方式整合到本地分支。这有助于保持提交历史的线性。可以省略

git push origin master - 将本地的master分支的更改推送到远程仓库origin的master分支。

