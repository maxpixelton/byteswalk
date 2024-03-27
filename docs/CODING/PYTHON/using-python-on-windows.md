# 在 Windows 上使用 Python


与大多数 Unix 系统和服务不同，Windows 未包括任何受系统支持的 Python 预安装版。

为让 Python 可用，CPython 团队为每个 [发布版](https://www.python.org/downloads/) 编译了 Windows 安装程序，这些安装程序主要被用来安装用户级 Python 安装版，包含给单独用户使用的核心解释器和库。安装程序也能够为单独机器上的所有用户进行安装，还提供了针对应用程序本地分发版的单独 ZIP 文件。

如 [PEP 11](https://peps.python.org/pep-0011/) 所述，Python 发布版对某个 Windows 平台的支持仅限于被 Microsoft 视为处于延长支持周期内的版本，即 Python 3.12 支持 Windows 8.1 及其后的版本。如果需要 Windows 7 支持，请安装 Python 3.8。

Windows 提供了下面安装程序：

- [完成的安装程序](#完整安装程序) 包含所有组件，对于使用 Python 进行任何类型项目的开发人员而言，它是最佳选择。 

- [Microsoft Store 包](#microsoft-sotre-包) 适用于运行脚本和包，并使用 IDLE 或其他开发环境的简易 Python 安装版。 它需要 Windows 10 或更新的系统，但可以安全地安装而不会破坏其他程序。 它还提供了许多便捷命令用来启动 Python 及其工具。

- [nuget.org 安装包](#nugetorg-安装包) 用于持续集成系统的轻量级安装。它可用于构建Python包或运行脚本，但不可更新且没有用户界面工具。

- [可嵌入的包](#可嵌入的包) Python的最小安装包，适合嵌入到更大的应用程序中。


## 完整安装程序

### 安装步骤

有 4 个安装程序可供下载 - 32 位和64 位版本的各有两个

- web installer（网络安装包）是一个小的初始化工具，它将在安装过程中，根据需要自动下载所需的组件。

- offline installer（离线安装包）内含默认安装所需的组件，可选择功能仍需要 Internet 连接下载。

启动安装程序后，可以选择以下两个选项之一：
![Install Python On Windows](https://shichuan-hao.github.io/images/python/installed-python-on-windows.png)

如果选择 “Install Now（立即安装）”：

- 不需要成为管理员（除非需要对C运行库进行系统更新，或者为所有用户安装[适用于Windows的Python启动器](#适用于-windows-的-python-启动器)

- Python 将安装到您的用户目录中

- [适用于Windows的Python启动器](#适用于-windows-的-python-启动器)将根据第一页底部的选项安装

- 将安装标准库，测试套件，启动器和 pip

- 如果选择将安装目录将添加到 PATH

- 快捷方式仅对当前用户可见

选择“自定义安装”将允许您选择：要安装的功能、安装位置、其他选项或安装后的操作。如果要安装调试符号或二进制文件，我们需要使用此选项。

如要为全部用户安装，应选择“自定义安装”。在这种情况下:

- 您可能需要提供管理凭据或批准

- Python 将安装到 Program Files 目录中

- [适用于Windows的Python启动器](#适用于-windows-的-python-启动器)将安装到Windows目录中

- 安装期间可以选择可选功能

- 标准库可以预编译为字节码

- 如果选中，安装目录将添加到系统 PATH

- 快捷方式所有用户可用

### 删除 MAX_PATH 限制

历史上Windows的路径长度限制为260个字符。这意味着长于此的路径将无法解决并导致错误。

在最新版本的 Windows 中，此限制可被扩展到大约 32,000 个字符。 但需要让管理员激活“启用 Win32 长路径”组策略，或在注册表键 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem` 中设置 `LongPathsEnabled` 为 1。

这允许 [`open()`](https://docs.python.org/zh-cn/3.12/library/functions.html#open) 函数，[`os`](https://docs.python.org/zh-cn/3.12/library/os.html#module-os) 模块和大多数其他路径功能接受并返回长度超过 260 个字符的路径。

更改上述选项后，无需进一步配置。

在 3.6 版本发生变更: Python中启用了对长路径的支持。

### 无 UI 安装

略！

### 免下载安装

略！


### 修改安装

安装Python后，您可以通过Windows中的“程序和功能”工具添加或删除功能。选择Python条目并选择“卸载/更改”以在维护模式下打开安装程序。

“修改” 允许您通过修改复选框来添加或删除功能 - 未更改的复选框将不会安装或删除任何内容。在此模式下无法更改某些选项，例如安装目录；要修改这些，您需要完全删除然后重新安装Python。

“修复” 将使用当前设置验证应安装的所有文件，并替换已删除或修改的任何文件

“卸载” 将完全删除Python，但[适用于Windows的Python启动器](#适用于-windows-的-python-启动器)除外，它在“程序和功能”中有自己的条目。

## Microsoft Sotre 包

略！

## nuget.org 安装包

略！

## 可嵌入的包

略！

## 替代捆绑包

除标准的 CPython 发行版之外，还有些含附加功能的修改包，以下热门版本及其主要功能列表：

- [ActivePython](https://www.activestate.com/products/python/) 具有多平台兼容性的安装程序，文档，PyWin32

- [Anaconda](https://www.anaconda.com/download/) 流行的科学模块（如numpy，scipy和pandas）和 conda 包管理器。

- [Enthought 部署管理器](https://assets.enthought.com/downloads/edm/)，“下一代的 Python 环境和包管理器”，之前 Enthought 提供了 Canopy，但已经 于 2016 年结束生命期。

- [WinPython](https://winpython.github.io)，特定于Windows的发行版，包含用于构建包的预构建科学包和工具。

请注意，这些软件包可能不包含最新版本的 Python 或其他库，并且不由核心 Python 团队维护或支持。


## 配置 Python

要从命令提示符方便地运行Python，我们可以考虑在Windows中更改一些默认环境变量。虽然安装程序提供了为您配置PATH和PATHEXT变量的选项，但这仅适用于单版本、全局安装。如果经常使用多个版本的Python，我们可以考虑使用[适用于Windows的Python启动器](#适用于-windows-的-python-启动器)。

### 设置环境变量

Windows允许在用户级别和系统级别永久配置环境变量，或临时在命令提示符中配置环境变量。

要临时设置环境变量，请打开命令提示符并使用 set 命令：

```shell
C:\>set PATH=C:\Program Files\Python 3.9;%PATH%
C:\>set PYTHONPATH=%PYTHONPATH%;C:\My_python_lib
C:\>python
```

这些环境变量的更改将应用​​于在该控制台中执行的任何其他命令，并且，由该控制台启动的任何应用程序都继承这些设置。

在百分号中包含的变量名将被现有值替换，允许在开始或结束时添加新值。通过将包含 python.exe 的目录添加到开头来修改 PATH 是确保启动正确版本的Python的常用方法。

要永久修改默认环境变量，请单击“开始”并搜索“编辑环境变量”，或打开系统属性的 高级系统设置 ，然后单击 环境变量 按钮。在此对话框中，您可以添加或修改用户和系统变量。要更改系统变量，您需要对计算机进行无限制访问（即管理员权限）。

!!! notes

    备注 Windows会将用户变量串联在系统变量 之后 ，这可能会在修改 PATH 时导致意外结果。
    [PYTHONPATH](https://docs.python.org/zh-cn/3.12/using/cmdline.html#envvar-PYTHONPATH) 变量被 Python 的所有版本使用，因此除非它列出的路径只包含与所有已安装的 Python 版本兼容的代码，否则不要永久配置此变量。

!!! Tip
    
    - [Windows 中的环境变量概述](https://docs.microsoft.com/en-us/windows/win32/procthread/environment-variables)

    - [用于临时修改环境变量的 set 命令]https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/set_1

    - [用于永久修改环境变量的 setx 命令]https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/setx

### 查找 Python 可执行文件

在 3.5 版本发生变更。

除了使用自动创建的Python解释器的开始菜单项之外，您可能还想在命令提示符下启动Python。安装程序有一个选项可以为您设置。

在安装程序的第一页上，可以选择标记为“将Python添加到环境变量”的选项，以使安装程序将安装位置添加到 PATH 。还添加了 `Scripts\` 文件夹的位置。这允许你输入 python 来运行解释器，并且 pip 用于包安装程序。因此，您还可以使用命令行选项执行脚本，请参阅 命令行 文档。

如果在安装时未启用此选项，则始终可以重新运行安装程序，选择“修改”并启用它。或者，您可以使用 附录：设置环境变量 的方法手动修改 PATH 。您需要将Python安装目录添加到 PATH 环境变量中，该内容与其他条目用分号分隔。示例变量可能如下所示（假设前两个条目已经存在）:

## 适用于 Windows 的 Python 启动器

在 3.3 版本加入。

用于 Windows 的 Python 启动器是一个实用程序，可以帮助我们定位和执行不同的 Python 版本，它允许脚本（或命令行）指示特定 Python 版本的首选项，并将定位并执行该版本。

与 PATH 变量不同，启动器将正确选择合适的 Python 版本，它更倾向于用户安装而不是系统安装，并按语言版本排序，而不是使用最新安装的版本。

启动器最初是在[PEP 397](https://peps.python.org/pep-0397/) 中指定的。