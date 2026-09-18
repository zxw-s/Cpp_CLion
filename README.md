# 项目名称

> 一句话描述项目，例：C++程序，熟悉CLion

## 📖 项目介绍

基于 CLion 开发的控制台程序。

- 开发IDE：CLion
- 编程语言：C / C++ / C#
- 项目类型：控制台应用

## ⚙️ 环境依赖

### C / C++

1. CMake
2. C/C++编译器：MinGW-w64 / GCC / Clang
3. CLion自动读取`CMakeLists.txt`构建项目

### C#

1. .NET SDK
2. CLion自带.NET插件支持C#项目

## 🚀 编译与运行

### C / C++

1. 将仓库克隆到本地，使用CLion打开项目文件夹
2. CLion自动加载CMake配置，等待项目索引完成
3. 右上角选择构建目标（Debug / Release）
4. 点击运行按钮 ▶️ 或者快捷键 `Shift+F10` 运行程序
5. `Shift+F9` 启动调试

> 手动CMake命令（终端）

```bash
mkdir build && cd build
cmake ..
make
./程序名
```

# 一、新建项目

1. 打开 CLion，欢迎页Welcome → `New Project`
- 左侧选择C语言：`C Executable`；C++：`C++ Executable`
- 设置项目存放路径Location：纯英文路径
- 选择语言标准Language standard：C选C11；C++选C++17/20
- Create 创建项目。

项目自动生成2个核心文件：

- `main.cpp` ：主代码文件

-  `CMakeLists.txt` ：CMake构建配置文件（CLion项目核心）

自动生成的最简代码：

```cpp
#include <iostream>

using namespace std;

int main() {
    cout << "Hello, CLion!" << endl;
    return 0;
}
```

# 二、运行程序

**方式1**：点击编辑区右上角绿色三角 ▶️

**方式2**：快捷键  Shift+F10 

底部控制台会输出运行结果： Hello, CLion!

# 三、基础调试（CLion最强功能）

测试代码 main.cpp

```cpp
#include <iostream>
using namespace std;

int add(int a,int b){
    return a+b;
}

int main()
{
    int x=10,y=20;
    int res = add(x,y);
    cout<<"结果 = "<<res<<endl;
    return 0;
}
```

1. **构建**：锤子图标 `Ctrl+F9`
2. **运行**：绿色三角 ▶ `Shift+F10`
3. **Debug调试（CLion核心）**
- 在代码行号左侧点击打断点（出现红色圆点=断点）
- Debug按钮（虫子图标）开启调试模式。快捷键`Shift+F9`
- `F7` Step Into：单步跳入函数
- `F8` Step Over：单步跳过函数
- `Shift+F8` Step Out：跳出函数
- `F9` Resume：运行到下一个断点
- 下方Debug窗口观察变量值。

底部可查看变量、调用栈，排查bug非常方便

# 四、新手常见报错排查

1. **运行按钮灰色** 工具链Toolchains配置错误；CMakeLists语法错误；点击Reload CMake Project。
2. **报未定义引用 undefined reference** 新增`.c/.cpp`文件没有写到`add_executable()`里面！90%新手踩坑点。
3. **中文控制台乱码** 文件编码UTF‑8，Registry关闭`run.processes.with.pty`。
4. 头文件红色波浪线找不到
   CMake没有reload；或者头文件路径没有配置。
5. cmake‑build‑debug目录异常
   File → Reload CMake Project；也可以删除cmake‑build‑debug文件夹重新构建。

# 五、简单多文件项目目录示例

```plaintext
demo/
├── CMakeLists.txt
├── main.cpp
├── include/
│   └── calc.h
└── src/
    └── calc.cpp
```

CMakeLists增加头文件目录：

```cmake
include_directories(${PROJECT_SOURCE_DIR}/include)
add_executable(demo main.cpp src/calc.cpp)
```

如果你需要，我可以给一份CLion的CMakeLists常用模板，或者一套C语言练习代码用来熟悉CLion调试。

# 六、CLion默认项目目录结构

```plaintext
demo/
├── .idea/                # IDE本地配置，不要提交Git，自动生成
├── cmake‑build‑debug/    # CMake编译输出目录，exe、obj文件，自动生成，可删除
├── CMakeLists.txt        # ✅核心构建脚本，CMake配置文件【必须提交Git】
├── main.c / main.cpp     # 源代码
└── .gitignore
```

> ⚠️新手大坑：新增`.c/.cpp`文件，**必须加到CMakeLists.txt的add_executable里面**，否则编译看不见新文件，报未定义引用错误。

# 七、CMakeLists.txt 基础讲解（新手必懂）

> CLion所有项目都靠这个文件管理，默认生成内容（C++最小模板）：

### CMakeLists.txt最小模板（C++）

项目结构

```plaintext
demo/
├─ main.cpp
└─ CMakeLists.txt
```

`CMakeLists.txt`

```cmake
# 指定cmake最低版本要求，CLion默认版本更高，写3.20够用
cmake_minimum_required(VERSION 3.3) 

# 项目名称，语言 C / CXX(C++)，同时支持C和C++
project(MyFirstProject LANGUAGES C CXX)

# 设置C++标准 C++17，不使用编译器扩展
set(CMAKE_CXX_STANDARD 17) # C++标准
set(CMAKE_CXX_STANDARD_REQUIRED ON) # 强制使用上面的标准，不允许回退旧版本
set(CMAKE_CXX_EXTENSIONS OFF)

# 生成可执行文件：项目名 源码文件，关联源文件，新增源文件写在这里
add_executable(MyFirstProject main.cpp) # 后面跟上所有 .c源文件，空格分隔
```

- 新增cpp文件：只需要在 add_executable 后面追加文件名，例： add_executable(app main.cpp test.cpp) 

- 修改CMakeLists后**必须Reload CMake + Clean再编译**，旧缓存不会自动更新编译参数。

- 修改后右键项目 →  Reload CMake Project  生效

# 八、C++ CLion 综合演示代码

文件名：`main.cpp` 功能覆盖：变量、数组、函数、指针、引用、结构体、类，用来熟悉 CLion：新建项目、编译运行、断点调试。

> CLion 使用 CMake，不需要手动写编译脚本。

```cpp
#include <iostream>
#include <string>

using namespace std;

// 结构体
struct Student
{
    int id;
    string name;
    double score;
};

// 引用参数（C++，等价C指针，更安全）
void setScore(Student &s, double newSc)
{
    s.score = newSc;
}

// 数组求平均值
double getAverage(int arr[], int len)
{
    int sum = 0;
    for (int i = 0; i < len; i++)
    {
        sum += arr[i];
    }
    return static_cast<double>(sum) / len;
}

int main()
{
    int nums[4] = {75, 88, 91, 66};
    Student stu = {1001, "王五", 79.5};

    cout << "==== CLion C++ 测试程序 ====" << endl;

    cout << "数组元素：";
    for (int i = 0; i < 4; i++)
    {
        cout << nums[i] << " ";
    }
    double avg = getAverage(nums, 4);
    cout << "\n数组平均值：" << avg << endl;

    cout << "\n修改前学生：id=" << stu.id << " 姓名=" << stu.name << " 分数=" << stu.score << endl;
    setScore(stu, 93.0);
    cout << "引用修改后分数：" << stu.score << endl;

    int num;
    cout << "\n请输入一个整数：";
    cin >> num;
    cout << "你输入的数字：" << num << endl;

    return 0;
}
```

运行输出：

```plaintext
==== CLion C++ 测试程序 ====
数组元素：75 88 91 66
数组平均值：80

修改前学生：id=1001 姓名=王五 分数=79.5
引用修改后分数：93

请输入一个整数：100
你输入的数字：100
```

## CLion 完整操作步骤

### 1.新建项目

1. New Project → 选择 **C++ Executable**
2. 填写项目名称，C++标准建议选 **C++17**，创建。
3. 把自动生成的 `main.cpp` 全部替换为上面代码。

