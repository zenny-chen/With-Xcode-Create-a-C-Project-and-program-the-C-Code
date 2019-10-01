# With Xcode Create a C Project and Program the C Code
用Xcode创建一个C语言项目并编写C语言程序

<br />

众所周知，macOS是广受开发者青睐的优秀开发平台。在macOS上我们可以直接编译几乎所有市面上主流的开源项目，而且macOS还预装了Python、PHP、Ruby等主流脚本语言。更重要的是，macOS采用的是FreeBSD与Mach微内核的融合性内核，因此大部分macOS上开发好的基于控制台的应用都能毫不费力地移植到Linux系统上去。

本博文将给大家介绍如何用Xcode创建一个C语言项目工程，并且如何编写C语言程序，最后再构建它。Xcode是最佳的开发C语言工程的IDE，没有之一。最新的Xcode 9能完美支持当前最新的GNU11标准，能支持GNU11所提供的各种关键字的语法高亮并且能正确地及时报错。结合Apple自己推出的Apple LLVM工具链，威力无穷！

<br />

### 下载安装Xocde并创建一个C语言的项目工程

我们在macOS上要下载最新的Xcode，请在Mac App Store直接搜索“Xcode”然后下载。千万别去第三方的网盘下载，否则很可能会中毒或者被留下后门！安装完Xcode之后，我们来到欢迎页面，先点击“Create a new Xcode Project”。
然后我们点击“macOS”选项卡，选择“Command Line Tool”，如下图所示。

