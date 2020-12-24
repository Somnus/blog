### 搭建开发环境
- 下载安装包，打开[官网](https://www.python.org/downloads/),选择最新Windows Installer版本下载。
- 运行安装包，勾选`Add Python 3.8 to Path`，选择`Install Now`，等待安装完成，直接关闭。
- 进入cmd界面，输入`python`，检查是否进入了Python输入界面。
- 编码可以使用 `IDLE(Python 3.8 32-bit)`shell工具，可以使用`Pthon 3.8(23-bit)`cmd命令工具，也可以使用其他IDE工具。
- 包管理工具，Python 2.7.9 + 或 Python 3.4+ 以上版本都自带 pip 工具。进入cmd界面输入`pip --version`查看是否返回版本号信息即可判断是否安装成功。

---

### pip常用命令
- 显示版本和路径：`pip --version`
- 获取帮助：`pip --help`
- 升级pip：`pip isntall -U pip`
- 安装包最新版本：`pip install SomePackage`
- 安装包指定版本：`pip install SomePackage==1.0.4`
- 安装包最小版本：`pip install SomePackage>=1.0.4`
- 升级包：`pip isntall --upgrade SomePackage`
- 卸载包：`pip uninstall SomePackage`
- 搜索包（模糊查询）：`pip search SomePackage`
- 显示安装包信息：`pip show`
- 查看指定安装包的详细信息：`pip show -f SomePackage`
- 列出已安装的包：`pip list`

---

### venv:当前项目虚拟环境，将项目引用到的库下载安装到当前项目而非系统环境，以解决不同项目引用同一库时版本问题。