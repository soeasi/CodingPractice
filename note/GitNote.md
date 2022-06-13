# Git命令

提交更新：	`git commit`

切换分支：	`git checkout <name>`
创建切换：	`git checkout <name> -b`

合并分支：	`git merge <name>`
合并分支：	`git rebase <name>`

$ git config user.name
$ git config user.email

修改用户名和邮箱地址

$  git config --global user.name  "xxxx"
S  git config --global user.email  "xxxx"



# Node.js  



npm list							# 查看当前目录下已安装的Node模块
npm install -g <moudel>				# 安装模块。-g参数表示全局安装，没有-g表示在当前项目安装
npm view <moudel> engines			# 查看模块所以来的Node的版本
npm outdated						# 检查模块是否已经过时，命令会列出所有已过时的包
npm update <moudel>					# 更新模块
npm uninstall <moudel>				# 卸载模块
npm <> -h							# 