### 2.编译运行

- `Shift+F10`：运行
- `Ctrl+F9`：编译

### 3.断点调试（CLion 调试非常强大）

1. 代码行号**左侧点击**，出现**红色圆点**就是断点。
   
   > 推荐断点位置：
   > 
   > - `double avg = getAverage(nums,4);`
   > - `setScore(stu,93.0);`

2. `Shift+F9`：启动调试

### CLion 调试快捷键

| 快捷键      | 功能           |
| -------- | ------------ |
| F7       | 单步进入（跳入函数内部） |
| F8       | 单步跳过         |
| Shift+F8 | 跳出函数         |
| F9       | 继续运行到下一个断点   |
| Ctrl+F8  | 切换断点         |

调试窗口：

- **Variables（变量窗口）**：自动查看局部变量，结构体、数组可以展开看成员。
- **Watches（监视）**：手动输入表达式查看：`stu`、`nums[2]`。
- **Frames（调用堆栈）**：看当前函数调用栈。

> 练习：F7 跳进 `setScore`，观察 C++ 引用 `&` 修改 main 里原始结构体。

## C++ 关键概念（和C / C#对比）

1. `&` 引用：C++特性，相当于安全指针，不需要 `*` 解引用；函数传引用可以修改外部变量。

```cpp
void setScore(Student &s, double newSc) // & 引用
```

调用直接传对象：`setScore(stu,93.0);`，不用取地址。

2. 输入输出：
- `cin >> num;` 等价C `scanf`

- `cout << ...` 等价C `printf`
3. `string`：C++字符串类，代替C语言`char[]`。

## CLion常见坑

1. 控制台输入：调试模式输入在CLion下方控制台窗口。
2. 程序闪退：CLion运行默认不会闪退，不需要写 `system("pause")`。
3. C++标准：新建项目建议 C++11 / C++17。
4. 项目由 CMake 管理，不要手动修改编译命令。

## 几款IDE简单对比

| IDE           | 语言       | 调试  | 特点                          |
| ------------- | -------- | --- | --------------------------- |
| Dev‑C++       | C/C++    | 简陋  | 初学，零配置                      |
| VS‑Code       | C/C#/C++ | 强   | 需要配置                        |
| Visual Studio | C/C#/C++ | 最强  | Windows大项目                  |
| CLion         | C/C++    | 优秀  | 跨平台，CMake，Linux/Mac/Windows |

# 九、C++ CLion 核心功能演示代码

文件名：`main.cpp` 涵盖：变量、数组、函数、指针、引用、类；用来练习 CLion：新建项目、编译、运行、断点调试、变量监视、调用栈。

```cpp
#include <iostream>
#include <string>

using namespace std;

// 类
class Person
{
private:
    int id;
    string name;
public:
    void setData(int i, string n)
    {
        id = i;
        name = n;
    }
    int getId() const { return id; }
    string getName() const { return name; }
};

// 引用参数，可以修改外部数据
void changePerson(Person &p)
{
    p.setData(888, "修改后的名字");
}

// 指针版本
void changePersonPtr(Person *p)
{
    p->setData(999, "指针修改");
}

// 数组求平均
double calcAvg(int arr[], int size)
{
    int sum = 0;
    for(int i = 0; i < size; ++i)
    {
        sum += arr[i];
    }
    return static_cast<double>(sum) / size;
}

int main()
{
    int data[4] = {60,70,80,90};
    Person p1;
    p1.setData(1001, "张三");

    cout << "==== CLion C++ 功能测试 ====" << endl;

    cout << "数组：";
    for(int i = 0; i < 4; i++)
    {
        cout << data[i] << " ";
    }
    double avg = calcAvg(data,4);
    cout << "\n平均值：" << avg << endl;

    cout << "\n修改前：id=" << p1.getId() << " name=" << p1.getName() << endl;
    changePerson(p1);
    cout << "引用修改后：id=" << p1.getId() << " name=" << p1.getName() << endl;

    changePersonPtr(&p1);
    cout << "指针修改后：id=" << p1.getId() << " name=" << p1.getName() << endl;

    int num;
    cout << "\n请输入一个整数：";
    cin >> num;
    cout << "你输入：" << num << endl;

    return 0;
}
```

输出：

```plaintext
==== CLion C++ 功能测试 ====
数组：60 70 80 90
平均值：70

修改前：id=1001 name=张三
引用修改后：id=888 name=修改后的名字
指针修改后：id=999 name=指针修改

请输入一个整数：66
你输入：66
```

## CLion 完整操作流程

### 1.新建项目

1. `New Project` → `C++ Executable`
2. C++标准选：**C++17**，创建。
3. 将生成的 `main.cpp` 全部替换为上面代码。

### 2.编译 & 运行

- `Ctrl+F9`：编译
- `Shift+F10`：运行程序

### 3.断点调试（CLion最重要）

1. 在代码行号**左侧灰色条点击**，出现**红色圆点 = 断点**。
   
   > 推荐断点位置：
   > 
   > - `double avg = calcAvg(data,4);`
   > - `changePerson(p1);`
   > - `changePersonPtr(&p1);`

2. `Shift+F9`：启动调试

#### CLion调试快捷键

| 快捷键      | 功能              |
| -------- | --------------- |
| F5       | 启动调试            |
| F7       | 单步进入（跳进函数内部）    |
| F8       | 单步跳过（执行本行，不进函数） |
| Shift+F8 | 跳出当前函数          |
| F9       | 继续运行到下一个断点      |
| Ctrl+F8  | 切换断点开关          |

调试面板：

1. **Variables（变量窗口）**：自动显示局部变量，数组、类对象可以展开查看成员。
2. **Watches（监视）**：手动输入表达式，例如：`p1`、`data[2]`、`&p1`。
3. **Frames（调用堆栈）**：查看函数调用链（main → calcAvg → changePerson）。

> ✅练习：F7 跳进 `changePerson`，观察引用直接操作原始对象；再跳进 `changePersonPtr`，观察指针。

## CLion 常见坑

1. 控制台输入：在下方 **Debug Console** 窗口输入。
2. 不需要写 `system("pause")`，程序结束控制台会保持。
3. CLion 使用 CMake 管理项目，不要手动修改编译参数。
4. 如果断点灰色无效：确认是 Debug 模式，不是 Release。

## 语法对照

| 语法           | 说明                   |
| ------------ | -------------------- |
| `Person &p`  | 引用，原始对象别名，不需要取地址     |
| `Person *p`  | 指针，存储对象地址，使用`->`访问成员 |
| `cin >> num` | 读取输入                 |
| `cout <<`    | 打印输出                 |

下面的CLion入门常用CMake指令清单和模板，方便你后续做多文件项目？

# 十、CLion CMake 常用指令清单和模板（新手版）

> 说明：CLion 的项目完全靠  `CMakeLists.txt` ，修改保存后，右键项目 → **Reload CMake Project** 才会生效。
> 
> 环境：CMake 3.16+，适配 Windows(MSVC) / Linux / macOS，提供**最小可运行、多文件项目、库项目**等模板。
> 
> 编译产物默认输出到  `cmake‑build‑debug`  /  `cmake‑build‑release` 。

## 基础模板（直接复制改名字就能用）

```cmake
cmake_minimum_required(VERSION 3.20) # cmake最低版本，CLion默认版本更高，写3.20够用
project(demo C CXX) # 项目名，同时支持C和C++

set(CMAKE_CXX_STANDARD 17) # C++版本：11/14/17/20
set(CMAKE_CXX_STANDARD_REQUIRED ON) # 强制使用上面的标准，不允许回退旧版本

# 生成可执行程序：项目名 后面跟上所有 .cpp .c源文件
add_executable(demo main.cpp)
```

## 🔧 常用指令分类

1. **版本、项目、语言标准**

