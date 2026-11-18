# cmake
cmake的学习记录

1. 1个小例子

```
cmake_minimum_required(VERSION 3.10)
project(test1)
add_executable(test1
    src/main.cpp
)

# 注: cmake_minimum_required(),project(),add_executable() 这些都是cmake的都是命令，PROJECT_NAME叫做变量
```

1. cmake_minimum_required()，**指定版本约束信息**：

```
cmake_minimum_required(VERSION <min> [... <policy_max>] [FATAL_ERROR])

VERSION：			关键字
min：				约束的最小版本
[... <policy_max>]：	策略版本上限(可选)
[FATAL_ERROR]：		（废弃）（可选）

# 注: 例如，cmake_minimum_required(VERSION 3.10...3.25),指定版本上限是3.25
```



1. project(),**声明当前项目的“身份信息”和“编译语言环境”**：

```
project(<PROJECT-NAME>
        [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
        [DESCRIPTION <项目描述>]
        [HOMEPAGE_URL <主页URL>]
        [LANGUAGES <语言>...])
        
<PROJECT-NAME>：	项目名称
VERSION：		版本号（可选）
DESCRIPTION：	项目简短描述
HOMEPAGE_URL：	项目主页地址
LANGUAGES：		指定使用的编程语言，编译根据这个选择编译器,如果不添加，会默认使用C和C++的编译器，

# 注: 这也体现了和C++等编程中函数的区别，命令中指定关键字后跟参数就行，比如指定VERSION后，可以不指定DESCRIOPTION,直接指定LABGUAGES。
# 注:	project(ImageTool
        	VERSION 1.0
        	DESCRIPTION "Qt6 + OpenCV image tool"
        	HOMEPAGE_URL "https://github.com/xxx/ImageTool"
        	LANGUAGES CXX)
# 注: 在添加project以后会生成很多参数:
		PROJECT_NAME:			编译生成的项目名称
		CMAKE_PROJECT_NAME:		顶层项目名，如果当前是根目录下的CMakeFiles.txt文件，则等同于PROJECT_NAME
		PROJECT_SOURCE_DIR:		CMakeFile.txt所在的目录
		PROJECT_BINARY_DIR:		当前构建所在的目录
		test1_SOURCE_DIR:		等同于	PROJECT_SOURCE_DIR
		test1_BINARY_DIR:		等同于	PROJECT_BINARY_DIR
		PROJECT_VERSION:		项目版本号，根据上一条注释，当前版本号是 1.0
		PROGECT_VERSION_MAJOR:	当前项目的主版本号，就是 1
		PROJECT_VERSION_MINOR:	当前项目的次版本号，就是 0
	以上参数的的练习使用会在后边添加。
```

1. add_executable() **创建可执行文件目标**：

```
add_executable(<name> [WIN32] [MACOSX_BUNDLE] [EXCLUDE_FROM_ALL] [source1] [source2] ...)

<name>：				生成的程序名
[WIN32]：			Windows 下生成 GUI 程序，不弹出控制台窗口
[MACOSX_BUNDLE]：	macOS 下生成 .app 应用包
[EXCLUDE_FROM_ALL]：	默认不构建，需要显式指定目标才编译
[source1] [source2] ...：参与编译的源文件(头文件也可以写进去，虽然它不直接参与编译，但能让 IDE 正确识别项目结构)
```

三种使用方式:

(1) 普通可执行目标：最常见的使用方式

```
add_executable(test1
    src/main.cpp
)
```

（2）导入可执行文件：引用项目外部的可执行文件，不需要重新编译:

```
add_executable(mytool IMPORTED)
set_target_properties(mytool PROPERTIES
    IMPORTED_LOCATION "/usr/local/bin/mytool"
)
```

(3) 别名目标：给已有目标起一个别名，常用于创建子项目或者库的对外暴露：

```
add_executable(myapp main.cpp)
add_executable(MyApp::app ALIAS myapp)
```

和**target_sources** 的配合：源文件可以一开始不全写，后面用 `target_sources()` 补充：

```
add_executable(test1 src/main.cpp)

target_sources(test1 PRIVATE
    src/utils.cpp
    src/mainwindow.cpp
)
```

注意和**add_library** 的区别:

​	add_executable: 用来生成可执行程序

​	add_library：生成库文件(.a .so .lib .dll)

1. target_sources()：**给已经创建好的目标追加源文件**：

```
target_sources(<目标名>
    PRIVATE 源文件...
    PUBLIC   源文件...
    INTERFACE 源文件...
)

<目标名>： 	生成的可执行程
PRIVATE：  只用于当前目标编译（几乎所有的.cpp都是用这个的）
PUBLIC：	  当前目标 + 依赖它的目标都能继承（几乎不用）
INTERFACE：当前目标不编译，只传给依赖者（常见于接口库、头文件库）

# 注： 对.cpp文件来说，几乎永远用 PRIVATE。因为源文件通常只需要被当前目标编译，不应该传播给其他目标。
```

`target_sources()` 的好处是：当源文件变多、跨目录、需要条件编译时，写法更清晰。

也可以多次追加:

```
add_executable(test1 src/main.cpp)
target_sources(test1 PRIVATE src/a.cpp)
target_sources(test1 PRIVATE src/b.cpp)
```

1. message**打印/调试命令**：

```
message([<mode>] "message text" ...)

[<mode>]：模式（可选）
"message text"：输出信息

[<mode>]:
	[空]：	输出打印内容		
	STATUS:		状态信息，前边会加上--
	WARNING:	告警信息
	SEND_ERROR:	错误信息，继续处理单不会中断
	FATAL_ERROR:	致命错误，立即终端
	DEBUG/VERBOSE/IRACE：   调试信息，默认通常不显示
```

