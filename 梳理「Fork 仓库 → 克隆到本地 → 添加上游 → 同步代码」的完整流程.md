**梳理「Fork 仓库 → 克隆到本地 → 添加上游 → 同步代码」的完整流程**  

## Fork   

Fork 相当于「把官方仓库复制一份到自己的账号下」，是参与项目的第一步。    
页面右上角的 **Fork** 按钮，选择 Fork 到自己的账号下。  

## Clone    

克隆 = 「把远程仓库下载到本地」，**必须克隆「自己 Fork 后的仓库」，而非官方仓库**。  

1. 打开本地终端；  
2. 进入想存放代码的文件夹（比如 `cd ~/code`，表示进入用户目录下的 code 文件夹）；  
3. 执行克隆命令（替换成自己 Fork 后的仓库地址）：`git clone https://******.git`  
4. 进入克隆后的仓库目录：`cd 仓库名`  

## 添加上游（upstream）仓库  

（该动作仅需 1 次）目的是让本地仓库「知道官方仓库的地址」，方便后续同步。  

1. 确保终端在仓库目录下（已执行 `cd 仓库名` ）；  
2. 执行添加命令：`git remote add upstream 官方仓库地址`  
3. 验证是否添加成功：`git remote -v`  

正常的输出大概是：  
```plaintext  
    origin    https://***.com/我的账号/***.git (fetch)
    origin    https://***.com/我的账号/***.git (push)
    upstream  https://***.com/官方账号/***.git (fetch)
    upstream  https://***.com/官方账号/***.git (push)
```  

## 同步官方仓库的最新代码  

官方仓库会不断更新，每次本地开发前，先同步上游代码，避免我们的修改和官方代码冲突。  

1. 确保本地工作区没有未提交的修改（如果有，先执行 `git add .` + `git commit -m "备注"` 保存）；  
2. 拉取 upstream 仓库的最新代码：`git pull upstream 官方仓库默认分支名(一般为mian或master)`    

## 提交自己的修改到自己的 origin 仓库  

当我们完成本地开发后，需要把修改推到自己 Fork 的仓库。  

```
# 1. 添加所有修改的文件到暂存区
git add .  

# 2. 提交修改（备注写清楚）
git commit -m "feat: 修复xx"  

# 3. 推送到自己的 Fork 仓库（origin）
git push origin main
```  

## 其它备注  

1. **同步时提示 “有冲突” 怎么办？**
    先执行 `git stash` 暂存自己的修改，同步完上游代码后，再执行 `git stash pop` 恢复自己的修改，手动解决冲突（冲突文件里会有 `<<<<<` `=====` `>>>>>` 标记，保留需要的代码即可）。
2. **如果不小心加错 upstream 地址？**
    
    执行 `git remote remove upstream` 删除错误的，再重新执行 `git remote add upstream 正确地址` 即可。  