```cmake
# 指定cmake最低版本
cmake_minimum_required(VERSION 3.20)

# project(项目名 语言) C CXX代表C、C++
project(myapp C CXX)

# 设置C++标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 设置C语言标准
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

# 开启编译警告，方便找bug（推荐加上）
add_compile_options(-Wall -Wextra)
```

2. **生成程序：可执行文件 / 静态库 / 动态库**

```cmake
# 生成可执行程序（写程序最常用）
add_executable(程序名 源文件1.cpp 源文件2.cpp ...)
add_executable(test main.cpp calc.cpp)

# 生成静态库 .a / .lib
add_library(mylib STATIC calc.cpp util.cpp)

# 生成动态库 .so / .dll
add_library(mylib SHARED calc.cpp util.cpp)
```

3. **头文件目录 include 路径**

当头文件不在当前目录，需要告诉cmake去哪里找  `.h` 

```cmake
# 添加头文件搜索目录
target_include_directories(目标名 PUBLIC ./include)

# 示例：可执行程序test去include文件夹找头文件
target_include_directories(test PUBLIC ./include)
```

4. **链接库（把库绑定到程序）**

```cmake
# target_link_libraries(程序名 库名)
# 把自己写的静态库mylib链接给test程序
target_link_libraries(test mylib)

# 链接系统库，例如linux数学库m
target_link_libraries(test m)
```

5. **源文件放在子文件夹**

项目结构：

```plaintext
project/
├─src/
│ main.cpp
│ util.cpp
├─include/
│ util.h
└─CMakeLists.txt
```

写法：

```cmake
cmake_minimum_required(VERSION 3.20)
project(demo C CXX)
set(CMAKE_CXX_STANDARD 17)

add_executable(demo
    src/main.cpp
    src/util.cpp
)

target_include_directories(demo PUBLIC ./include)
```

6. **递归获取文件夹下全部cpp（不推荐新手滥用）**

新手不建议，新增删除文件不会自动检测，需要手动Reload CMake

```cmake
aux_source_directory(./src SRC_LIST)
add_executable(demo ${SRC_LIST})
```

7. **子项目 add_subdirectory**

当项目分多个模块，每个模块有自己的 CMakeLists.txt 

```cmake
# 进入lib文件夹，读取里面的CMakeLists.txt
add_subdirectory(lib)
```

8. **Debug / Release 模式设置**

CLion右上角可以切换Debug/Release配置

```cmake
# 设置debug模式编译选项
set(CMAKE_CXX_FLAGS_DEBUG "-g -O0")

# release开启优化
set(CMAKE_CXX_FLAGS_RELEASE "-O2")
```

9. **输出路径修改（把exe输出到指定文件夹）**

```cmake
# 可执行文件输出目录
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)

# 库文件输出目录
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib)
```

## 📦 常见项目示例模板

### 模板1：最小单文件可执行程序

项目结构

```plaintext
demo/
├─ main.cpp
└─ CMakeLists.txt
```

`CMakeLists.txt`

```cmake
# 指定cmake最低版本要求
cmake_minimum_required(VERSION 3.16)

# 项目名称，语言 C / CXX(C++)
project(DemoProject LANGUAGES C CXX)

# 设置C++标准 C++17，不使用编译器扩展
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# 生成可执行文件：项目名 源码文件
add_executable(DemoApp main.cpp)
```

构建命令（终端）

```bash
mkdir build
cd build
cmake ..
cmake --build .
```

---

### 模板2：简单多文件项目

目录

```plaintext
./
├─main.cpp
├─foo.cpp
├─foo.h
└─CMakeLists.txt
```

**CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.20)
project(multi_demo C CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_compile_options(-Wall -Wextra)

add_executable(multi_demo
    main.cpp
    foo.cpp
)
```

### 模板3：C++多文件项目（带include头文件目录）

项目结构

```plaintext
demo_c/
├── CMakeLists.txt
├── main.c
├── include
│   └── calc.h
└── src
    └── calc.c
```

`CMakeLists.txt`

```cmake
cmake_minimum_required(VERSION 3.20)
project(demo_cpp LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_compile_options(-Wall -Wextra)

# 头文件搜索路径
include_directories(${PROJECT_SOURCE_DIR}/include)

# 收集所有cpp源码
file(GLOB SRC_FILES 
    ${PROJECT_SOURCE_DIR}/src/*.cpp
)

add_executable(multi_demo
        main.cpp
        ${SRC_FILES}
)
```

> 注意：`file(GLOB)` 适合小项目；大型项目建议手动列出源文件，避免新增文件不触发重编译。

---

### 模板4：使用静态库 + 可执行程序（最常用工程模板）

目录结构

```plaintext
lib_demo/
├─main.cpp
├─CMakeLists.txt
├─ src/
│  └─ libcalc_lib.cpp
└─ include/
   └─ libcalc.h
```

`CMakeLists.txt`

```cmake
cmake_minimum_required(VERSION 3.20)
project(lib_demo C CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_compile_options(-Wall -Wextra)

include_directories(${PROJECT_SOURCE_DIR}/include)

# 1.编译静态库
add_library(calc STATIC src/libcalc.cpp)
target_include_directories(calc PUBLIC ./)

# 2.编译主程序
add_executable(app main.cpp)

# 3.主程序链接静态库
target_link_libraries(app PRIVATE calc) #链接静态库
```

- `STATIC`：静态库 `.lib`(win) / `.a`(linux)
- `SHARED`：动态库 `.dll`(win) / `.so`(linux) / `.dylib`(mac)
- `PRIVATE / PUBLIC / INTERFACE` 链接权限：
  - `PRIVATE`：仅当前目标使用
  - `PUBLIC`：当前目标 + 依赖它的目标都生效

### 模板5：Windows MSVC 额外配置（中文乱码、输出目录）

```cmake
cmake_minimum_required(VERSION 3.16)
project(MsvcDemo LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# MSVC编译器配置
if(MSVC)
    add_compile_options(/utf‑8)       # 源码和执行文件UTF‑8，解决中文乱码
    add_compile_options(/W4)          # 开启警告等级4
    add_compile_options(/WX‑)         # 警告不视为错误
endif()

# 设置输出目录：所有exe、dll输出到bin
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

add_executable(App main.cpp)
```

---

### 模板6：子目录多模块项目（add_subdirectory）

目录

```plaintext
bigproj/
├─ CMakeLists.txt
├─ main.cpp
└─ module_a/
   ├─ CMakeLists.txt
   ├─ a.cpp
   └─ a.h
```

根目录 `CMakeLists.txt`

```cmake
cmake_minimum_required(VERSION 3.16)
project(BigProj LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 17)

# 引入子模块
add_subdirectory(module_a)

add_executable(BigApp main.cpp)
target_link_libraries(BigApp PRIVATE module_a_lib)
```

`module_a/CMakeLists.txt`

```cmake
add_library(module_a_lib STATIC a.cpp)
target_include_directories(module_a_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
```

---

### 模板7：简单Git忽略文件 .gitignore

```gitignore
# CLion IDE配置
.idea/

# CMake编译产物
cmake-build-*/
CMakeCache.txt
CMakeFiles/
*.cmake
install_manifest.txt

# 操作系统
.DS_Store
Thumbs.db
```

---

## ⚠️ 新手高频踩坑

**1. 新增cpp文件一定要写到add_executable的文件列表中！**

不写进去CLion识别不到，编译直接报未定义引用。

2. 修改完CMakeLists.txt，**必须Reload CMake Project，否则不生效**。

3. 头文件  `.h`  不需要写进  `add_executable` ，只需要 `target_include_directories` 指定目录即可。

4. 报错乱码：`CLion设置 → Editor → File Encodings`，全部设为UTF‑8。

5. CMake报错时，可以删除  `cmake‑build‑debug`  整个文件夹，重新加载。

## 📝 常用变量速查表

| 变量                    | 含义                        |
| --------------------- | ------------------------- |
| ${CMAKE_SOURCE_DIR}   | 项目根目录                     |
| ${CMAKE_BINARY_DIR}   | 编译输出目录（cmake‑build‑debug） |
| PROJECT_NAME  project | 设置的项目名                    |

## 关键常用指令速查表

| 命令                             | 作用                                   |
| ------------------------------ | ------------------------------------ |
| `cmake_minimum_required()`     | 最低CMake版本                            |
| `project()`                    | 定义项目名、语言                             |
| `set()`                        | 设置变量                                 |
| `add_executable()`             | 生成可执行文件                              |
| `add_library()`                | 生成静态/动态库                             |
| `target_include_directories()` | 给目标设置头文件路径（推荐，替代include_directories） |
| `target_link_libraries()`      | 目标链接库                                |
| `add_subdirectory()`           | 引入子文件夹CMake项目                        |
| `if(MSVC)`                     | Windows MSVC条件判断                     |
| `if(UNIX)`                     | Linux/macOS条件判断                      |

> 💡最佳实践：优先使用 `target_xxx` 系列命令（target_include_directories），不要全局 `include_directories`，作用域清晰，适合大型项目。

## 配套习题

1. 使用模板1新建项目，编译输出 `cout<<"Hello CMake"`。
2. 修改模板3，把静态库改为**动态库(SHARED)**，编译运行观察dll文件生成。
3. 使用模板4，验证MSVC下中文输出不乱码。
4. 搭建子目录项目结构，使用`add_subdirectory`拆分模块。

如果你需要，我可以给一份**可直接复制的CMake完整工程目录模板**，或者 CMakeLists.txt 调试技巧。

下面的CMakeLists.txt模板文件，你直接复制粘贴，改改文件名就能写大一点的C/C++项目。

# 十一、CMakeLists.txt 通用模板

> 适用：CLion C/C++ 项目，普通可执行程序、带include/src目录，支持C++17，开启警告，可直接复制使用。
> 
> 修改后：右键项目 →  `Reload CMake Project` 

## 模板A｜小型项目（所有源文件放根目录）

文件名： CMakeLists.txt 

```cmake
# CMake最低版本要求
cmake_minimum_required(VERSION 3.20)

# 项目名称，支持C和C++
project(my_project C CXX)

# C++标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# C标准（不需要C可以注释掉）
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

# 开启编译警告，帮你发现代码问题
add_compile_options(-Wall -Wextra)

# 生成可执行文件，把你的全部 .cpp .c 写在这里
add_executable(${PROJECT_NAME}
    main.cpp
    # other.cpp # 新增源文件在这里添加
)
```

## 模板B｜规范目录结构（推荐，工程化）

**目录结构：**

```plaintext
my_project/
├─ CMakeLists.txt
├─ src/ # 放 .cpp .c 源文件
│ └─ main.cpp
└─ include/ # 放 .h .hpp 头文件
    └─ util.h
```

 CMakeLists.txt 

```cmake
cmake_minimum_required(VERSION 3.20)

project(my_project C CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

add_compile_options(-Wall -Wextra)

# 可执行程序，src下所有cpp写这里
add_executable(${PROJECT_NAME}
    src/main.cpp
# src/xxx.cpp # 新增源文件在此追加
)

# 指定头文件搜索路径
target_include_directories(${PROJECT_NAME}
    PUBLIC
        ${CMAKE_SOURCE_DIR}/include
)

# 【可选】把生成的exe输出到项目下bin文件夹
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)
```

## 模板C｜带静态库的项目模板

**目录：**

```plaintext
my_project/
├─CMakeLists.txt
├─src
│ main.cpp
├─include
│ mathlib.h
└─lib_src
 mathlib.cpp
```

 CMakeLists.txt 

```cmake
cmake_minimum_required(VERSION 3.20)
project(my_project C CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_compile_options(-Wall -Wextra)

# 编译静态库
add_library(math_lib STATIC
    lib_src/mathlib.cpp
)
target_include_directories(math_lib PUBLIC ${CMAKE_SOURCE_DIR}/include)

# 主程序
add_executable(${PROJECT_NAME}
    src/main.cpp
)

# 链接静态库
target_link_libraries(${PROJECT_NAME} math_lib)

# 输出路径
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)
```

## ✂️ 使用要点

1. `${PROJECT_NAME}`  会自动读取 `project(xxx)`  里的项目名，不用到处改名字。

2. **头文件.h/.hpp不要写进add_executable**，只需要 `target_include_directories` 指定include目录。

3. 新增 `.cpp` 文件，务必加到 `add_executable` 的文件列表。

4. 报错时：删除 `cmake‑build‑debug` 文件夹，`Reload CMake`。

> 小提示：直接复制模板保存为  `CMakeLists.txt` ，放到项目根目录，CLion就可以识别。

如果你需要，我可以再给一份模板D，支持Debug/Release分开配置、条件编译。

## 模板D：Debug / Release分离 + 条件编译 + 可选库开关

适合中等规模C/C++项目，CLion直接使用。

特性：

- Debug：开启调试符号、关闭优化、全警告

- Release：O2优化、去除调试信息

- 条件编译宏开关，可选择性开启功能

- 规范 src / include 目录

- 可选择是否编译内部静态库

- 可执行文件输出到项目  bin/ ，库输出到  lib/ 

目录结构

```plaintext
my_project/
├─ CMakeLists.txt
├─ src/
│ └─ main.cpp
├─ include/
│ └─ util.h
└─ lib_src/ # 内部库源码（可选）
    └─ mylib.cpp
```

 CMakeLists.txt 

```cmake
cmake_minimum_required(VERSION 3.20)

# ========== 项目基础配置 ==========
project(my_app C CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

# ========== 编译选项：Debug / Release 差异化 ==========
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    message(STATUS "==== Build Mode: DEBUG ====")
    add_compile_options(-Wall -Wextra -g -O0)
    add_definitions(-DDEBUG_MODE) # 代码中可以 #ifdef DEBUG_MODE
else()
    message(STATUS "==== Build Mode: RELEASE ====")
    add_compile_options(-Wall -Wextra -O2)
    add_definitions(-DRELEASE_MODE)
endif()

# ========== 功能开关（条件编译，ON/OFF） ==========
option(BUILD_MY_LIB "build internal static library" ON)
option(ENABLE_LOG "enable log print" ON)
if(ENABLE_LOG)
    add_definitions(-DENABLE_LOG)
endif()

# ========== 输出路径设置 ==========
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib)

# ========== 内部静态库（受BUILD_MY_LIB开关控制） ==========
if(BUILD_MY_LIB)
    add_library(inner_lib STATIC
        lib_src/mylib.cpp
    )

    target_include_directories(inner_lib
        PUBLIC
            ${CMAKE_SOURCE_DIR}/include
    )
endif()

# ========== 主可执行程序 ==========
add_executable(${PROJECT_NAME}
    src/main.cpp

# src/other.cpp #在这里追加你的源文件
)

target_include_directories(${PROJECT_NAME}
    PUBLIC
        ${CMAKE_SOURCE_DIR}/include
)

# 条件链接库
if(BUILD_MY_LIB)
    target_link_libraries(${PROJECT_NAME} inner_lib)
endif()

# 如果需要链接系统库，在这里添加
# target_link_libraries(${PROJECT_NAME} m pthread)
```

## 📌 在C++代码里怎么用条件宏示例 main.cpp

```cpp
#include <iostream>

int main() {
#ifdef DEBUG_MODE
    std::cout << "Debug版本运行 ";
#endif

#ifdef RELEASE_MODE
    std::cout << "Release版本运行 ";
#endif

#ifdef ENABLE_LOG
    std::cout << "日志功能已开启 ";
#endif

    return 0;
}
```

# 十二、✨ CLion中修改选项两种方式

### 方式1：直接改CMakeLists.txt

```cmake
option(BUILD_MY_LIB "build internal static library" OFF)
option(ENABLE_LOG "enable log print" OFF)
```

改完后右键项目 →  `Reload CMake Project` 

### 方式2：CLion图形界面修改

 `File → Settings → Build,Execution,Deployment → CMake` 

找到  `CMake options` ，填入：

```plaintext
-DBUILD_MY_LIB=ON -DENABLE_LOG=OFF
```

### ⚠️ 使用注意

1. CLion右上角切换  `Debug / Release`  配置，切换后需要`Reload CMake`。

2. 宏  `add_definitions(-DXXX)`  等价于代码里 `#define XXX` 。

3. 如果不需要内部库，直接把 `BUILD_MY_LIB` 设为 OFF ，库就不会编译、也不会链接。

4. 清理构建：删除 `cmake‑build‑debug` 、 `cmake‑build‑release` 文件夹，重新加载。

### 常用扩展小提示

- 链接线程库： `target_link_libraries(${PROJECT_NAME} pthread)` （Linux）

- 如果你不需要C语言，把 `project(my_app C CXX) 改为 project(my_app CXX)` 。

需要的话，我可以给你一份简单的CMake排错清单，遇到报错快速定位。

# 十三、CMake 简单排错清单（CLion场景）

> 出现CMake报错，优先顺序：看CLion底部CMake输出日志 → 按下面逐条排查 → 不行就删除构建目录重新Reload。

## 🔴 第一类：CMake解析报错（CMakeLists.txt语法错误）

现象：CLion提示  CMake Error at CMakeLists.txt:xx ，指向某一行。

1. 括号、引号必须成对， add_executable(  少右括号、逗号多写漏写都会报错。

2. 指令大小写不敏感，但变量名区分大小写。

3. 换行：源文件列表一行一个，不要漏换行； # 是注释，后面内容全部忽略。

4. 字符串路径带空格，一定要用双引号包裹  "${CMAKE_SOURCE_DIR}/my project" 。

✅修复：定位报错行，检查语法；修改后 Reload CMake Project。

## 🟠 第二类：工具链/编译器问题

现象：报错  no CMAKE_CXX_COMPILER could be found 、找不到gcc/g++。

1. Windows：检查 Toolchains 是否选中MinGW，gcc/g++是否真实存在。

2. macOS：确认执行过  xcode-select --install 。

3. Linux：确认安装  gcc g++ cmake gdb 。

4. CLion： File→Settings→Build,Execution,Deployment→Toolchains ，看是否有红色警告。

✅修复：配置好编译器；如果工具链显示红色，重新选择gcc路径。

## 🟡 第三类：找不到头文件  fatal error: xxx.h: No such file or directory

编译报错找不到 .h 头文件。

1. 是否写了  target_include_directories(目标 PUBLIC 头文件目录) 。

2. 路径写错：区分相对路径，多用  ${CMAKE_SOURCE_DIR}  写绝对项目根路径。

3. ⚠️不要用  include_directories()  全局乱加，优先用target_include_directories。

4. 修改CMakeLists之后必须Reload CMake。

✅修复：核对include目录路径；Reload。

## 🟢 第四类：未定义引用 undefined reference（高频大坑）

编译能过，链接阶段报错，函数声明找到了，但找不到函数实现。

原因：

1.  .cpp/.c 源文件没有加到add_executable / add_library源文件列表。（最常见！头文件h不需要写，cpp一定要写）

2. 库没有链接，忘记写 target_link_libraries() 。

3. C和C++混编extern"C"问题。

4. 库顺序错误：被依赖的库要写在后面。

✅修复：

- 把所有实现文件加入add_executable；

- 确认链接库指令；

- 保存后Reload CMake。

## 🔵 第五类：新增文件后CLion识别不到

现象：新建cpp文件，CLion看不见，编译报函数找不到。

1. 新 .cpp 必须手动加到 add_executable 的文件列表。

2. 不要过度依赖 aux_source_directory ，新增文件不会自动更新。

3. 修改CMakeLists之后右键项目 Reload CMake Project。

## 🟣 第六类：Debug/Release 行为异常，改配置不生效

1. CLion右上角切换Debug/Release，切换之后建议Reload CMake。

2. option选项修改后，必须Reload。

3. 条件编译宏不生效：确认 add_definitions(-DXXX) 写对，区分大小写。

## ⚫ 万能急救操作（80%诡异问题可以解决）

1. 关闭CLion。

2. 删除项目目录下： cmake‑build‑debug 、 cmake‑build‑release  两个文件夹。

3. 重新打开CLion，等待CMake自动重新加载。

## 🟤 第七类：中文乱码

Windows平台控制台输出中文乱码：

1. Settings → Editor → File Encodings，全部设置为UTF‑8。

2. 如果是MinGW，可添加编译选项： add_compile_options(-fexec-charset=UTF-8) 

## 📋快速自查小表，出问题照着勾一遍

CMakeLists.txt语法无括号缺失

Toolchain编译器无红色报错

全部 .cpp 实现文件加入add_executable

头文件目录通过target_include_directories配置

需要链接的库写target_link_libraries

修改CMakeLists后执行Reload CMake Project

异常时删除cmake‑build‑*构建目录重新生成

## 💡排错小技巧

1. 使用 `message(STATUS "打印信息: ${变量名}")` 打印变量，查看路径是否符合预期。

```cmake
message(STATUS "项目根目录: ${CMAKE_SOURCE_DIR}")
```

2. 仔细阅读CMake输出窗口的完整报错，**第一行报错才是真正原因**，后面都是连锁报错。

---

# 十四、CLion新手自检小清单

- 项目路径**无中文、无空格**
- Toolchains工具链MinGW‑w64配置正常，gcc/g++/gdb全部识别
- 新增`.c/.cpp`必须写入`add_executable()`
- 头文件目录使用 `include_directories()`
- 编码全部设置为UTF‑8，Windows控制台乱码关闭 `run.processes.with.pty`
- cmake‑build‑debug 编译产物不要提交Git

接下来可以：

1. 练习多文件头文件拆分代码
2. 或者我给你一份CLion优化设置清单，你想要哪个？

下面是基于 `std::vector` 的版本，体验C++ STL容器。

# 十五、C++ std::vector 演示代码（CLion）

`std::vector`：C++ STL 动态数组，不需要手动设置最大长度，自动扩容，代替传统固定大小数组。
可以用来练习 CLion 调试：观察 vector 内部元素变化。

```cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;

class Student
{
private:
    int id;
    string name;
    double score;
public:
    void setInfo(int id_, string name_, double score_)
    {
        id = id_;
        name = name_;
        score = score_;
    }
    int getId() const { return id; }
    string getName() const { return name; }
    double getScore() const { return score; }
};

// 添加学生，vector 引用传入，修改外部容器
void addStudent(vector<Student> &stuVec)
{
    Student s;
    int id;
    string name;
    double score;
    cout << "请输入 id 姓名 分数：";
    cin >> id >> name >> score;
    s.setInfo(id, name, score);
    stuVec.push_back(s); // 尾部添加元素，自动扩容
}

void showAll(const vector<Student> &stuVec)
{
    if (stuVec.empty())
    {
        cout << "暂无学生数据" << endl;
        return;
    }
    cout << "\n====学生列表====" << endl;
    for (size_t i = 0; i < stuVec.size(); i++)
    {
        cout << "id:" << stuVec[i].getId()
             << " 姓名:" << stuVec[i].getName()
             << " 分数:" << stuVec[i].getScore() << endl;
    }
}

// 修改分数
void modifyScore(vector<Student> &stuVec, int targetId, double newScore)
{
    for (size_t i = 0; i < stuVec.size(); i++)
    {
        if (stuVec[i].getId() == targetId)
        {
            stuVec[i].setInfo(stuVec[i].getId(), stuVec[i].getName(), newScore);
            cout << "修改成功" << endl;
            return;
        }
    }
    cout << "未找到该学生" << endl;
}

// 删除学生
void deleteById(vector<Student> &stuVec, int delId)
{
    for (size_t i = 0; i < stuVec.size(); i++)
    {
        if (stuVec[i].getId() == delId)
        {
            stuVec.erase(stuVec.begin() + i); // 删除指定位置元素
            cout << "删除成功" << endl;
            return;
        }
    }
    cout << "未找到该学生" << endl;
}

double getAvg(const vector<Student> &stuVec)
{
    if (stuVec.empty()) return 0;
    double sum = 0;
    for (const auto &s : stuVec)
    {
        sum += s.getScore();
    }
    return sum / stuVec.size();
}

int main()
{
    vector<Student> stuVector; // 动态数组，无固定上限
    int op;

    while (true)
    {
        cout << "\n====菜单====" << endl;
        cout << "1.添加学生   2.显示全部   3.修改分数   4.删除学生   5.查看平均分   0.退出" << endl;
        cout << "请选择操作：";
        cin >> op;

        if (op == 0) break;
        else if (op == 1)
        {
            addStudent(stuVector);
        }
        else if (op == 2)
        {
            showAll(stuVector);
        }
        else if (op == 3)
        {
            int targetId;
            double newSc;
            cout << "输入id 和新分数：";
            cin >> targetId >> newSc;
            modifyScore(stuVector, targetId, newSc);
        }
        else if (op == 4)
        {
            int delId;
            cout << "输入要删除学生id：";
            cin >> delId;
            deleteById(stuVector, delId);
        }
        else if (op == 5)
        {
            double avg = getAvg(stuVector);
            cout << "全体平均分：" << avg << endl;
        }
        else
        {
            cout << "无效选项" << endl;
        }
    }
    cout << "程序结束" << endl;
    return 0;
}
```

## vector 核心API

| 函数                        | 作用            |
| ------------------------- | ------------- |
| `push_back(x)`            | 尾部添加元素，自动扩容   |
| `size()`                  | 获取当前元素个数      |
| `empty()`                 | 判断是否为空        |
| `erase(pos)`              | 删除指定位置元素      |
| `stuVec[i]`               | 下标访问，和普通数组一样  |
| `vector<Student> &stuVec` | 引用传参，避免拷贝整个容器 |

> 范围for循环：

```cpp
for(const auto &s : stuVec)
```

`auto`自动推导类型，`const &`避免拷贝，只读遍历。

## CLion调试练习

1. 断点设置：`addStudent(stuVector);`
2. **F5** 启动调试，**F7** 跳进函数
3. Variables窗口：可以直接看到 `stuVector`，展开查看内部所有元素、`size`。
4. Watches监视：输入 `stuVector.size()`，观察元素数量变化。

### vector vs 普通数组

- 普通数组：`Student arr[6];`，固定大小，容易越界，需要手动维护count。
- `vector<Student>`：动态大小，自动管理内存，`.size()`获取有效数量。

## 关键注意点

1. 传递vector给函数，尽量传**引用 `vector<Student> &`**，否则会拷贝整个容器，性能差。
2. 只读不修改：用 `const vector<Student> &`。
3. `erase` 删除元素后，后面元素会自动前移，不需要手动循环移位（对比之前固定数组版本）。

---

### 回顾整套学习链路

1. C：Dev‑C++，指针 + 固定数组
2. C#：VS，struct 值类型 / class 引用类型
3. C++：CLion，class封装 + 引用，再到 STL vector 动态容器

如果你需要，我可以简单对比：C++ vector / C# List，帮你打通两边集合容器的对应关系。

# 十六、C++ vector 🆚 C# List 对比

两者都是**动态可变长度容器**，代替固定数组，自动扩容，不需要手动维护有效元素数量。
业务逻辑完全等价：存储学生对象，增删改查。

## API 对照表

| 功能        | C++ `std::vector<T>`                          | C# `List<T>`                                  |
| --------- | --------------------------------------------- | --------------------------------------------- |
| 头文件/命名空间  | `#include <vector>`<br>`using namespace std;` | `using System.Collections.Generic;`           |
| 创建容器      | `vector<Student> vec;`                        | `List<Student> list = new List<Student>();`   |
| 尾部添加元素    | `vec.push_back(obj);`                         | `list.Add(obj);`                              |
| 元素个数      | `vec.size()`                                  | `list.Count`                                  |
| 是否为空      | `vec.empty()`                                 | `list.Count == 0`                             |
| 下标访问      | `vec[i]`                                      | `list[i]`                                     |
| 删除指定下标    | `vec.erase(vec.begin()+i);`                   | `list.RemoveAt(i);`                           |
| 清空全部      | `vec.clear();`                                | `list.Clear();`                               |
| 遍历(范围for) | `for(const auto& s : vec){}`                  | `foreach(var s in list){}`                    |
| 传参避免拷贝    | `func(vector<Student> &vec)` 引用               | `func(List<Student> list)`（引用类型，天然传引用，不需要ref） |

> ⚠️重要差异：

1. **C++ vector：值语义** vector 存的是完整对象；传参不传 `&`，会**拷贝整个容器**，开销很大，所以一般用引用 `&`。
2. **C# List：引用类型** List 本身是引用对象，内部存对象引用；方法传参默认传递引用，**不会拷贝整个集合，不需要 ref**。

---

## 极简对照示例

### C++ vector

```cpp
vector<Student> vec;
vec.push_back(s);
cout << vec.size();
vec.erase(vec.begin()+0);
```

### C# List

```csharp
List<Student> list = new List<Student>();
list.Add(s);
Console.WriteLine(list.Count);
list.RemoveAt(0);
```

---

## 完整逻辑横向对比（学生管理核心片段）

### C++ vector（CLion）

```cpp
// 传入引用，避免拷贝
void addStudent(vector<Student> &vec)
{
    Student s;
    cin >> s.getId() ...;
    vec.push_back(s);
}
```

### C# List（Visual Studio）

```csharp
// List 是引用类型，直接传，不需要 ref
static void AddStudent(List<Student> list)
{
    Student s = new Student();
    // ...赋值
    list.Add(s);
}
```

---

## 调试上的区别

1. **CLion(C++)** Variables 窗口展开 `vector`，可以看到内部数组、`size`、`capacity`(容量)。
   
   > size：实际元素数量；capacity：已经分配的内存容量（大于等于size）。

2. **VS(C#)** 局部变量窗口展开 `List<T>`，直接看到所有元素，`Count` 属性。

> capacity：预分配的空间，vector / List 都会自动扩容，不需要你手动管理。

---

## 全套技术栈回顾总结

| 语言  | IDE           | 容器            | 修改外部数据方式                |
| --- | ------------- | ------------- | ----------------------- |
| C   | Dev‑C++       | 固定数组          | 指针 `*`、`&`              |
| C++ | CLion         | `std::vector` | 引用 `&` / 指针             |
| C#  | Visual Studio | `List<T>`     | class：天然引用；struct：`ref` |

### 核心记忆点

- C：一切靠指针，手动管理内存。
- C++：引用 `&` 安全替代指针；vector 值容器，传参记得加 `&`。
- C#：class / List 都是引用类型，默认传引用；只有 struct 值类型才需要 `ref`。

---

如果你想，我可以下一步给你做一个小题目：

> 实现一个简单的学生管理，分别用：C++ vector、C# List，自己默写增删改查，巩固这套知识。
> 或者，我可以简单讲讲 C++ 指针、引用、拷贝构造函数的坑。

如果你需要，我可以写一个 C++ 版学生管理系统（使用引用、struct / class）。

# 十七、C++ 学生管理控制台程序（CLion）

功能：菜单交互，增、删、改、查、平均分；使用 struct、C++引用 `&`，数组存储，适合 CLion 断点调试。

> CLion 不需要 `system("pause")`，控制台不会闪退。

```csharp
#include <iostream>
#include <string>

using namespace std;

const int MAX_STUDENT = 6;

// 学生结构体
struct Student
{
    int id;
    string name;
    double score;
};

// 添加学生，count 使用引用，修改外部有效数量
void addStudent(Student stuArr[], int &count)
{
    if (count >= MAX_STUDENT)
    {
        cout << "学生已满，无法添加！" << endl;
        return;
    }
    Student s;
    cout << "请输入 id 姓名 分数：";
    cin >> s.id >> s.name >> s.score;
    stuArr[count] = s;
    count++;
}

// 显示全部学生
void showAll(Student stuArr[], int count)
{
    if (count <= 0)
    {
        cout << "暂无学生数据" << endl;
        return;
    }
    cout << "\n====学生列表====" << endl;
    for (int i = 0; i < count; i++)
    {
        cout << "id:" << stuArr[i].id
             << " 姓名:" << stuArr[i].name
             << " 分数:" << stuArr[i].score << endl;
    }
}

// C++引用 &，直接修改原始结构体
void modifyScore(Student &s, double newScore)
{
    s.score = newScore;
}

// 根据id删除学生
bool deleteById(Student stuArr[], int &count, int delId)
{
    int pos = -1;
    for (int i = 0; i < count; i++)
    {
        if (stuArr[i].id == delId)
        {
            pos = i;
            break;
        }
    }
    if (pos == -1)
        return false;

    // 数组元素向前覆盖
    for (int i = pos; i < count - 1; i++)
    {
        stuArr[i] = stuArr[i + 1];
    }
    count--;
    return true;
}

// 计算平均分
double getAvg(Student stuArr[], int count)
{
    if (count <= 0)
        return 0;
    double sum = 0;
    for (int i = 0; i < count; i++)
    {
        sum += stuArr[i].score;
    }
    return sum / count;
}

int main()
{
    Student stuArray[MAX_STUDENT];
    int stuCount = 0;
    int op;

    while (true)
    {
        cout << "\n====菜单====" << endl;
        cout << "1.添加学生   2.显示全部   3.修改分数   4.删除学生   5.查看平均分   0.退出" << endl;
        cout << "请选择操作：";
        cin >> op;

        if (op == 0)
        {
            break;
        }
        else if (op == 1)
        {
            addStudent(stuArray, stuCount);
        }
        else if (op == 2)
        {
            showAll(stuArray, stuCount);
        }
        else if (op == 3)
        {
            int targetId;
            double newSc;
            cout << "输入要修改的id 和新分数：";
            cin >> targetId >> newSc;

            int findIndex = -1;
            for (int i = 0; i < stuCount; i++)
            {
                if (stuArray[i].id == targetId)
                {
                    findIndex = i;
                    break;
                }
            }
            if (findIndex != -1)
            {
                modifyScore(stuArray[findIndex], newSc);
                cout << "修改成功" << endl;
            }
            else
            {
                cout << "未找到该学生" << endl;
            }
        }
        else if (op == 4)
        {
            int delId;
            cout << "输入要删除学生id：";
            cin >> delId;
            bool ok = deleteById(stuArray, stuCount, delId);
            if (ok)
                cout << "删除成功" << endl;
            else
                cout << "未找到学生" << endl;
        }
        else if (op == 5)
        {
            double avg = getAvg(stuArray, stuCount);
            cout << "全体平均分：" << avg << endl;
        }
        else
        {
            cout << "无效选项" << endl;
        }
    }

    cout << "程序结束" << endl;
    return 0;
}
```

## CLion 调试实操

1. 新建 C++ Executable 项目，替换 `main.cpp`。
2. 行号左侧点击设置**红色断点**。
   推荐断点：
- `addStudent(stuArray, stuCount);`
- `modifyScore(stuArray[findIndex], newSc);`

### CLion 调试快捷键

| 快捷键      | 功能                  |
| -------- | ------------------- |
| F5       | 启动调试                |
| F7       | 单步进入（跳进函数内部，观察引用修改） |
| F8       | 单步跳过                |
| Shift+F8 | 跳出函数                |
| F9       | 运行到下一个断点            |

调试面板：

- **Variables**：查看局部变量，结构体、数组可以展开。
- **Watches**：监视表达式，例如：`stuCount`、`stuArray[0]`
- **Frames**：调用堆栈，查看函数调用关系。

> 练习：F7 跳进 `modifyScore`，观察 C++ 引用 `&` 直接修改 main 中数组里的结构体，不需要取地址符号。

## C++ 引用 & 重点

```csharp
void modifyScore(Student &s, double newScore)
{
    s.score = newScore;
}
```

- `&`：引用，别名，是原始变量的另一个名字，**不是副本**。
- 调用直接传对象：`modifyScore(stuArray[findIndex], newSc);`，不用 `&`。
- 和C对比：等价C语言指针，但语法更干净。
- 和C#对比：C++引用 ≈ C# `ref`。

## C / C# / C++ 对照

| C             | C++            | C#                         |
| ------------- | -------------- | -------------------------- |
| `printf`      | `cout`         | `Console.WriteLine`        |
| `scanf`       | `cin`          | `Console.ReadLine()+Parse` |
| 指针 `Student*` | 引用 `Student&`  | `ref Student`              |
| struct结构体     | struct / class | struct / class             |

> 拓展思考：把 `struct Student` 改成 `class Student`，学习访问权限 `private/public`。

需要我给你改成 class 版本（带访问权限）的 C++ 学生管理吗？

# 十八、C++ class版本学生管理系统（CLion）

把 Student 从 `struct` 改为 `class`，使用**封装思想**：成员变量设置为 `private`，通过 public 的成员函数来读写数据，这是C++面向对象最核心的写法。

```csharp
#include <iostream>
#include <string>

using namespace std;

const int MAX_STUDENT = 6;

// class：默认成员 private
class Student
{
private:
    // 私有成员：外部不能直接访问
    int id;
    string name;
    double score;

public:
    // 设置数据
    void setInfo(int id_, string name_, double score_)
    {
        id = id_;
        name = name_;
        score = score_;
    }

    void setScore(double s)
    {
        score = s;
    }

    // 获取数据
    int getId() const
    {
        return id;
    }

    string getName() const
    {
        return name;
    }

    double getScore() const
    {
        return score;
    }
};

// 添加学生，count引用修改外部数量
void addStudent(Student stuArr[], int &count)
{
    if (count >= MAX_STUDENT)
    {
        cout << "学生已满，无法添加！" << endl;
        return;
    }
    int id;
    string name;
    double score;
    cout << "请输入 id 姓名 分数：";
    cin >> id >> name >> score;

    stuArr[count].setInfo(id, name, score);
    count++;
}

// 显示全部学生
void showAll(Student stuArr[], int count)
{
    if (count <= 0)
    {
        cout << "暂无学生数据" << endl;
        return;
    }
    cout << "\n====学生列表====" << endl;
    for (int i = 0; i < count; i++)
    {
        cout << "id:" << stuArr[i].getId()
             << " 姓名:" << stuArr[i].getName()
             << " 分数:" << stuArr[i].getScore() << endl;
    }
}

// 修改分数：引用传入对象
void modifyScore(Student &s, double newScore)
{
    s.setScore(newScore);
}

// 根据id删除学生
bool deleteById(Student stuArr[], int &count, int delId)
{
    int pos = -1;
    for (int i = 0; i < count; i++)
    {
        if (stuArr[i].getId() == delId)
        {
            pos = i;
            break;
        }
    }
    if (pos == -1)
        return false;

    // 数组元素向前覆盖
    for (int i = pos; i < count - 1; i++)
    {
        stuArr[i] = stuArr[i + 1];
    }
    count--;
    return true;
}

// 计算平均分
double getAvg(Student stuArr[], int count)
{
    if (count <= 0)
        return 0;
    double sum = 0;
    for (int i = 0; i < count; i++)
    {
        sum += stuArr[i].getScore();
    }
    return sum / count;
}

int main()
{
    Student stuArray[MAX_STUDENT];
    int stuCount = 0;
    int op;

    while (true)
    {
        cout << "\n====菜单====" << endl;
        cout << "1.添加学生   2.显示全部   3.修改分数   4.删除学生   5.查看平均分   0.退出" << endl;
        cout << "请选择操作：";
        cin >> op;

        if (op == 0)
        {
            break;
        }
        else if (op == 1)
        {
            addStudent(stuArray, stuCount);
        }
        else if (op == 2)
        {
            showAll(stuArray, stuCount);
        }
        else if (op == 3)
        {
            int targetId;
            double newSc;
            cout << "输入要修改的id 和新分数：";
            cin >> targetId >> newSc;

            int findIndex = -1;
            for (int i = 0; i < stuCount; i++)
            {
                if (stuArray[i].getId() == targetId)
                {
                    findIndex = i;
                    break;
                }
            }
            if (findIndex != -1)
            {
                modifyScore(stuArray[findIndex], newSc);
                cout << "修改成功" << endl;
            }
            else
            {
                cout << "未找到该学生" << endl;
            }
        }
        else if (op == 4)
        {
            int delId;
            cout << "输入要删除学生id：";
            cin >> delId;
            bool ok = deleteById(stuArray, stuCount, delId);
            if (ok)
                cout << "删除成功" << endl;
            else
                cout << "未找到学生" << endl;
        }
        else if (op == 5)
        {
            double avg = getAvg(stuArray, stuCount);
            cout << "全体平均分：" << avg << endl;
        }
        else
        {
            cout << "无效选项" << endl;
        }
    }

    cout << "程序结束" << endl;
    return 0;
}
```

## CLion 调试要点

1. 断点设置：`modifyScore(stuArray[findIndex], newSc);`，按 **F7** 跳入函数，观察引用修改对象。
2. **Variables** 窗口可以看到 class 对象内部私有成员。
3. **F7**：单步进入；**F8**：单步跳过；**F5**：启动调试。

## class 核心知识点

1. `private`：私有，类外部不能直接访问成员变量，不能写 `stuArray[i].id`。
2. `public`：公开接口，外部只能调用公开的成员函数 `setInfo() / getId() / getScore()` 读写数据。
3. `const` 修饰成员函数：`int getId() const`，代表这个函数**不会修改对象数据**。
4. C++ 的 `&` 引用：函数传参别名，修改的是原始对象，不是副本。

### struct vs class 在 C++ 中的区别

- `struct`：默认成员是 **public**
- `class`：默认成员是 **private**

## C / C++ / C# 横向汇总

| 语言  | 数据封装                  | 修改外部变量      | 对象实例化    |
| --- | --------------------- | ----------- | -------- |
| C   | struct，全部公开           | 指针 `* &`    | 栈上分配     |
| C++ | class(private/public) | 引用 `&` / 指针 | 栈 / new堆 |
| C#  | class(私有属性)           | `ref`       | new 堆对象  |

---

到此，你已经完整过一遍：
C(Dev‑C++/VS‑Code) → C#(Visual Studio) → C++(CLion)，三套同业务逻辑的学生管理系统。

下面的**横向对比总结表**，把语法、关键字、IDE快捷键全部汇总在一起，方便复习。

# 十九、C / C# / C++ 横向对比总结

> 业务完全一样：控制台学生管理系统（增删改查、数组存储），分别在 Dev‑C++、Visual Studio、CLion。

## 一、基础输入输出

| 项目    | C语言                      | C++            | C#                              |
| ----- | ------------------------ | -------------- | ------------------------------- |
| 输出    | `printf("%d",a);`        | `cout << a;`   | `Console.WriteLine(a);`         |
| 输入    | `scanf("%d",&a);`        | `cin >> a;`    | `int.Parse(Console.ReadLine())` |
| 字符串   | `char[]`，`strcpy/strcmp` | `std::string`类 | `string`类                       |
| 暂停控制台 | `system("pause");`       | 不需要            | `Console.ReadKey();`            |

## 二、结构体 / 类与访问权限

| 项目   | C语言                                 | C++                                      | C#                              |
| ---- | ----------------------------------- | ---------------------------------------- | ------------------------------- |
| 结构体  | `struct Student{ int id; };` 默认全部公开 | `struct` 默认 public<br>`class` 默认 private | `struct`：值类型<br>`class`：引用类型    |
| 私有封装 | 无，全部公开                              | `private / public`                       | `private / public`，属性`get;set;` |
| 读取成员 | `stu.id`                            | `stu.id` / `stu.getId()`                 | `stu.Id`                        |

## 三、如何在函数/方法中修改外部原始数据

> 这是三门语言最容易混淆的核心！

### C语言：指针

```c
void Modify(Student* s){ s->score = 90; }
// 调用：Modify(&stu);
```

### C++：引用（推荐） / 指针

```cpp
void Modify(Student &s){ s.setScore(90); }
// 调用：Modify(stu); // 直接传对象，不用取地址
```

### C#

- struct（值类型）：必须加 `ref`

```csharp
static void Modify(ref Student s){ s.score=90; }
```

- class（引用类型）：**不需要 ref**，直接修改对象成员

```cs
static void Modify(Student s){ s.Score=90; }
```

| 语言        | 值拷贝           | 修改外部原始数据     |
| --------- | ------------- | ------------ |
| C         | 传结构体变量是拷贝     | 指针 `*` + `&` |
| C++       | 传对象不加引用是拷贝    | 引用 `&` / 指针  |
| C# struct | 默认拷贝副本        | 必须 `ref`     |
| C# class  | 拷贝对象地址，不是数据副本 | 不需要ref       |

## 四、数组

| 项目   | C语言               | C++               | C#                              |
| ---- | ----------------- | ----------------- | ------------------------------- |
| 定义   | `Student arr[6];` | `Student arr[6];` | `Student[] arr=new Student[6];` |
| 元素个数 | 手动维护count         | 手动维护count         | `.Length`获取总长度                  |
| 数组传参 | 退化为指针             | 退化为指针             | 传递引用                            |

## 五、IDE 与快捷键汇总

### 1）Dev‑C++（C/C++）

- F9：编译

- F11：编译运行

- F5：启动调试

- F7：单步进入；Shift+F7：单步跳过
  
  > ⚠️调试前：工具 → 编译选项 → 勾选【加入调试信息】

### 2）Visual Studio（C# / C++）

- Ctrl+F7：编译
- Ctrl+F5：不调试运行（防止闪退）
- F5：启动调试
- F11：逐语句（跳进函数/方法）
- F10：逐过程（跳过）
- Shift+F11：跳出函数

### 3）CLion（C++，CMake）

- Ctrl+F9：编译
- Shift+F10：运行
- F5：启动调试
- F7：单步进入；F8：单步跳过；Shift+F8：跳出函数
- F9：运行到下一个断点

> 通用调试技巧：行号左侧点击设置**红色断点**，Variables/局部变量窗口展开查看结构体、数组，Watches/监视窗口自定义表达式。

## 六、核心概念类比记忆

| C语言              | C++              | C#               | 说明              |
| ---------------- | ---------------- | ---------------- | --------------- |
| `struct Student` | `struct Student` | `struct Student` | 值类型，赋值拷贝全部数据    |
| `Student*`结构体指针  | `Student &`引用    | `class Student`  | 引用语义，多个变量指向同一实体 |
| `&`取地址           | `&`引用符号          | `ref`参数          | 修改外部原始变量        |
| 函数               | 成员函数             | static 静态方法      | 代码块封装           |

## 七、学习路径小结

1. **C**：最底层，手动管理内存，靠指针，适合理解底层原理。
2. **C++**：C的超集，增加引用、类、面向对象、STL库，兼顾性能与抽象。
3. **C#**：.NET托管语言，垃圾回收不用手动释放内存，区分值类型/引用类型，开发效率高。

> 共同练习模式：
> 手写控制台菜单项目 + 打断点调试，不要只复制代码，观察每一步变量变化。

如果你接下来想继续深入，可以选择：

1. C++ STL：`std::vector`动态容器，代替固定大小数组
2. C++ 链表实现学生管理
3. C# List版本学生管理
