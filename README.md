# Web Application: Blog

A web app development practice from michaelliao's python course.

根据 [廖雪峰 Python 教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001432170876125c96f6cc10717484baea0c6da9bee2be4000) 进行的练习项目，实现一个 MVC 架构博客站点。

* 通过 jinja2 实现前端模板，就可以根据 http 请求返回相应的 html 页面。
* 该 web 应用通过异步的方式处理 http 请求，因此，对底层库 aiohttp 进行封装，实现一个 web 应用框架。
* 再由 aiohttp 和 aiomysql 实现 ORM 框架，以便通过 Python 的类来操作 MySQL 数据库。
* 基于以上实现的框架进行 web 应用的业务功能开发，主要是用户管理和博文管理。

## Day 1 - build a development environment

开发环境：Mac + VS Code + Python 3.7.1 + 虚拟环境

### 使用 pyenv 安装和管理不同版本的 Python

相关资料：

* [Simple Python Version Management: pyenv](https://github.com/pyenv/pyenv) - 看 README file 进行安装和使用

```bash
# 查看可安装的 Python 版本
pyenv install --list

# 安装 Python 3.7.1
pyenv install 3.7.1

# 在项目路径指定 Python 版本
cd /path/to/your/project
pyenv local 3.7.1
```

### 使用 pipenv 创建虚拟环境，安装和维护第三方库以及相关依赖

相关资料：

* [Pipenv: Python Development Workflow for Humans](https://github.com/pypa/pipenv) - 看 README file 进行安装和使用
* [使用 pipenv 管理你的项目](https://zhuanlan.zhihu.com/p/32913361)
* [求教 pipenv 到底优势在哪里？](https://www.v2ex.com/t/432899)
* [Advanced Usage of Pipenv](https://pipenv.readthedocs.io/en/latest/advanced/#configuration-with-environment-variables) - 结合环境变量设置来使用 pipenv

```bash
# 创建虚拟环境，并生成依赖管理文件 Pipfile 和 Pipfile.lock
pipenv --python 3.7.1

# 编辑 Pipfile 文件，将第三方库源换成清华源
# 将 url = "https://pypi.org/simple"
# 修改为 url = "https://pypi.tuna.tsinghua.edu.cn/simple"

# 激活虚拟环境
pipenv shell

# 安装第三方库 aiohttp jinja2 aiomysql
pipenv install aiohttp jinja2 aiomysql
```

### 根据项目结构，建立工作目录

需要开源代码的话，根据需求选择合适的开源证书就好了，本项目选用 FreeBSD 证书

相关资料：

* [如何选择开源许可证？](http://www.ruanyifeng.com/blog/2011/05/how_to_choose_free_software_licenses.html)
* [FreeBSD License](https://en.wikipedia.org/wiki/BSD_licenses)

```text
web_app/
        blog/
             static/
             templates/
LICENSE
```

### 使用 Git 进行版本控制，并将代码放到 GitHub

相关资料：

* [git - 简明指南](http://rogerdudler.github.io/git-guide/index.zh.html)
* [A collection of .gitignore templates](https://github.com/github/gitignore) - [参考 Python 部分](https://github.com/github/gitignore/blob/master/Python.gitignore)

```bash
git init

# 创建 .gitignore 文件，参考 python.gitignore, 忽略无需关注的文件

# 添加当前目录以及子目录中所有未被 .gitignore 包括的文件

# 添加到暂存区
git add .

# 提交到 HEAD
git commit -m "Day 1 - build a development environment"

# 先在 GitHub 创建一个仓库
# 关联 GitHub 仓库
git remote add origin git@github.com:justoneliu/web_app.git

# 将代码推送到 GitHub
git push origin master
```
