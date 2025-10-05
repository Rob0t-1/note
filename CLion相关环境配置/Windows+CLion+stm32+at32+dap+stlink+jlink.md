>参考
> - [Jecjune师兄的笔记](https://gitee.com/jecjune/embedded-build/tree/master/Clion+Cmake%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA)
> - [CLion2025.1.1开发Cubemx生成的Cmake的stm32工程中的烧录程序和调试教程(例子里是实现stm32f103c8t6最小系统板的闪烁灯)，调试一次配置长久使用](https://blog.csdn.net/headerrr/article/details/148078027?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522226f051a0402a4fe8fce6df342e5f7ca%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=226f051a0402a4fe8fce6df342e5f7ca&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-148078027-null-null.142^v102^pc_search_result_base8&utm_term=clion%20stlink%E7%83%A7%E5%BD%95&spm=1018.2226.3001.4187)
> - [AT32 CLion CMakeList 模板](https://blog.csdn.net/weixin_55772937/article/details/145095788?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522c2b5ad7f4badacb3533cb60df55b35a2%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=c2b5ad7f4badacb3533cb60df55b35a2&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-145095788-null-null.142^v102^pc_search_result_base8&utm_term=clion%20at32&spm=1018.2226.3001.4187)
> - [AT32-CLion开发环境搭建](https://zhuanlan.zhihu.com/p/648357234)
> - [使用STM32 ST-LINK Utility 烧录程序，ST LINK烧录程序，解锁FLASH](https://blog.csdn.net/yutian0606/article/details/130115488?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-130115488-blog-134734841.235^v43^pc_blog_bottom_relevance_base3&spm=1001.2101.3001.4242.1&utm_relevant_index=3)

## Windows+CLion+stm32
我一开始是跟着keysking这个视频配的
https://www.bilibili.com/video/BV1pnjizYEAk/?spm_id_from=333.337.search-card.all.click&vd_source=14559bb1e73d126a14fd8d8c1a6a69e4
>==**注意，工程路径不要有中文**==
后面配的时候如果出现没有小锤子的情况，按下图那样点进去之后发现那个目标找不到就有可能是路径有中文导致的
![[pic1.png]](picture/pic1.png)
![[pic2.png]](picture/pic2.png)

### 下载资源
1. 在ARM官网下载编译工具。exe的是安装程序，zip的是免安装版，后续教程选取免安装版。网址：[Downloads | GNU Arm Embedded Toolchain Downloads – Arm Developer](https://developer.arm.com/downloads/-/gnu-rm)
![[pic3.png]](picture/pic3.png)
1. 下载MINGW。MINGW是windows下集成了一系列免费的头文件和库文件的包，其中还整合了GNU工具链。网址：[MinGW Distro - nuwen.net](https://nuwen.net/mingw.html)
2. 下载J-link下载器，网址[SEGGER - The Embedded Experts - Downloads - J-Link / J-Trace](https://www.segger.com/downloads/jlink/)
![[pic4.png]](picture/pic4.png)
1. 下载Openocd，Openocd通俗点讲就是一个支持多款烧录器的上层管理软件，类似于烧录器的命令行。网址：[Download OpenOCD for Windows (gnutoolchains.com)](https://gnutoolchains.com/arm-eabi/openocd/)
![[pic5.png]](picture/pic5.png)

### 安装
1. 安装MINGW
可能会出现警告，点击更多信息然后点运行即可：
![[pic6.png]](picture/pic6.png)
该程序会把文件解压到目标目录下，请根据需求修改该目录的位置。
2. 解压OpenOCD和gcc-arm-none-eabi，建议将三个配置文件解压到同一个目录下（方便配置环境变量），注意检查目录路径，不要带有中文。
![[pic7.png]](picture/pic7.png)
3. 安装Jlink驱动，路径可以保持默认，需要记得安装路径
![[pic8.png]](picture/pic8.png)

### 配置环境变量
![[pic9.png]](picture/pic9.png)
![[pic10.png]](picture/pic10.png)
![[pic11.png]](picture/pic11.png)
![[pic12.png]](picture/pic12.png)
这里需要将刚刚三个文件的bin文件路径加到PATH里面
例如：
```
C:\Users\Jecjune\Downloads\Clion_Config\OpenOCD-20230712-0.12.0\bin
```
![[pic46.png]](picture/pic46.png)
添加完成后可以通过打开cmd检查，win+R进入命令行窗口。
![[pic13.png]](picture/pic13.png)
输入以下指令：
```php
g++ -v
```
```php
openocd -v
```
```php
make -v
```
如果出现类似以下输出，就是设置成功了。
![[pic14.png]](picture/pic14.png)
然后 ==**重启电脑**==

### Clion内部配置
我的CLion显示的是中文，所以下面的图片都是中文版，如果你的是英文怕找不到路径可以去看我开头贴出来的Jecjune师兄的笔记或者看贴出来的文字
1. 先拉取一个示例代码作为工程模板：[stm32f407-template: stm32f407代码的cmake版本 (gitee.com)](https://gitee.com/jecjune/stm32f407-template)。可以通过git拉取，也可以通过下载zip的方式。
      >或者自己用CubeMX创建一个工程(不过好像keysking的那个视频也教了？不管了)：
      左上角三条横杠鼠标移上去显示这个界面
      ![[pic15.png]](picture/pic15.png)
      打开CubeMX
      ![[pic16.png]](picture/pic16.png)
      ![[pic17.png]](picture/pic17.png)
      想选哪个芯片选哪个
      ![[pic18.png]](picture/pic18.png)
      ==**一定要选！！！nodebug烧录会把芯片锁住的！！！**==
      如果已经锁住了看我后面贴的方法解锁吧
      ![[pic19.png]](picture/pic19.png)
      ![[pic20.png]](picture/pic20.png)
      选一个接了LED的GPIO口方便后面观察程序烧进去没
      ![[pic21.png]](picture/pic21.png)
      主频按你选的芯片来
      ![[pic22.png]](picture/pic22.png)
      Toolchain/IDE这里注意要配置成CMake
      ![[pic23.png]](picture/pic23.png)
      我喜欢勾上这个
      ![[pic25.png]](picture/pic25.png)
      生成工程
      ![[pic24.png]](picture/pic24.png)
      复制生成工程所在路径
      ![[pic26.png]](picture/pic26.png)
      粘贴到Clion这里，继续
      ![[pic27.png]](picture/pic27.png)
      这里默认的就行，确定
      上面那个"...重新加载CMake项目"的选项可以勾上，后面一般情况下就不用手动点重新加载CMake项目了
      ![[pic28.png]](picture/pic28.png)
2. 使用clion打开你的工程文件。
==**后面使用的图片大部分是我配好了之后才截的图，你的界面跟我图片上不一样也别慌**==
![[pic47.png]](picture/pic47.png)
![[pic48.png]](picture/pic48.png)
![[pic49.png]](picture/pic49.png)
![[pic29.png]](picture/pic29.png)
3. 左上方然后点击“文件”，然后选择“设置”
![[pic30.png]](picture/pic30.png)
![[pic31.png]](picture/pic31.png)

开始检查：找到“Build,Execution,Deployment”下的“Toolchains”。将Toolset选项选择为刚刚解压的MinGW的文件夹，指定C Compiler和C++ Compiler为刚刚下载的。将Debugger设置为gcc-arm下的arm-gcc-none-eabi-gdb.exe。
直接点"工具链"，不用点左边那个">"展开
![[pic32.png]](picture/pic32.png)
在Embedded Development栏设置Openocd.exe和STM32CubeMX.exe的位置路径。然后点击Test，弹出绿色气泡则没有问题。cubemx有的话可以加上，不需要可以不设置。
![[pic33.png]](picture/pic33.png)
![[pic34.png]](picture/pic34.png)
点击ok保存设置，然后Reload Cmake Project，然后点击右上角的小锤子开始编译工程。
![[pic35.png]](picture/pic35.png)
![[pic37.png]](picture/pic37.png)
如果出现如下编译信息，即工具链配置成功
主要是那个"构建 已完成",上面的不一样应该没关系，不管出来红的蓝的绿的啥乱七八糟颜色的，只要最后说构建成功那应该就没问题了
![[pic36.png]](picture/pic36.png)

## +stlink/dap
主要用stlink的配置来做示范，dap的步骤也差不多，有不一样的地方我会标出来
>确认一下有没有启用调试服务器
不会打开设置的罚你再看一遍上面的内容
![[pic38.png]](picture/pic38.png)
按图片路径看看有没有这个ST-LINK ==**(如果是dap就不用看了)**==
(跟着视频配的应该是有的，我是没有自己再配了，如果没有就点那个+号然后选ST-LINK，其它不用管，点确定就行)
![[pic39.png]](picture/pic39.png)

在根目录下新建一个.cfg配置文件，名称随机，只要是英文
但是命名最好还是写点自己能看懂的
![[pic40.png]](picture/pic40.png)
![[pic41.png]](picture/pic41.png)
.cfg配置文件内容如下（如果是dap就把下载器路径最后那里改成cmsis-dap.cfg）
（你可以把stlink的和dap的都弄了，到时候想用哪个用哪个）
指定下载器stlink.cfg的绝对路径
`source [find D:/CLion_huanjing/OpenOCD-20250710-0.12.0/share/openocd/scripts/interface/stlink.cfg]`
指定芯片类型（要用绝对路径）
`source [find D:/CLion_huanjing/OpenOCD-20250710-0.12.0/share/openocd/scripts/target/stm32f1x.cfg]`
==**如果直接复制路径要记得把反斜杠"\\"全部改成正斜杠"/"**==
stlink版：
```
# choose st-link/j-link/dap-link etc.
#adapter driver cmsis-dap
#transport select swd
source [find D:/CLion_huanjing/OpenOCD-20250710-0.12.0/share/openocd/scripts/interface/stlink.cfg]
transport select swd
source [find D:/CLion_huanjing/OpenOCD-20250710-0.12.0/share/openocd/scripts/target/stm32f1x.cfg]
# download speed = 10MHz
adapter speed 10000
```
dap版：
```
# choose st-link/j-link/dap-link etc.
#adapter driver cmsis-dap
#transport select swd
source [find D:/CLion_huanjing/OpenOCD-20250710-0.12.0/share/openocd/scripts/interface/cmsis-dap.cfg]
transport select swd
source [find D:/CLion_huanjing/OpenOCD-20250710-0.12.0/share/openocd/scripts/target/stm32f1x.cfg]
# download speed = 10MHz
adapter speed 10000
```
![[pic42.png]](picture/pic42.png)
点这里，编辑配置
![[pic43.png]](picture/pic43.png)
点+号，选OpenOCD下载并运行
![[pic44.png]](picture/pic44.png)
将刚才的.cfg的文件路径添加进去，点击确定
![[pic45.png]](picture/pic45.png)
写一个简单的LED闪烁测试程序
```
HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_SET);
HAL_Delay(500);
HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_RESET);
HAL_Delay(500);
```
![[pic50.png]](picture/pic50.png)
这里我不调试，只是烧录程序，所以我这边设置为原生，如果想调试就再将ST-link选上就行。如果只是单纯的烧录程序，就不要选择 ==**（dap的话调试也是用的原生，不用改）**==
![[pic51.png]](picture/pic51.png)
环境配置完毕，点击小锤子进行构建
![[pic52.png]](picture/pic52.png)
然后点这个三角形烧录程序，记得把st-link连上板子接电脑上
![[pic53.png]](picture/pic53.png)
烧录成功，耶
![[pic54.png]](picture/pic54.png)
看看板子上的LED有没有闪烁现象，如果没有就按一下复位键试试

## +JLink
> 烧录还没试过，因为JLink太贵了！等我试试把st-link刷成JLink


## +at32
接着上面stm32的配
### 下载资源
需要下载Artery官方定制的OpenOCD版本，与stm32的不能混用
[官方下载链接](https://www.arterytek.com/cn/support/)
打开网站后点这个
![[pic59.png]](picture/pic59.png)
搜索VSCode，点下面这个下载
![[pic60.png]](picture/pic60.png)
下载成功，解压
![[pic61.png]](picture/pic61.png)
按上面这个路径点进去，把这个文件夹挖出来，放哪都行，不拿出来也行，不过你要能找得到路径
![[pic62.png]](picture/pic62.png)
### 下载AT32 Workbench
[官方网站下载链接](https://www.arterytek.com/cn/support/tools.jsp?index=0)
点这个下载
![[pic64.png]](picture/pic64.png)
打开文件夹按上面这个路径找到.exe，双击打开
![[pic65.png]](picture/pic65.png)
如果语言不对点右上角改一下，然后搜MCU，选好后下面都不动，直接点击新建
![[pic66.png]](picture/pic66.png)
配置
![[pic67.png]](picture/pic67.png)
配置
![[pic68.png]](picture/pic68.png)
配个接了LED的GPIO口看现象
![[pic69.png]](picture/pic69.png)
时钟一般已经自动配到最大了，看一眼就行
![[pic70.png]](picture/pic70.png)
点页面上方的生成代码，来到这个界面，名字随便取一下，位置你自己选，工具链要选CMake。  
第一次打开可能提示你固件包不存在，点击右边的固件包管理下载
![[pic71.png]](picture/pic71.png)
路径可以自己改，但是改了有什么影响我不清楚。  
芯片型号自己选，然后选一个下载方式，我这里选从本地安装会莫名其妙卡住，所以选择这个从网络安装
![[pic72.png]](picture/pic72.png)
等待下载完成
![[pic73.png]](picture/pic73.png)
下载好之后这里有个勾，点击关闭
![[pic74.png]](picture/pic74.png)
现在好了，点击确定
![[pic75.png]](picture/pic75.png)
OK！点打开文件夹
![[pic76.png]](picture/pic76.png)
右键CMakeLists.txt文件用CLion打开
![[pic77.png]](picture/pic77.png)
直接点确定
![[pic78.png]](picture/pic78.png)
### CLion内部配置
点开设置，修改OpenOCD路径为刚刚下载的OpenOCD_V2.0.2里的bin的openocd.exe(如果后面要用回stm32则要改回去，我试了不兼容，不知道啥区别)
![[pic63.png]](picture/pic63.png)
然后按上面配置daplink.cfg的方法走一遍，注意路径都要改成OpenOCD_V2.0.2里的
例如
```
# choose st-link/j-link/dap-link etc.
#adapter driver cmsis-dap
#transport select swd
source [find D:/CLion_huanjing/OpenOCD_V2.0.2/share/openocd/scripts/interface/cmsis-dap.cfg]
transport select swd
source [find D:/CLion_huanjing/OpenOCD_V2.0.2/share/openocd/scripts/target/at32f403axx.cfg]
# download speed = 10MHz
adapter speed 10000
```
然后就可以烧录了！

## 如果你还想用CLion写C/C++：
此时你会发现不行了！
其实是因为前面配工具链的时候直接在默认的那里改了，只需要新添加一个工具链，然后什么都不改，点击确定
![[pic79.png]](picture/pic79.png)
新建项目，路径改一下
![[pic80.png]](picture/pic80.png)
红红的！咋办？  
点击修正，选择为当前文件配置
![[pic81.png]](picture/pic81.png)
点击工具链，选择你刚刚创建的，然后直接点下面的确定
![[pic82.png]](picture/pic82.png)
点击运行，成功打印Hello,World!
![[pic83.png]](picture/pic83.png)

## 解锁FLASH(stm32)
> - 什么情况可能是锁住了呢：
  烧录时连不上(如果用keil就是no target connect)，且接线没问题、stlink也没问题、芯片也没烧

这个解锁方法为stlink+stm32芯片+ST-LINK Utility，别的搭配我没试过，可以自己去试试
- 下载ST-LINK Utility
  > 具体下载步骤没截到图就不说了，一路确认然后路径可以自己选一下，我没在这一步遇到啥问题
  > [下载链接（是github的链接，如果上不去你得去学科学上网，或者去下江科大的工具软件资源，我这个就是从那掏的）](https://github.com/Rob0t-1/SoftWare_Tool/tree/main)
- 点开.exe
  上面三个参数改成和图片上一样,应该主要是改Size
  ![[pic55.png]](picture/pic55.png)
  把板子接stlink连电脑上，把板子上的BOOT0接高电平(1)，然后点这个连接
  ![[pic56.png]](picture/pic56.png)
  连接上之后点这个橡皮擦擦除
  ![[pic57.png]](picture/pic57.png)
  擦除成功，记得把BOOT0接回地(0)然后再尝试烧程序，不然虽然能成功烧录但是是没有现象的
  ![[pic58.png]](picture/pic58.png)