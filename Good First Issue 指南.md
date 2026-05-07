## Good First Issue  

Good = 友好的、适合新手的  
First = 给**第一次**做开源贡献的人    

`good-first-issue` ： **适合新手第一次贡献的优质入门任务**。它是开源社区吸引新成员、降低入门门槛的重要机制，也是开启开源贡献之旅的最佳起点。    


> **声明**
>  - 这里首先演示 HTTPS 方式    
>  - master / main 请根据实际情况选择  
>  - 不同环境或系统的命令不一定相同，本文仅作为参考。   

## 基础准备  

### 环境配置  
```bash  
# 安装 git、vim  
sudo dnf install -y git vim git-lfs  

# 配置Git全局信息（与 AtomGit/GitHub 账号一致）  
git config --global user.name "用户名"   
git config --global user.email "注册邮箱"  

# 查看配置是否生效   
git config --list  
```  

### 访问令牌  

在`个人设置`→`安全设置`之类的地方找到`访问令牌`，生成新令牌（勾选 `repo` 权限），复制并保存生成的令牌字符串（该字符串只显示一次，务必保存好）  

## Fork   

Fork 相当于「把官方仓库复制一份到自己的账号下」，是参与项目的第一步。    
页面右上角的 **Fork** 按钮，选择 Fork 到自己的账号下。  

## Clone    

克隆 = 「把远程仓库下载到本地」，**必须克隆「自己 Fork 后的仓库」，而非官方仓库**。  

1. 打开本地终端；  

2. 进入想存放代码的文件夹（比如 `cd ~/code`，表示进入用户目录下的 code 文件夹）；  

3. 执行克隆命令（替换成自己 Fork 后的仓库地址）
```bash  
git clone https://******.git   
```

4. 进入克隆后的仓库目录  
```bash  
cd 仓库名
```

## 配置上游官方仓库  

（该动作仅需 1 次）目的是让本地仓库「知道官方仓库的地址」，方便后续同步。  

1. 确保终端在仓库目录下（已执行 `cd 仓库名` ）；  

2. 执行添加命令  
```bash  
git remote add upstream 官方仓库地址
```  

3. 验证是否添加成功
```bash  
git remote -v  
```

正常的输出大概是：  
```bash    
    origin    https://***.com/我的账号/***.git (fetch)
    origin    https://***.com/我的账号/***.git (push)
    upstream    https://***.com/官方账号/***.git (fetch)
    upstream    https://***.com/官方账号/***.git (push)
```  

## 同步官方仓库的最新代码  

官方仓库会不断更新，**每次本地开发前，先同步上游代码**，避免本地的修改和官方代码冲突。  

1. 确保当前在仓库根目录；  

2. 拉取官方仓库的最新代码（只下载，不修改本地文件）  
```bash  
git fetch upstream  
```

3. 切换到本地**主分支**（master/main）  
```bash  
git checkout master
```  

4. 合并官方最新代码到本地主分支（最终同步）  
```bash  
git merge upstream/master
```

5. 验证同步成功  
```bash  
git log --oneline  # 同步上游代码后，验证本地是否是官方最新版  
```

## 创建并切换到开发分支  

严禁在主分支（master/main）修改代码，因为这会导致代码混乱。    
所以需要新建一个**专属分支**用来修改/更新，任务完成后直接提交这个分支即可。  

### 分支命名规范  

格式：`issue-编号-简单描述`
示例：
- 修复文档：`issue-1234-fix-docs`
- 修复错别字：`issue-5678-fix-typo`
- 完善注释：`issue-9012-add-comment`

> Issue 数字编号方便之后关联任务  

### 创建 + 切换分支  

```bash  
git checkout -b issue-1234-fix-docs  
```

- `checkout`：切换分支  
- `-b`：create branch（创建新分支）  
- 自定义分支名    

**验证分支切换成功**   

执行命令查看当前所在分支：
```bash  
git branch  
```

示例效果：  
```bash  
* issue-1234-fix-docs # 前面带*的分支，就是当前正在使用的开发分支  
  master  
```

## 开发  

```bash  
# 打开需要修改的文件（根据Issue要求，例：修改README.md）
vim README.md

# Vim 基础操作（新手必看）
# 按 i  → 进入编辑模式
# 修改内容（修复错别字、补全注释、调整格式等）
# 按 Esc → 退出编辑模式
# 输入 :wq → 保存并退出
# 输入 :q! → 不保存退出（放弃修改）

# 查看修改内容（确认修改正确）
git diff  
```
## 提交与推送  

