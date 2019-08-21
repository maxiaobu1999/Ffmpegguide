./gradlew externalNativeBuildDebug
./gradlew media_arm:externalNativeBuildDebug


# /Users/v_maqinglong/Documents/AndroidProject/Ffmpegguide/media_arm/src/main/cpp
SET(USER_KEY "Hello World")
MESSAGE( STATUS "+++++++++++++ ${PROJECT_SOURCE_DIR}")



#库文件类型  STATIC | SHARED | MODULE
https://zh.wikibooks.org/zh/CMake_%E5%85%A5%E9%96%80/%E5%BB%BA%E7%BD%AE%E8%88%87%E9%80%A3%E7%B5%90%E7%A8%8B%E5%BC%8F%E5%BA%AB
STATIC：靜態程式庫。
SHARED：共享程式庫，例如 Unix like 系統上的 so 檔或 Windows 上的 dll 檔。在建置 dll 時也會連帶生成連結用的 .a 檔，例如建置 libcalc.dll 時也會生成 libcalc.dll.a。
MODULE：動態連結程式庫，不在編譯期進行連結，而是等到執行期才透過 dlopen() 或 LoadLibrary() 方式調用。一般情況下 MODULE 輸出的檔案類型也是 so 或 dll，但建立 dll 時不會順便產生 .a 檔。
库有两种：静态库（.a、.lib）和动态库（.so、.dll）。 windows上对应的是.lib .dll linux上对应的是.a .so
对象文件：*.o及*.obj,是可执行文件
https://pic2.zhimg.com/80/72b693726d70eea37aacbb93d8d40a43_hd.jpg
【静态库】，是因为在链接阶段，会将汇编生成的目标文件.o与引用到的库一起链接打包到可执行文件中。因此对应的链接方式称为静态链接。
静态库与汇编生成的目标文件一起链接为可执行文件，那么静态库必定跟.o文件格式相似。其实一个静态库可以简单看成是一组目标文件（.o/.obj文件）的集合，即很多目标文件经过压缩打包后形成的一个文件。静态库特点总结：
 静态库对函数库的链接是放在编译时期完成的。
l 程序在运行时与函数库再无瓜葛，移植方便。
l 浪费空间和资源，因为所有相关的目标文件与牵涉到的函数库被链接合成一个可执行文件。
【动态库】
https://pic2.zhimg.com/80/6aac2e2dc8faa8d1008f5320a7a83f5d_hd.jpg
https://pic3.zhimg.com/80/73f097608fecb37ffc4128273341376e_hd.jpg
动态库特点总结：
l 动态库把对一些库函数的链接载入推迟到程序运行的时期。
l 可以实现进程之间的资源共享。（因此动态库也称为共享库）
l 将一些程序升级变得简单。
l 甚至可以真正做到链接载入完全由程序员在程序代码中控制（显示调用）。










###############jni
#JNIEnv
JNIEnv类型是一个指向全部JNI方法的指针。该指针只在创建它的线程有效，不能跨线程传递
#JavaVM
JavaVM是虚拟机在JNI中的表示，一个JVM中只有一个JavaVM对象，这个对象是线程共享的。

