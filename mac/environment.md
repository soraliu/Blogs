# Mac配置环境变量的地方
1./etc/profile   （建议不修改这个文件 ）

全局（公有）配置，不管是哪个用户，登录时都会读取该文件。
 
2./etc/bashrc    （一般在这个文件中添加系统级环境变量）

全局（公有）配置，bash shell执行时，不管是何种方式，都会读取此文件。
 
3.~/.bash_profile  （一般在这个文件中添加用户级环境变量）

每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件仅仅执行一次!

# 操作示例：
通过编辑 启动文件 来改PATH，

`vim /etc/profile`

在文档最后，添加:

`export PATH="/opt/STM/STLinux-2.3/devkit/sh4/bin:$PATH"`

保存，退出。
 
想立即生效请运行：
`source /etc/profile`
`source .bash_profile（这是文件名）`

不报错则成功。 