当我们完成本地开发后，需要把修改推到自己 Fork 的仓库（ origin 仓库）。  

```bash  
# 1. 添加修改的文件到暂存区
git add 文件名  # 例：git add README.md
# 或 git add . （添加所有修改文件）  

# 2. 提交修改（注意社区规范）
# 提交单行msg
git commit -m "docs: fix typo in README (fix #1234)"   

# 提交多行msg  
git commit # 自动弹出vim编辑  

# 想要修改msg
git commit --amend

# 验证提交的信息  
git log -1  

# 3. 推送到自己的 Fork 仓库（origin）
git push origin 本地分支名  
```  

如果已经push，想要修改msg：  
```bash  
# 先修改 msg（同上）  
git commit --amend # Vim 修改 → :wq 保存  

# 强制同步到远程  
git push origin 本地分支名 --force-with-lease  
```

推送时需要认证个人访问令牌，终端会提示：  

```bash  
Username for 'https://github.com': 输入github用户名   
Password for 'https://xxx@github.com': 输入之前保存的个人访问令牌  
```

## 提交 PR    

1. 在网页打开自己 Fork 的仓库：`https://github.com/你的用户名/仓库名`  

2. `Pull Requests` → `新建 Pull Request`  

3. 配置参数：
	
	- 目标仓库：`官方账户/官方仓库`（官方）  
	- 目标分支：`master`
	- 源仓库：你的个人 Fork
	- 源分支：你创建的分支（例如`issue-1234-fix-docs`）  

4. 填写 PR 标题 / 描述 （注意社区规范）  

5. 点击 `创建 Pull Request`，完成提交。  

## PR 审查与修改  

维护者会审核提交的代码，若需要修改已经提交pr的内容：  

```bash  
# 1. 切换回到本地开发分支
git checkout issue-1234-fix-docs

# 2. Vim修改文件
vim README.md

# 3. 提交并推送
git add README.md
git commit --amend --no-edit
git push origin issue-1234-fix-docs --force-with-lease
```

解释：  
```bash  
git commit --amend --no-edit  
# 把新修改的代码，合并到上一次的提交里，不新增任何提交记录，保持代码历史干净整洁。
# git commit：提交代码
# --amend：修改/追加到「最后一次提交」（不新建提交）
# --no-edit：不修改提交说明，直接沿用原来的备注
```  

```bash  
git push origin issue-1234-fix-docs --force-with-lease  
# 安全地把修改后的代码，强制同步到远程的分支上（因为修改了历史提交，普通 git push 会报错）  
# git push：推送代码
# origin：自己的个人 Fork 仓库
# issue-1234-fix-docs：自己的开发分支名
# --force-with-lease：安全强制推送  
```

## PR 合并后的清理  

贡献完成后，清理本地分支：  

```bash  
# 切换回主分支
git checkout master

# 同步官方最终代码
git fetch upstream
git rebase upstream/master

# 删除本地开发分支
git branch -d issue-1234-fix-docs

```


## FAQ   

1. 同步时提示 “有冲突” 怎么办？  
    先执行 `git stash` 暂存自己的修改，同步完上游代码后，再执行 `git stash pop` 恢复自己的修改，手动解决冲突（冲突文件里会有 `<<<<<` `=====` `>>>>>` 标记，保留需要的代码即可）。  

2. 如果不小心加错 upstream 地址？  
    执行 `git remote remove upstream` 删除错误的，再重新执行 `git remote add upstream 正确地址` 即可。    

3. **提示令牌权限不足**怎么办？    
	重新生成令牌，务必勾选 `repo` 全权限。  

4. 修改PR后，需要重新「新建 Pull Request」吗？    
	不用。修改代码后，推送的是**同一个分支**，原来创建的 PR，会**自动同步最新代码**，维护者能直接看到修改后的内容。  

5. 解决 VMware 中 openEuler 24.03 无法粘贴外部内容的问题  

```bash  
# 安装 / 修复 open-vm-tools  
# 1. 更新系统
sudo dnf update -y

# 2. 卸载旧版本（如有）
sudo dnf remove open-vm-tools* -y

# 3. 安装完整组件
sudo dnf install open-vm-tools open-vm-tools-desktop -y

# 4. 重启服务
sudo systemctl restart vmtoolsd

# 5. 重启系统（确保生效）
sudo reboot
```

## 文章小结  

本文梳理了`Good First Issue` 全流程贡献指南：    

「 Fork 克隆→配上游→建分支→改代码→推送到自己仓库→提 PR 」  

包括部分 Git 命令、开源规范、令牌使用、冲突解决以及FAQ。  


