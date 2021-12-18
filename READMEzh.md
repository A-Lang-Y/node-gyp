node-gyp - Node.js 原生插件构建工具
构建状态 新产品经理

node-gyp是一个用 Node.js 编写的跨平台命令行工具，用于为 Node.js 编译本机插件模块。它包含Chromium 团队以前使用的 gyp -next项目的供应商副本，经过 扩展以支持 Node.js 原生插件的开发。

请注意，node-gyp它不用于构建 Node.js 本身。

支持 Node.js 的多个目标版本（即0.8, ..., 4, 5,6等），无论您的系统上实际安装的是哪个版本的 Node.js（node-gyp下载目标版本所需的开发文件或头文件） .

特征
相同的构建命令适用于任何受支持的平台
支持针对不同版本的 Node.js
安装
您可以node-gyp使用npm以下方法安装：

npm install -g node-gyp
根据您的操作系统，您需要安装：

在 Unix 上
Python v3.6、v3.7、v3.8 或 v3.9
make
一个合适的 C/C++ 编译器工具链，比如GCC
在 macOS 上
注意：如果您的 Mac 已升级到 macOS Catalina (10.15)，请阅读macOS_Catalina.md。

Python v3.6、v3.7、v3.8 或 v3.9
Xcode
您还需要XCode Command Line Tools通过运行来安装xcode-select --install. 或者，如果您已经安装了完整的 Xcode，您可以在菜单下找到它们Xcode -> Open Developer Tool -> More Developer Tools...。这一步将安装clang，clang++和make。
在 Windows 上
从Microsoft Store 包安装当前版本的 Python 。

手动安装工具和配置：

安装 Visual C++ 构建环境：Visual Studio 构建工具 （使用“Visual C++ 构建工具”工作负载）或Visual Studio 社区 （使用“使用 C++ 进行桌面开发”工作负载）
启动cmd， npm config set msvs_version 2017
如果上述步骤对您不起作用，请访问Microsoft 的 Windows Node.js 指南以获取其他提示。

要在 ARM 上的 Windows 10 上以本机 ARM64 Node.js 为目标，请添加组件“Visual C++ compilers and libraries for ARM64”和“Visual C++ ATL for ARM64”。

配置 Python 依赖
node-gyp要求您已安装兼容的 Python 版本，其中之一是：v3.6、v3.7、v3.8 或 v3.9。如果您安装了多个 Python 版本，您可以node-gyp通过以下方式之一确定应使用哪个 Python 版本：

通过设置--python命令行选项，例如：
node- gyp <命令> --python /path/to/executable/python
如果node-gyp通过 调用npm，并且您安装了多个版本的 Python，那么您可以将npm'python' 配置键设置为适当的值：
npm 配置设置python /path/to/executable/python
如果PYTHON环境变量设置为 Python 可执行文件的路径，则将使用该版本（如果它是兼容版本）。

如果NODE_GYP_FORCE_PYTHON环境变量设置为 Python 可执行文件的路径，则将使用它而不是任何其他配置或内置的 Python 搜索路径。如果它不是兼容的版本，则不会进行进一步的搜索。

为第三方 Node.js 运行时构建
当为第三方 Node.js 运行时（如 Electron）构建模块时，这些模块具有与官方 Node.js 发行版不同的构建配置，您应该使用--dist-url或--nodedir标志来指定要构建的运行时的标头。

此外，当--dist-url或--nodedir标志被传递时，node-gyp 将使用config.gypi头文件分发中的 交付来生成构建配置，这与使用process.config正在运行的 Node.js 实例的对象的默认模式不同 。

一些旧版本的 Electronconfig.gypi在其标头分发中的格式不正确，您可能需要传递--force-process-config给 node-gyp 以解决配置错误。

如何使用
要编译你的原生插件，首先进入它的根目录：

cd my_node_addon
下一步是为当前平台生成适当的项目构建文件。用途configure为：

节点gyp配置
Visual C++ Build Tools 2015 的自动检测失败，因此--msvs_version=2015 需要添加（如上面配置的由 npm 运行时不需要）：

node-gyp 配置 --msvs_version=2015
注意：该configure步骤binding.gyp在当前目录中查找要处理的文件。有关创建binding.gyp文件的说明，请参见下文。

