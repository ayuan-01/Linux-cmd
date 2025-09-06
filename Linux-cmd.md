# 文本处理命令

## sed

`sed`（stream editor）是一个用于处理和转换文本流的命令行工具，特别擅长在文本中查找、替换、插入、删除等操作。`sed`在对文件或标准输入的每一行进行处理时，不会修改原始文件，而是输出修改后的文本。

1. **替换命令** `s` 替换匹配的字符串。

   ```
   sed 's/old_text/new_text/' file.txt
   ```

   将`file.txt`中第一次出现的`old_text`替换为`new_text`。如果要替换每一行的所有匹配项，可以加上`g`选项：g选项表示全局匹配，没有g选项表示只匹配第一个。

   ```
   sed 's/old_text/new_text/g' file.txt
   ```

2. **删除行** `d` 删除特定行或满足条件的行。

   ```
   sed '2d' file.txt
   ```

   删除文件`file.txt`中的第2行。

3. **打印行** `p` 打印匹配的行，通常和`-n`选项一起使用。-n表示只打印匹配的行，否则会打印所有的行，匹配的会打印两次。

   ```
   sed -n '/pattern/p' file.txt
   ```

   只打印匹配`pattern`的行。

4. **插入内容** `i` 在指定行前插入文本。

   ```
   sed '2i\This is a new line' file.txt
   ```

   在第2行前插入`This is a new line`。

5. **追加内容** `a` 在指定行后追加文本。

   ```
   sed '2a\This is an added line' file.txt
   ```

6. **替换匹配范围**（多行操作） 你可以指定多个行的范围来进行操作，例如替换从第2行到第5行的某个字符串：

   ```
   sed '2,5s/old_text/new_text/' file.txt
   ```

## head/tail

用于查看文件的开头和结尾部分，默认10行

```sh
head test.txt
tail test.txt
```

## more/less

分页查看文件内容，more是less的早期版本，现在一般使用less。

仅介绍less的常用命令。

```
空格键：向下翻页
b：向上翻页
上下方向键：移动一行
g：文件开头
G：文件末尾
50%：跳到中间
/patten：向前搜索
?patten：向后搜索
n：跳到下一个匹配项
N：跳到上一个匹配项
-N：显示行号
v：使用默认编辑器打开当前文件
```

## nl

类似`cat -n`

## grep

匹配特定字符

```sh
-i：不区分大小写。
-n：显示匹配行及行号。
-E：扩展的正则表达式
-l：查询多文件时只输出包含匹配字符的文件名。
```

## 正则表达式

| 元字符 | 说明                             | 示例      | 匹配示例                    |
| :----- | :------------------------------- | :-------- | :-------------------------- |
| `.`    | 匹配任意**单个**字符（除换行符） | `a.c`     | "abc", "a-c", "a c"         |
| `*`    | 匹配前面的字符**0次或多次**      | `ab*c`    | "ac", "abc", "abbc"         |
| `+`    | 匹配前面的字符**1次或多次**      | `ab+c`    | "abc", "abbc" (不匹配 "ac") |
| `?`    | 匹配前面的字符**0次或1次**       | `colou?r` | "color", "colour"           |
| `\d`   | 匹配任意数字                     | `\d\d`    | "12", "45", "99"            |
| `\w`   | 匹配字母、数字、下划线           | `\w+`     | "hello", "abc123", "test_"  |
| `\s`   | 匹配空白字符（空格、制表符等）   | `a\sb`    | "a b"                       |
| `^`    | 匹配字符串的**开始**             | `^Hello`  | "Hello world" 中的 "Hello"  |
| `$`    | 匹配字符串的**结束**             | `world$`  | "Hello world" 中的 "world"  |

字符组

| 表达式   | 说明                           | 示例      | 匹配示例                |
| -------- | :----------------------------- | :-------- | :---------------------- |
| `[abc]`  | 匹配方括号中的**任意一个**字符 | `[aeiou]` | "a", "e", "i", "o", "u" |
| `[a-z]`  | 匹配小写字母                   | `[a-z]+`  | "hello", "test"         |
| `[A-Z]`  | 匹配大写字母                   | `[A-Z]`   | "A", "B", "C"           |
| `[0-9]`  | 匹配数字                       | `[0-9]`   | "0", "1", "2", ..., "9" |
| `[^abc]` | 匹配**不在**方括号中的任意字符 | `[^0-9]`  | "a", "B", "-" (非数字)  |

| 量词    | 说明              | 示例     | 匹配示例            |
| :------ | :---------------- | :------- | :------------------ |
| `{n}`   | 匹配**恰好 n 次** | `a{3}`   | "aaa"               |
| `{n,}`  | 匹配**至少 n 次** | `a{2,}`  | "aa", "aaa", "aaaa" |
| `{n,m}` | 匹配**n 到 m 次** | `a{2,4}` | "aa", "aaa", "aaaa" |

| 表达式    | 说明               | 示例      | 匹配示例 |
| :-------- | :----------------- | :-------- | :--------------------- |
| `(abc)`   | 分组，视为一个整体 | `(ab)+`   | "ab", "abab", "ababab" | 
| `a|b`     | 或操作             | `cat|dog` | "cat", "dog"           |
| `(?:abc)` | 非捕获分组         | `(?:ab)+` | 分组但不捕获           |

