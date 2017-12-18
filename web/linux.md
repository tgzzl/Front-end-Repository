### 共享文件 for Mac
---
- 安装扩展包 Oracle_VM_VirtualBox_Extension_Pack-5.1.8-111374.vbox-extpack [virtualbox官网](https://www.virtualbox.org/wiki/Downloads)
- 加载VBoxGuestAdditions.ios [下载地址](http://download.virtualbox.org/virtualbox/)
- 设置共享文件夹，并勾选自动挂载和固定分配
- 启动Ubuntu，运行扩展包软件，等待系统执行完毕
- 这时可以看到共享的文件了。Mac系统在系统偏好里添加需要共享的文件夹的访问权限
- 普通用户不能访问，执行 sudo usermod -aG vboxsf $(whoami) 将当前用户加入到vboxsf组就行了

### 使用 zsh oh-my-zsh 替代 bash
---
- sudo apt-get install zsh
- 参考设置 https://github.com/robbyrussell/oh-my-zsh

### Linux 常用命令
---

#### ==ls 列出目录内容==
- ##### ls

    方便区分目录和文件。目录为蓝色；可执行文件为草绿色；链接文件为淡蓝色；普通文件为黑色

- ##### ls -F

    在目录后加/；在可执行文件后加*；在链接文件后加@

- ##### ls -a

    列出当前目录所有文件，包含隐藏的文件。

- ##### ls -l

    查看文件属性。从左到右依次为：权限、链接文件个数、用户、用户组、文件大小、最后修改日期、最后修改时间、文件名

- ##### ls -ld demo/

    查看文件夹属性

- ##### ls /usr/bin

    查看指定目录下的文件

#### ==dir & vdir 列出目录内容==
- ##### dir

    linux中，dir除了比ls功能少，其它都一样

- ##### vdir

    vdir相当于``` ls -l ```，列出文件的完整信息

#### ==cat & more 查看文本文件==
- ##### cat -n demo.txt

    查看文本时显示行号

- ##### more

    more可以分页显示文件内容。按下 空格键 向下翻页，按下 回车键 向下滚动一行，按下 Q键 退出

#### ==head & tail 查看文件开头、结尾==
- ##### head -n 5 demo.txt

    查看文本开头5行

- ##### tail demo.txt

    查看文件末尾内容

#### ==less 更好的文本阅读==
- ##### less demo.txt

    less更像vim，只是去掉的编辑功能。按下 空格键 向下翻页，按下 B键 向前翻页。输入“/”跟上要查找的内容，less会把找到的目标高亮显示。

- ##### less -M demo.txt

     命令可以显示更多文件信息，比如文件名、当前页码等

#### ==grep 查找文件内容==
- ##### grep [options] pattern [file...]

    grep 'this is search content' demo.txt 命令会将文件中出现匹配内容的行输出

#### ==find 指定目录下查找文件==
- ##### find [options] [path...] [expression]

- [x]     使用 -type 选项来定位特殊文件类型

    参数 | 含义 | 参数 | 含义
    ---|---|---|---
    b | 块设备文件 | c | 字符设备文件
    d | 目录文件   | f | 普通文件
    p | 命名管道   | l | 符号链接

- [x]     通过指定时间来指导 find 命令查找文件

    -atime n 查找最后一次使用在n天前的文件

    -mtime n 查找最后一次修改在n天前的文件

    使用 +n -n 来限定时间范围。+n表示大于n，-n表示小于n

> find ~/ -name *.c -print 命令会列出用户主目录下所有的c程序文件

> find . -type f -atime -5 -print 查找当前目录下最近5天内使用过的文件

#### ==whereis 查找特定程序==
- ##### whereis -b find 查找find命令的二进制可执行文件

### 文件管理
---

- ##### mkdir -p ~/tmp/test 创建一个完整的目录结构

- ##### mv -i demo.txt tmp/ 移动文件，如果有重名文件会给出提示

- ##### mv -b demo.txt tmp/ 移动文件，系统会将目标目录的同名文件在其文件名后面加“~”，有效避免覆盖

- ##### cp -i demo.txt tmp/ 复制文件，如果有重名文件会给出提示

- ##### cp -b demo.txt tmp/ 复制文件，系统会将目标目录的同名文件在其文件名后面加“~”，有效避免覆盖

- ##### cp -r demo/ tmp/ 复制文件，将子目录及其其中的文件全部复制

- ##### rm -f demo.txt 删除有写保护的文件时避免系统的确认提示

- ##### rm -r demo/ 完整删除目录

- ##### chown & chgrp 改变文件所有权

    chown [option]... [owner][:[group]] file...

    chown -R tg:root tmp/ 更改目录及其下所有文件的所有权

    chgrp -R root tmp/ 更改目录及其下所有文件的所有权

- ##### chmod 改变文件权限

>     这个命令还用“用户组+/-权限”的表达式来添加/删除相应的权限
>     用户组包含文件属性主（u）、文件属组（g）、其他人（o）和所有人（a）;权限包含读（r）、写（w）、执行（x）
>     chmod a-x demo ; chmod u+x demo ; chmod ug=rw,o=r demo ;chmod o=u demo

>     文件权限的八进制表示
>     1：x，2：w，4：r ；rwx=4+2+1=7 ; chmod 775 demo


### apt-get & apt-cache
---

    命令 | 描述
    ---|---
    apt-get install | 下载安装
    apt-get upgrade | 更新系统上已下载的软件包
    apt-get remove  | 卸载
    apt-get source  | 下载特定的软件源代码
    apt-get clean   | 删除所有已下载的包文件
    apt-cache search   | 模糊搜索
    apt-cache depends  | 列出指定包的依赖关系


==从源代码编译软件==

-    **tar zxvf**  xxx.tar.gz #解压源代码压缩包
-    **./configure --prefix**=/usr/local/yourdir #设置安装目录
-    make #编译源代码
-    sudo make install #安装


==打包/解包 & 压缩/解压==

- tar -cvf demo.tar demo/ #打包demo目录
- tar -xvf demo.tar #解包

    参数 | 描述
    ---|---
    c | 创建归档
    x | 解开归档
    v | 显示执行过程
    f | 指定归档文件名
    w | 逐个将单个文件归档时向用户征求意见
    z | 自动调用gzip压缩/解压