现在您将在目录中有一个Makefile（在 Unix 平台上）或一个vcxproj文件（在 Windows 上）build/。接下来，调用build命令：

节点gyp构建
现在你有了编译好的.node绑定文件！编译后的绑定以build/Debug/或结尾build/Release/，具体取决于构建模式。此时，您可以.node使用 Node.js要求该文件并运行您的测试！

注意：要创建绑定文件的调试版本，请在运行,或命令时传递--debug（或 -d）开关。configurebuildrebuild

该binding.gyp文件
一个binding.gyp文件以类似 JSON 的格式描述了构建模块的配置。该文件放置在包的根目录中，与 package.json.

gyp适用于构建 Node.js 插件的准系统文件可能如下所示：

{
   “目标”：[
    {
      “目标名称”：“绑定”，
       “来源”：[ “src/binding.cc” ]
    }
  ]
}
进一步阅读
docs目录包含有关特定 node-gyp 主题的附加文档，如果您在使用 node-gyp 安装或构建插件时遇到问题，这些文档可能会很有用。

Node.js 原生插件和编写gyp配置文件的一些额外资源：

“Going Native” nodeschool.io 教程
“Hello World”节点插件示例
gyp 用户文档
gyp 输入格式参考
“binding.gyp”文件在野生维基页面中
命令
node-gyp 响应以下命令：

命令	描述
help	显示帮助对话框
build	调用make/msbuild.exe并构建本机插件
clean	删除build目录（如果存在）
configure	为当前平台生成项目构建文件
rebuild	运行clean，configure并且build全部连续
install	安装给定版本的 Node.js 头文件
list	列出当前安装的 Node.js 头文件版本
remove	删除给定版本的 Node.js 头文件
命令选项
node-gyp 接受以下命令选项：

命令	描述
-j n, --jobs n	make并行运行。该值max将使用所有可用的 CPU 内核
--target=v6.2.1	要为其构建的 Node.js 版本（默认为process.version）
--silly, --loglevel=silly	将所有进度记录到控制台
--verbose, --loglevel=verbose	将大部分进度记录到控制台
--silent, --loglevel=silent	不要将任何内容记录到控制台
debug, --debug	进行调试构建（默认为Release）
--release, --no-debug	发布版本
-C $dir, --directory=$dir	在不同目录下运行命令
--make=$make	覆盖make命令（例如gmake）
--thin=yes	启用瘦静态库
--arch=$arch	设置目标架构（例如 ia32）
--tarball=$path	从本地 tarball 获取标头
--devdir=$path	SDK下载目录（默认为OS缓存目录）
--ensure	如果标题已经存在，请不要重新安装
--dist-url=$url	从自定义 URL 下载标头 tarball
--proxy=$url	设置用于下载标头 tarball 的 HTTP(S) 代理
--noproxy=$urls	在下载标头 tarball 时将 url 设置为忽略代理
--cafile=$cafile	覆盖默认 CA 链（下载 tarball）
--nodedir=$path	设置节点源码的路径
--python=$path	设置 Python 二进制文件的路径
--msvs_version=$version	设置 Visual Studio 版本（仅限 Windows）
--solution=$solution	设置 Visual Studio 解决方案版本（仅限 Windows）
--force-process-config	强制使用运行时的process.config对象生成config.gypi文件
配置
环境变量
使用npm_config_OPTION_NAME上面列出的任何命令选项的形式（选项名称中的破折号应替换为下划线）。

例如，要设置devdir等于/tmp/.gyp，您将：

在 Unix 上运行：

导出npm_config_devdir=/tmp/.gyp
或者在 Windows 上：

设置 npm_config_devdir=c:\temp\.gyp
npm 配置
将表单OPTION_NAME用于上面列出的任何命令选项。

例如，要设置devdir等于/tmp/.gyp，您将运行：

npm 配置集[--global] devdir /tmp/.gyp
注意：配置设置 vianpm只会在node-gyp run vianpm时使用，而不是node-gyp直接 run 时。

执照
node-gyp可在 MIT 许可下使用。有关详细信息，请参阅许可证文件。
