---
title: linuxdeployqt源码编译与使用
date: 2020-08-01 17:41:02
tags:
- Linux
- Qt
---
1. 安装patchelf与cmake, 修改qt环境变量
    `sudo apt install patchelf`
    
    打开.bashrc，增加下列内容，然后注销，重新登录。
    
    ```shell
    # add Qt
    #add QT ENV
    export PATH=/home/george/Qt5.14.2/5.14.2/gcc_64/bin:$PATH
    export LD_LIBRARY_PATH=/home/george/Qt5.14.2/5.14.2/gcc_64/lib:$LD_LIBRARY_PATH
    export QT_PLUGIN_PATH=/home/george/Qt5.14.2/5.14.2/gcc_64/plugins:$QT_PLUGIN_PATH
    export QML2_IMPORT_PATH=/home/george/Qt5.14.2/5.14.2/gcc_64/qml:$QML2_IMPORT_PATH
    ```
    
    
    
2. git clone linuxdeployqt
   `git clone https://github.com/probonopd/linuxdeployqt.git --depth=1`

3. 进入linuxdeployqt目录，修改tools/linuxdeployqt/main.cpp源码

4. 在linuxdeployqt目录使用cmake命令，`cmake CMakeList.txt`

5. 使用make命令，在tools/linuxdeployqt目录生成一个可执行文件,linuxdeployqt

6. 移动到/usr/local/bin方便使用

7. 在qt Creator中，选择release编译，将编译好的二进制文件，单独列出来，放到一个目录中。在该目录打开，使用`linuxdeployqt hello -appimage`等待编译完成。
8. 至此，我们使用linuxdeployqt打包完成，可以把整个目录放到其他计算机上使用了。