# mount

```sh
# 查看已经挂载的设备
sudo mount
# 挂载U盘
sudo mount /dev/sda1 /mnt/usb
# 卸载U盘
sudo umount /mnt/usb
# nfs网络文件系统挂载
sudo mount -t nfs 192.168.1.200:/home/nfs /mnt/nfs
```

# tar

```sh
tar -vxjf # 压缩
tar -vcjf # 解压
```

# gcc

`gcc`（GNU Compiler Collection）是一个功能强大的编译器套件，支持多种编程语言，包括 C、C++、Objective-C 等。对于 C 语言，它主要用来将 C 源代码文件编译成可执行程序。常见的 `gcc` 命令及选项有很多，下面是常用选项的详解。

### 基本用法：

```
gcc [选项] 文件 [输出选项]
```

### 常用选项详解：

1. **-o [文件名]**:
   指定输出文件名。默认情况下，GCC 会将编译结果命名为 `a.out`，如果需要自定义输出文件名，可以使用该选项。例如：

   ```
   gcc main.c -o main
   ```
   
   上面的命令会将 `main.c` 编译并生成名为 `main` 的可执行文件。
   
2. **-c**:
   只进行编译，生成目标文件（`.o` 文件），不进行链接。适用于将多个源文件分别编译为目标文件，然后再进行链接。例如：

   ```
   gcc -c file1.c
   gcc -c file2.c
   gcc file1.o file2.o -o program
   ```
   
3. **-g**:
   生成调试信息，用于调试程序。加上 `-g` 选项后，生成的可执行文件会包含符号信息，可以使用调试工具（如 `gdb`）进行调试。例如：

   ```
   gcc -g main.c -o main
   ```
   
4. **-Wall**:
   打开所有常见的警告信息，帮助开发者发现潜在的问题。虽然这些警告不会阻止程序编译，但它们可以帮助提高代码质量。例如：

   ```
   gcc -Wall main.c -o main
   ```
   
5. **-Werror**:
   将所有警告视为错误，任何警告都会导致编译失败。适用于严格的代码检查。例如：

   ```
   gcc -Werror main.c -o main
   ```
   
6. **-O, -O1, -O2, -O3**:
   用于优化代码，`-O` 表示默认优化，`-O1` 到 `-O3` 级别越高，优化力度越大，可能导致编译时间变长。`-O3` 进行最大程度的优化。例子：

   ```
   gcc -O2 main.c -o main
   ```
   
7. **-std=[标准]**:
   指定 C 语言标准，常用的有 `-std=c99`、`-std=c11` 等，确保代码符合特定的 C 标准。例如：

   ```
   gcc -std=c99 main.c -o main
   ```
   
8. **-I [路径]**:
   指定头文件的搜索路径。当头文件位于非标准路径下时，可以用 `-I` 指定。例如：

   ```
   gcc -I./include main.c -o main
   ```
   
9. **-L [路径]**:
   指定库文件的搜索路径。用于指定静态或动态库的位置。例如：

   ```
   gcc -L./lib -lmyLib main.c -o main
   ```
   
10. **-l [库名]**:
    链接指定的库文件。比如，如果有 `libmylib.so` 或 `libmylib.a` 库，使用 `-lmylib` 来链接。例如：

    ```
    gcc -L./lib -lmylib main.c -o main
    ```
    
11. **-shared**:
    用于生成共享库（即动态库）。此选项创建一个 `.so` 文件（共享对象）。例如：

    ```
    gcc -shared -fPIC -o libmylib.so mylib.c
    ```
    
12. **-fPIC**:
    生成位置无关代码（Position Independent Code），通常用于编写共享库。动态库必须用这个选项。

13. **-static**:
    强制使用静态链接库，而不是动态库。静态链接会将所有依赖的库打包到可执行文件中，生成的程序体积较大，但独立性更强。

14. **-E**:
    只进行预处理，不进行编译。这个选项用于查看预处理后的代码，包含宏展开、头文件引入等内容。例如：

    ```
    gcc -E main.c -o main.i
    ```
    
15. **-S**:
    生成汇编代码，而不进行后续编译成机器代码。用于调试或查看编译后的汇编指令。例如：

    ```
    gcc -S main.c -o main.s
    ```
    
16. **-v**:
    显示详细的编译过程信息。这个选项可以帮助调试编译问题，尤其是在编译链上出现问题时。

17. **-pthread**:
    用于编译多线程程序，启用 POSIX 线程库。

### 编译过程：

GCC 编译 C 程序分为四个主要步骤：

1. 预处理（Preprocessing）

   ```
#include
   ```
   
   ```
#define
   ```

    等预处理指令。
   
   - 通过 `gcc -E` 查看预处理结果。

2. 编译（Compilation）：将源代码转换为汇编代码。

   - 通过 `gcc -S` 查看汇编代码。

3. 汇编（Assembly）：将汇编代码转换为目标文件（机器码）。

   - 通过 `gcc -c` 生成目标文件。

