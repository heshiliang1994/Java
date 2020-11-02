### 一、忽略部分文件
.gitignore文件的语法规则：
- 忽略文件夹：/文件夹名/
- 忽略指定文件：

如果.gitignore不生效，使用以下命令清除git缓存：
git rm -r --cached 指定文件夹
git add .
git commit -m 'update .gitignore'

### 二、新建项目
初始化：git init
添加文件：git add .
提交到本地缓存：git commit -m "提交"
和远程库关联：git remote add origin https://github.com/heshiliang1994/learning.git
提交到远程库：git push -u origin master

从远程库下载：git pull origin master

### 三、大文件无法上传，使用git large file storage
git lfs install
git lfs track "路径或文件"
git add .
git commit -m "备注"
git push origin master
