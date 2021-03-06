## IDEA Tips and Tricks(Keymap: Mac OS X 10.5+)

### 主操作

1. `Cmd + Shift + A`: Help -> Find Action, 搜索对应操作, 如recent files、Rest Client等
2. 安装`Key Promoter X`插件
4. Help -> Productivity Guide 看下自己没有用哪些feature
5. Help -> Keymap Reference
6. **关闭Navigation Bar和Editor tabs**, inefficiency for navigation
7. JetBrains ToolBox, 可以设置heap size等

### 跳转

1. 编辑区/文件区跳转(Cmd+1, Esc, F4): Cmd+1: 跳转到Project视图, Cmd+7跳转到Structure视图,`Esc`或者`F4`后跳转到主窗口代码. **Hide All Windows(Cmd + Shift + F12)**. **Distraction Free Mode(Ctrl + F12)**. `Shift + Esc`: 关闭当前视图. 
2. 项目跳转
	1. `Cdm + \``: Window -> Next Project Window
	2. `Cmd + Shift + \`: Window -> Previous Project Window
3. 文件跳转
	1. `Cmd + E`: Recent Files(也可以打开其他的window, 比如Spring/TODO window等)
	2. `Cmd + Shift + E`: Recent Changed Files
4. 浏览/修改位置跳转
	1. 修改位置跳转`Cmd + Shift + Delete`: Navigate -> Last Edit Location
	2. 修改位置跳转`Cmd + Ctrl + Delete`: Navigate -> Next Edit Location
	3. 浏览位置跳转`Cmd + [`: Navigate -> Back
	4. 浏览位置跳转`Cmd + ]`: Navigate -> Forward
5. 书签跳转
	1. 添加书签(Toggle Bookmark): `F3/Option+F3`: Navigate -> Bookmarks
	2. 跳转(Go To Bookmark): `Ctrl + 书签值`: `Ctrl + 1`等
	3. 查看书签(Show Bookmarks): `Cmd + F3`, 选择之后跳转
6. 收藏位置和文件
	1. `Option + Shift + F`: Add to Favorites
7. 字符跳转神器(emacsIdea插件)
8. 错误跳转
	1. `F2`: 跳转到下一个错误地, Next Highlighted Error
	2. `Shift + F2`: 跳转到上一个错误地, Previous Highlighted Error
9. 接口/实现跳转
	1. 接口->实现: `Cmd + Option + B`, implementations
	2. 实现->接口: `Cmd + U`, Super method
10. Select in project view, `Ctrl + Option + L`(自己分配快捷键)
11. `Double Shift`, 搜索navigation可以启用/禁用navigation bar, 建议禁用, 使用`Cmd + 上箭头`调出使用
12. 分割屏幕: Split window horizontally/vertically: Window -> Editor Tabs, `Shift + F4`: Open source in new window, 在新窗口打开文件
13. `Cmd + Shift + T`: 测试类跳转(也可以创建测试类)
14. 当前类内方法查找: `Cmd + F12`: Navicate -> File Structure


### 精准搜索
 
1. 类: `Cmd + O`, 根据类名定位类(ec可搜索出EdasConfig等类CamelCase, 也支持通配符*等), Navigate -> Class
2. 文件: `Cmd + Shift + O`, 根据文件名定位文件(/开头可以进入文件夹), Navigate -> File
3. 符号: `Cmd + Option + O`, 根据方法名定位方法(Object.wait可以通过Object.wait指定上下文(namespace)定位), Navigate -> Symbol
4. 以上: 可以加:number跳转到指定行, 比如Object:12; `Shift + Enter`会在另一个窗口打开
4. 字符串: `Cmd + Shift + F`, 根据关键字定位, Edit -> Find -> Find in Path
5. Search Every: `Double Shift`, 可以使用Tab跳转不同的搜索区, #plugins设置插件, #editor设置编辑器...

### 代码助手

1. live template: Live Templates(psvm, getlo, pic, psc等, 可以自定义). `Cmd + J`: 主动获取live templates
2. postfix: Postfix Completion
	1. .if
	2. .var
	3. .sout
	4. .for
	5. .notnull .nn
	6. .field
	7. .return
3. `Option + Enter`: 自动修复, Show intention action
	1. 自动创建函数
	2. list replace: 将`for(int i=0; i < items.size(); i++)`转变成`for(String item : items)`
	3. 字符串format/build: 将`"name: " + name + " age: " + age`转变成`	String.format("name: %s age: %d", name, age)`
	4. 实现接口, 接口上(Option+Enter)自动添加实现类
	5. 单词拼写, typo change to, 将eame转变成ease
	6. 导包, 自动导包
	7. Language Injection: 插入json、正则(还可以简单验证)等
	8. 格式化代码: 选中代码 -> Adjust code style settings
4. `Cmd + Shift + Enter`: Complete Current Statement, 快速补全行末分号
5. `Ctrl + Shift + Space`: Completion SmartType, 智能自动补全(只提示适用于此的补全代码)
6. `Cmd + Shift + up/down`: move statement up/down, `Option + Shift + up/down`: move line up/down
7. `Ctrl + Shift + J`: Join Lines, 合并两行(字符串)
8. `Ctrl + Shift + P`: Expression Type, 显示表达式结果类型, 主要用于分析lambda表达式的类型
9. Locate Duplicates: Analyze -> Locate Duplicates 分析重复代码
10. Analyze -> Analyze Data Flow to/from Here, 定位set或者get该field值的代码
11. Search Structurally: Edit -> Find 根据代码结构定位代码(搜索try catch坏味道代码). 可以添加到Inspections中, Editor -> Inspections开启Structure Search Inspection并添加Search Template或Replace Template(可以`Option Enter`给出intension)
12. Replace Structurally: Edit -> Find 根据代码结构定位代码并替换
13. Run Inspection By Name: 显式调用Inspection定位代码(比如Questionable name)
14. `Cmd + Option + L`: 格式化代码(可以选中某部分代码单独格式化)
15. `Option + /`: Cyclic Expand Word(Hippie Completion)
16. `Cmd + P`: Parameter Info, 提示方法参数(减少进入方法内部看参数的次数, new FileReader(Cmd+p提示参数)). `F1`: Quick Documentation, 打开文档
17. `Cmd + Option + T`: Surround with, 选中代码后用if/try catch/while等surround
18. `Option + 上箭头`: 选择代码

### 重构

1. `Ctrl + T`: Refactor This, 

1. 重构
	1. 重构变量: `Shift + F6`, Refactor -> Rename
	2. 重构方法: `Cmd + F6`, Refactor -> Change Signature
3. 抽取
	1. 抽取变量: Refactor -> Extract -> Variable
	2. 抽取函数: Refactor -> Extract -> Method

### Git集成

1. Annotate: 开启/关闭(右击行数显示/关闭)
2. 撤销: `Cmd + Option + Z`: Revert
3. local history: 
4. **commit之前校验代码**

### 调试Debug

1. 添加断点: `Cmd + F8`, toggle line breakpoint, Run -> Toggle Line Breakpoint
2. 单步运行Step Over: `F8`
3. 跳转到下一个断点Resume: `F9`
4. Step Into: `F7`
5. Smart Step Into: `Shift + F7`, println(getList()), 选择进入哪个方法中
5. Force Step Into: `Option + Shift + F7`
6. Step Out: `Shift + F8`
7. 查看所有断点: `Cmd + Shift + F8`, View breakpoints, 非断点行快捷键生效
8. 禁止所有断点: Mute Breakpoints 
9. 条件断点: `Cmd + Shift + F8`, Edit breakpoint, 在断点行快捷键生效, 再敲快捷键会进入查看所有断点界面
10. 表达式求值: `Option + F8`, Evaluate Expression, 
11. 运行到指定行: `Option + F9`, Run to cursor
12. setValue: `F2`
13. run anywhere if you can: `Ctrl + Shift + D`, Debug context configuration
14. 当前运行列表选择运行: `Ctrl + Option + D`, Run -> Debug
15. 轻量Rest Client: `Cmd + Shift + A` -> 搜索 `Rest Client`
    1. 可以新建Http Request类型文件(.http后缀)保存请求
       
       ```
       GET http://www.taobao.com
		Accept: application/json
		Accept-Charset: utf-8
       ```
16. New Class Level Watch: 添加类级别Watch(getName() + getAge())
17. Java 8 流调试(Trace Current Stream Chain)
	
	```java
	    String msg = "I love idea love Idea ddd";

        String[] words = msg.split(" ");

        String result = Stream.of(words)
            .distinct()
            .map(String::toLowerCase)
            .filter(word -> word.contains("e"))
            .distinct()
            .collect(Collectors.joining(","));
	```
	


### 文件操作
1. `Ctrl + Option + N`: 在当前文件同一级目录下新建一个文件, New...
2. `F5`: 复制当前文件, Refactor -> Copy
3. `F6`: 移动当前文件, Refactor -> Move
4. `Cmd + N`: 创建文件(Project window激活)
5. `Ctrl + Option + N`: Create In This Directory(Project Window或者编辑区激活)
6. Scratch File: 创建临时文件用来临时测试(比如测试某些工具类DateUtil)

### 文本操作
1. `Cmd + c`: 复制文件名; `Cmd + Shift + c`: 复制完整路径
2. 剪贴板: `Cmd + Shift + v`

### 结构图
1. `Cmd + F12`: File Structure, 查看当前Filed、method大纲(method), Navigate -> File Structure
2. `Cmd + Option + Shift + U`: 查看maven依赖、类图, Show Diagram
3. `Ctrl + Option + H`: 查看方法调用层次, Call Hierarchy

### 其他

1. settings -> Editor -> Font -> Font(Fira Code), Enable font ligatures
2. 每一条Inspection检查都是可配置的, Setting -> Editor -> Inspections
3. Enter Presentation Mode(Presentation模式)
4. `Cmd + Shift + Arrow`: Stretch to Left/Right/Up/Down, 调整窗口大小
5. `Shift + Option + Mouse`: Multi cursor