![1.jpg](https://github.com/zenny-chen/With-Xcode-Create-a-C-Project-and-program-the-C-Code/blob/master/1.jpg)

完成之后，我们点击下一步来到设置项目名称以及项目基本属性的页面，这里要填写“Product Name”，即产品名称，本项目构建完成后所生成的可执行文件名就是它。然后，这里的Team可以选择“None”，因为我们不需要将控制台程序提交到Mac App Store，Apple也不会允许你提交一个控制台程序，呵呵。最后，这里最重要的是，我们要在最后的“Language”这项中选择“C”，别去选别的，如下图所示。

![2.jpg](https://github.com/zenny-chen/With-Xcode-Create-a-C-Project-and-program-the-C-Code/blob/master/2.jpg)

完成之后，我们再点击下一步，来到项目位置存储设置的页面。这里我们在文件目录对话框中把当前项目工程所要存放的路径设置好之后，然后务必需要检查一下下面的“Create Git repository on my Mac”是否被勾选上了，如果被勾选上则**必须取消勾选**，否则Xcode将会自动在你计算机本地做该项目的版本管理，这会给当前文件系统造成混乱，同时你的项目工程也会变得很大！

![3.jpg](https://github.com/zenny-chen/With-Xcode-Create-a-C-Project-and-program-the-C-Code/blob/master/3.jpg)

设置完路径之后，我们最后点击“Create”按钮，该项目工程就建立完毕了。我们随后就马上能看到该工程已经默认创建了一个C语言源文件，名为main.c，然后在此源文件中已经生成了输出“Hello, world”的模板代码。如下图所示。

![4.jpg](https://github.com/zenny-chen/With-Xcode-Create-a-C-Project-and-program-the-C-Code/blob/master/4.jpg)

随后，我们点击蓝色的“CTest”这一项目文件，然后在中间栏我们选中“**TARGETS**”下面的项目名。在右侧栏，我们先选中上方的“**General**”选项卡，然后在下方“**Deployment Target**”中选择版本较低的系统。Deployment Target表示当前构建出来的程序可以在最低哪个系统版本上运行。默认情况下是当前你正在用的系统版本，而这里笔者选的是“macOS 10.10”。

![5.jpg](https://github.com/zenny-chen/With-Xcode-Create-a-C-Project-and-program-the-C-Code/blob/master/5.jpg)

如果各位看不到中间栏，或者中间栏显示不全，那么可以点击上图中所标注的小方框。

随后，我们选中“**Build Settings**”选项卡。这个选项卡用来设置当前项目工程的编译、构建选项相关的属性。在下方默认选中的是“Basic”，但我们需要列出所有属性，因此我们得选中“All”。然后我们向下滚动，找到“**Apple LLVMx.x - Language**”这一栏，先确保这里的C语言标准使用的是gnu11标准。然后我们再找到下方的“ **Apple LLVM x.x - Language - C++** ”这一栏，将“ **Enable C++ Exceptions** ”以及“ **Enable C++ Runtime Types** ”全都设置成**No**！这有助于减少我们代码体积，另外也能提升少许运行时性能。因为我们的项目工程也不需要使用C++。

![6.jpg](https://github.com/zenny-chen/With-Xcode-Create-a-C-Project-and-program-the-C-Code/blob/master/6.jpg)

这些全都设置完之后，我们就可以写一些C语言的测试代码了。当然，以上所描述的这些编译选项仅仅是最最基本的一些选项，还有许多选项各位可以从其他网络渠道搜索。这里再要简单介绍的是，当我们要引用当前工程的相对路径时，我们不需要把这个路径写死为自己本地的路径，Xcode有一个 `$(SRCROOT)` 常量，表示当前项目工程所在的根目录路径。如果我们在本工程中的main.c所在的目录又创建了一个名为test的文件夹，并且要对它访问的话，可以在相关搜索路径中填写：`$(SRCROOT)/CTest/test`
另外，还有一个常用的常量是 `$(PROJECT_NAME)`，这个常量指示当前项目名称。

<br />

### 编写C语言代码并进行构建、调试

下面各位先复制粘贴以下代码：
```c
#include <stdio.h>
#include <stdbool.h>
#include <stdint.h>
#include <stdatomic.h>
#include <stdnoreturn.h>
#include <stdlib.h>
#include <assert.h>
#include <complex.h>

#ifndef var
#define var     __auto_type
#endif

/// noreturn测试函数
static int noreturn MyTest(void)
{
    const bool equal = sizeof(long) == sizeof(int);
    printf("Is equal? %s\n", equal ? "Yes" : "No");
    
    // 原子操作测试
    volatile atomic_int atom;
    atomic_init(&atom, 0);
    const var oldValue = atomic_fetch_add(&atom, 100);
    printf("Previous value = %d, current value = %d\n", oldValue, atomic_load(&atom));
    
    volatile atomic_flag flag = ATOMIC_FLAG_INIT;
    if(!atomic_flag_test_and_set(&flag))
        puts("Access the resource...");
    else
        puts("Locked!");
    
    if(!atomic_flag_test_and_set(&flag))
        puts("Access the resource...");
    else
        puts("Locked!");
    
    atomic_flag_clear(&flag);
    if(!atomic_flag_test_and_set(&flag))
        puts("Access the resource...");
    else
        puts("Locked!");
    
    exit(0);
}

int main(int argc, const char * argv[])
{
    // 字符编码测试
    const char *utf8Str = u8"你好，世界！";
    printf("The string is: %s\n", utf8Str);
    
    const var utf16Char = u'¥';
    printf("The UTF-16 code is: %.4X\n", utf16Char);
    
    const char encoding = _Generic(utf16Char, uint8_t:'c',
                                   uint16_t:'s', uint32_t:'i', default:'o');
    
    printf("The type of utf16Char is: %c\n", encoding);
    
    // 复数测试
    complex float comp1 = 5.0f + 3.0fi;
    complex float comp2 = {3.0f, -2.0f};
    comp1 -= comp2;
    printf("comp1 real = %f, imag = %f\n", __real(comp1), __imag(comp1));
    
    // 静态断言测试
    static_assert(sizeof(long) == 8, "Not 64-bit environment!");
    
    return MyTest();
}
```
编写完上述代码之后，我们点击上方的三角按钮，Xcode即会对该项目进行编译构建。

![7.jpg](https://github.com/zenny-chen/With-Xcode-Create-a-C-Project-and-program-the-C-Code/blob/master/7.jpg)

<br />

### 利用Xcode调试C程序

我们写完一个程序之后，如果这个程序在运行过程中出现些问题，那么我们就要对它进行**调试**（**Debug**）。调试最常用的手段就是设置**断点**（**breakpoint**）。在Xcode中设置断点非常简单，我们只需在编辑栏左侧行号这边点击某一行号，那么就能在该行处设置断点了。一旦在某一行含有断点，那么我们在调试模式下运行程序时，每当执行到此行程序就会中断执行。此时，我们可以观察当前程序的上下文，比如观察相关的全局对象、局部对象的值，甚至可以观察当前寄存器的值以及指定内存地址的值等等。当我们点击“继续”按钮时，程序将会继续执行。Xcode中的断点用的是淡蓝色矩形箭头表示。如果我们对一个断点进行左键双击，那么就会弹出设置该断点成立的条件对话框。此时，只有当自己设定的条件表达式成立时，该断点才会起作用。如果我们左键单击一个断点，那么它将会被禁用，此时它呈灰色状态。再对它单击一次即可恢复成可用状态。

![8.jpg](https://github.com/zenny-chen/With-Xcode-Create-a-C-Project-and-program-the-C-Code/blob/master/8.jpg)

上图中，第63行的断点就处于禁用状态，而第66行的断点则呈现出了设置条件断点的对话框。下面我们实际演练一下。

![9.jpg](https://github.com/zenny-chen/With-Xcode-Create-a-C-Project-and-program-the-C-Code/blob/master/9.jpg)

我们在第72行处设置断点，然后以调试模式运行该程序，程序执行到72行的代码处中断。然后我们看到下方左侧白色区域中列出了当前上下文已经计算得出的局部对象的名称以及其值。我们可以右键单击该对象，然后可观察其更多信息或是它的其他形式下的信息。我们也可以直接修改某个对象的值。我们先单击选中它，然后再对它单击一次，此时它的值部分就出现可编辑的状态，此时我们就可以对它进行修改了。如果一个对象的值当前被修改了，那么它接下去执行的时候所携带的就是当前已被修改的值。我们点击红框框出来的三角箭头即可继续执行程序。请注意，我们要继续执行程序一定要按这个小箭头，如果按此前的大箭头按钮就会重新运行程序。

下方右侧黑色区域则是控制台区域，我们可以在该区域通过输入LLDB调试器的命令来获得更多信息。我们可以通过输入 **help** 来查看LLDB的帮助文档。
对于一般开发者来说，最有用的命令莫过于expr或po命令了。两者都是用来实时计算一个表达式的值的。我们参看下图。

![10.jpg](https://github.com/zenny-chen/With-Xcode-Create-a-C-Project-and-program-the-C-Code/blob/master/10.jpg)

上图中，我们每执行完一条expr命令就会出现当前计算结果，并且该结果会存放到以\$符号打头的变量中去。LLDB调试器厉害的一点是，你可以直接用该计算过的\$打头的变量来做其他计算，如上图第三行expr命令所示。

<br />

### 文档化注释

我们首先讲一下Xcode支持的 **MARK** 以及 **TODO** 注释。其形式如下：
```c
// MARK: 标注程序段落
// TODO: 标记自己所写的代码
```

下面我们先来看下使用这俩注释之后的效果。

![11.jpg](https://github.com/zenny-chen/With-Xcode-Create-a-C-Project-and-program-the-C-Code/blob/master/11.jpg)

我们看到，当我们点击main.c旁边的“No Selection”这一项之后，会显示相应的程序分段以及注解名称。这使得我们要找到相关代码、相关函数就非常方便快捷了。

Xcode支持Doxygen文档化注释。我们可以在C、C++以及Objective-C中使用这种形式的文档化注释，而对于Swift则使用的是Apple自己定义的Markup文档化注释形式。我们可以使用 `/** ...  */` 与 `/// ...` 这两种文档化注释形式。笔者偏好使用后者形式。我们可以对自定义类型、静态或全局对象以及函数使用文档化注释形式。下面笔者将列出我们常用的一些文档化注释参数。

![12.jpg](https://github.com/zenny-chen/With-Xcode-Create-a-C-Project-and-program-the-C-Code/blob/master/12.jpg)

当我们对某个函数、对象或类型写上文档化注释之后，我们在使用它们时，使用option+鼠标左键即可看到它相应的信息了，如上图所示。此外，我们鼠标单击包含文档化注释的标识符，也可以直接在右侧快速帮助中查看到它的相关信息。

![13.jpg](https://github.com/zenny-chen/With-Xcode-Create-a-C-Project-and-program-the-C-Code/blob/master/13.jpg)

我们点击右侧上方的圆形问号按钮即可切换到快速帮助一栏。关于Xcode使用Doxygen文档化注释的更多信息可参考《[DOCUMENTING OBJECTIVE-C WITH DOXYGEN PART I](https://duckrowing.com/2010/03/18/documenting-objective-c-with-doxygen-part-i/)》