4. 链接（Linking）：将目标文件和库文件链接为可执行文件。

   - 默认 `gcc` 会自动进行链接，如果不想链接，可以使用 `-c` 选项。

### 实例讲解：

假设有两个源文件 `main.c` 和 `utils.c`，要编译生成一个名为 `program` 的可执行文件，可以按照如下步骤进行：

```
# 编译 main.c 和 utils.c 为目标文件
gcc -c main.c
gcc -c utils.c

# 链接目标文件并生成可执行文件 program
gcc main.o utils.o -o program
```

这样，分步编译可以有效提高编译效率，尤其是在大型项目中，当只有部分文件修改时，无需重新编译整个项目。

通过以上 `gcc` 选项的理解和使用，可以更灵活地编译和调试 C 程序。

# ps & kill -9 PID

ps查看当前的进程

kill -9 PID 结束进程PID

./hello &在命令后面加符号**&**为程序在后台运行

# grep -rinl "xxx" /path

在路径中查找文件中的内容 xxx

`-r` 递归查找

`-i` 不分大小写

`-n` 显示行号

`-l` 只显示在哪个文件里

# 通过Linux转发实现开发板上网

解决的关键步骤。设置开发板的网关为与开发板处于同一网段的Linux服务器IP地址

1. 设置开发板的DNS服务器

   ```sh
   /etc/resolv.conf
   nameserver 114.114.114.114
   nameserver 192.168.137.1
   ```

2. 启用IP转发

   ```sh
   sudo sh -c 'echo 1 > /proc/sys/net/ipv4/ip_forward'
   
   # 永久启动IP转发
   # 在Linux服务器的/etc/sysctl.conf下添加
   net.ipv4.ip_forward = 1
   
   # 执行以下命令使得更改生效
   sudo sysctl -p
   ```

3. 配置防火墙

   ```sh
   # 设置 NAT 规则
   sudo iptables -t nat -A POSTROUTING -o ens38 -j MASQUERADE
   
   # 允许转发
   sudo iptables -A FORWARD -i ens33 -o ens38 -j ACCEPT
   sudo iptables -A FORWARD -i ens38 -o ens33 -m state --state ESTABLISHED,RELATED -j ACCEPT
   ```

4. 设置开发板网络

   与Linux服务器处于同一网段

   网关地址设置为Linux服务器地址

# man命令

Linux系统命令的详解

# 查看平台驱动的路径

**/sys/bus/platform/drivers  **

`/sys/bus/platform/devices/` 目录是 Linux 系统中的一个虚拟文件系统（sysfs）目录，用于管理和控制系统中的平台设备（platform devices）。平台设备是指那些直接连接到系统总线（如 SoC 内部的设备），而不是通过标准总线（如 PCI、USB 等）连接的设备。

# ifconfig

```sh
ifconfig eth0 up # 打开网卡
udhcpc -i eth0 	 #通过路由器分配 IP 地址
ifconfig eth0 192.168.1.251 netmask 255.255.255.0 //设置 IP 地址和子网掩码
route add default gw 192.168.1.1 //添加默认网关
```

正点原子开发板开机自启文件：**/etc/init.d/rcS**

# 文件系统

```sh
# Ubuntu 使用的哪个版本的文件系统呢？
df -T –h


# 为SD卡创建分区
sudo fdisk -l
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda           8:0    0 238.5G  0 disk 
└─sda1        8:1    0 238.5G  0 part /
mmcblk0     179:0    0  29.8G  0 disk 
├─mmcblk0p1 179:1    0   256M  0 part /boot
└─mmcblk0p2 179:2    0  29.5G  0 part /home
mmcblk1     179:8    0  14.9G  0 disk  # 这是新插入的 SD 卡（未分区）

sudo fdisk /dev/mmcblk1  # 替换为你的 SD 卡设备路径

# 创建第一个分区（如 512MB 的 FAT32 引导分区）
Command (m for help): n		# 输入 n 创建新分区
Partition type: p			# 主分区（primary）
Partition number: 1			# 分区号（默认 1）
First sector: 2048			# 起始扇区（默认 2048，直接回车）
Last sector: +512M  		# 结束扇区512MB
 
# 创建第二个分区（剩余空间）
Command (m for help): n
Partition type: p
Partition number: 2
First sector: (直接回车，使用默认值)
Last sector: (直接回车，使用剩余空间)

Command (m for help): t  # 修改分区类型
Partition number: 1     # 选择分区
Hex code (type L to list): 1  # 输入类型代码（1=FAT32，83=Linux，EF=EFI）

Command (m for help): w  # 写入并退出

sudo mkfs.vfat -F 32 /dev/mmcblk1p2  # 格式化第二个分区为 FAT32

sudo mkdir /mnt/sdcard  # 创建挂载点
sudo mount /dev/mmcblk1p1 /mnt/sdcard  # 挂载分区
```

# find

```sh
find /path -name "*.c"

find / -name "*.so" 2>/dev/null
# 用 2>/dev/null将错误信息“扔进黑洞”（/dev/null是一个特殊设备，会丢弃所有写入它的数据）。

-a        and 与操作
-o        or  或操作
-not      not 非操作
```



