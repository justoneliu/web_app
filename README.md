# Web 应用开发：博客站点

根据 [廖雪峰 Python 教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001432170876125c96f6cc10717484baea0c6da9bee2be4000) 进行的练习项目，实现一个 MVC 架构的博客站点。

整个项目的开发工作主要分为五部分：

1. 选用模板语言库 [jinja2](http://jinja.pocoo.org/docs/2.10/) 制作前端模板，以便于根据 http 请求返回相应的 html 页面
2. 对异步 HTTP 客户端/服务端库 [aiohttp](https://aiohttp.readthedocs.io/en/stable/) 进行封装，实现一个异步 Web 框架来异步处理各种 HTTP 请求
3. 选用异步 MySQL 驱动库 [aiomysql](https://aiomysql.readthedocs.io/en/latest/) 来制作一个 ORM 框架，使 Python 的类操作映射为 MySQL 数据库的添加、读取、修改和删除（CRUD），并通过异步的方式访问数据库
4. 基于以上框架进行博客站点的业务功能开发，主要为用户模块和文章模块
5. 项目的部署和运维，使用 Linux Nginx MySQL Python 架构，简称 LNMP

## Day 1 - 搭建开发环境

要点：

1. 学习并构思项目结构，以便于有条理的编排代码、功能、模块
2. 学习开发环境的管理：Python 版本，虚拟环境，第三方库以及相关依赖
3. 学习代码版本控制 Git

本项目的开发环境为：macOS + VS Code + MySQL + Python 3.7.1 的虚拟环境。

### 根据项目结构，建立工作目录

除了按教程的项目结构来创建工作目录，还能参照 GitHub 上的一些优秀开源项目的结构，一个良好的项目结构使代码的管理更轻松，可读性更强。

```text
web_app/
        blog/
             static/
             templates/
LICENSE
```

需要开源代码的话，根据需求选择合适的开源证书即可。本项目使用 [FreeBSD](https://en.wikipedia.org/wiki/BSD_licenses) 证书。

拓展资料：

* [如何选择开源许可证？](http://www.ruanyifeng.com/blog/2011/05/how_to_choose_free_software_licenses.html)

### Python 版本的管理

如何让系统安装多个 Python 版本而不引起混乱和错误？推荐使用 Python 版本管理库 [pyenv](https://github.com/pyenv/pyenv) 来安装、管理不同版本的 Python，pyenv 不仅可以便捷地安装、切换不同 Python 版本，还能指定系统全局使用的 Python 版本，或指定项目目录所使用的 Python 版本。

```bash
# 安装 pyenv
brew install pipenv

# 查看可安装的 Python 版本
pyenv install --list

# 安装 Python 3.7.1，可更改版本号来安装其它版本
pyenv install 3.7.1

# 查看已安装的 Python 版本
pyenv versions

# 指定项目目录使用 Python 3.7.1
cd /path/to/your/project
pyenv local 3.7.1

# 检查 Python 版本是否切换成功
python -V
```

### 创建虚拟环境，安装和维护第三方库及其依赖

为避免因安装、卸载第三方库，或误操作导致 Python 崩溃，甚至系统崩溃，请使用 Python 虚拟环境进行开发。Python 虚拟环境的本质是将 Python 进行拷贝，通常存放在你的当前目录，并提供命令来管理 Python 虚拟环境，在此环境中的操作不会影响系统中的其它 Python 版本。

在安装、卸载第三方库时，多数第三方库会依赖其它第三方库，虽然 [pip](https://pip.pypa.io/en/stable/installing/) 可以便捷地安装第三方库及其依赖，但是卸载库时，pip 并不会卸载其依赖。此外，这些依赖有可能是项目中用到的其它第三方库的依赖，因此，卸载前还要检查是否其它库的依赖，否则容易引发错误。还有，在部署阶段，需要一个包含第三方库信息的 requirement.txt 文件来快速安装生产环境的库，如果仅用 pip freeze 来生成 requirement.txt 文件的话，不能直接区分哪些库是开发、测试、生产专用的，哪些是通用的。

针对以上情况，推荐使用依赖管理库 [pipenv](https://pipenv.readthedocs.io/en/latest/)，pipenv 不仅可以创建虚拟环境，还能很方便的管理、维护第三方库及其依赖，也很容易区分库的使用环境。

拓展资料：

* [Advanced Usage of Pipenv](https://pipenv.readthedocs.io/en/latest/advanced/#configuration-with-environment-variables) - 通过设置相关的环境变量来使用 pipenv

```bash
# 安装 pipenv
brew install pipenv

# 创建 Python 3.7.1 虚拟环境，会自动生成依赖管理文件 Pipfile 和 Pipfile.lock
pipenv --python 3.7.1

# 编辑 Pipfile 文件，将第三方库源换成清华源，以提高 pipenv 的处理速度
# 将 url = "https://pypi.org/simple"
# 修改为 url = "https://pypi.tuna.tsinghua.edu.cn/simple"

# 激活虚拟环境
pipenv shell

# 输入 exit 可退出虚拟环境

# 在虚拟环境中安装第三方库 aiohttp jinja2 aiomysql
pipenv install aiohttp jinja2 aiomysql

# 在开发环境中安装代码检查库 pylint pep8
pipenv install pylint pep8 --dev
```

### 使用 Git 进行版本控制，并将代码放到 GitHub

相关资料：

* [git - 简明指南](http://rogerdudler.github.io/git-guide/index.zh.html)
* [A collection of .gitignore templates](https://github.com/github/gitignore) - [参考 Python 部分](https://github.com/github/gitignore/blob/master/Python.gitignore)

```bash
# 创建仓库
git init

# 新建一个 .gitignore 文件，可参考 GitHub 上的 python.gitignore, 忽略无需关注的文件

# 将当前目录以及子目录中所有未被 .gitignore 包括的文件添加到暂存区
git add .

# 提交到 HEAD
git commit -m "Day 1 - 搭建开发环境"

# 先在 GitHub 网站上创建一个仓库

# 将本地的代码库关联 GitHub 的仓库
git remote add origin git@github.com:justoneliu/web_app.git

# 将代码推送到 GitHub
git push origin master
```
