# linux

## vim使用

#### 1. 三种运行模式：

1. 普通模式（normal）  
2. 插入模式（insert）  
3. 命令行模式（Command）

#### 2. 普通模式（normal）

1. G用于直接跳转到文件尾
2. ZZ用于存盘退出Vi
3. ZQ用于不存盘退出Vi
4. /和？用于查找字符串
5. n继续查找下一个
6. yy复制一行
7. p粘帖在下一行，P粘贴在前一行
8. dd删除一行文本
9. x删除光标所在的字符
10. u取消上一次编辑操作（undo）

#### 3. 插入模式（insert）

在 Normal 模式下输入插入命令 i、 a 、 o进入insert模式。用户输入的任何字符都被vim当做文件内容
保存起来，并将其显示在屏幕上。
在文本输入过程中，若想回到Normal模式下，按 Esc 键即可。

#### 4. 命令行模式（Command)

Normal 模式下，用户按冒号 :即可进入 Command 模式，此时 vim 会在显示窗口的最后一行 (屏幕的
最后一行) 显示一个 “:” 作为 Command 模式的提示符，等待输入命令。

1. :w 保存当前编辑文件，但并不退出
2. :w newfile 存为另外一个名为 “newfile” 的文件
3. :wq 用于存盘退出Vi
4. :q! 用于不存盘退出Vi
5. :q用于直接退出Vi （未做修改）