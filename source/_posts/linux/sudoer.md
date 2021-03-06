---
title: "sudoer"
date: 2016-05-26 09:30
---

### User privilege specification

    root ALL=(ALL) ALL

#### Members of the admin group may gain root privileges

    %admin ALL=(ALL) ALL

#### 下面对以上配置做简要说明：

第一项配置的作用，是允许root用户使用sudo命令变成系统中任何其它类型的用户。第二个配置规定，管理组中的所有成员都能以root的身份执行所有命令。因此，在默认安装的Ubuntu系统中，要想作为root身份来执行命令的话，只要在sudo后面跟上欲执行的命令即可。

我们用一个实例来详细解释/etc/sudoers文件的配置语法，请看下面的例子：

    jorge ALL=(root) /usr/bin/find, /bin/rm

上面的第一栏规定它的适用对象：用户或组，就本例来说，它是用户jorge。此外，因为系统中的组和用户可以重名，要想指定该规则的适用对象是组而非用户的话，组对象的名称一定要用百分号%开头。

第二栏指定该规则的适用主机。当我们在多个系统之间部署sudo环境时，这一栏格外有用，这里的ALL代表所有主机。但是，对于桌面系统或不想将sudo部署到多个系统的情况，这一栏就换成相应的主机名。

第三栏的值放在括号内，指出第一栏规定的用户能够以何种身份来执行命令。本例中该值设为root，这意味着用户jorge能够以root用户的身份来运行后面列出的命令。该值也可以设成通配符ALL，jorge便能作为系统中的任何用户来执行列出的命令了。

最后一栏（即/usr/bin/find,/bin/rm）是使用逗号分开的命令表，这些命令能被第一栏规定的用户以第三栏指出的身份来运行它们。本例中，该配置允许jorge作为超级用户运行/usr/bin/find和 /bin/rm这两个命令。需要指出的是，这里列出的命令一定要使用绝对路径。

进一步：
我们可以利用这些规则为系统创建具体的角色。例如，要让一个组负责帐户管理，你一方面不想让这些用户具备完全的root访问权限，另一方面还得让他们具有增加和删除用户的权利，那么我们可以在系统上创建一个名为accounts的组，然后把那些用户添加到这个组里。

之后，再使用visudo为/etc/sudoers添加下列内容：

    %accounts ALL=(root) /usr/sbin/useradd,/usr/sbin/userdel, /usr/sbin/usermod

现在好了，accounts组中的任何成员都能运行useradd、userdel和usermod命令了。如果过一段时间后，您发现该角色还需要其他工具，只要在该表的尾部将其添上就行了。这样真是方便极了！

需要注意的是，当我们为用户定义可以运行的命令时，必须使用完整的命令路径。这样做是完全出于安全的考虑，如果我们给出的命令只是简单的userad而非/usr/sbin/useradd，那么用户有可能创建一个他自己的脚本，也叫做useradd，然后放在它的本地路径中，如此一来他就能够通过这个名为useradd的本地脚本，作为root来执行任何他想要的命令了。这是相当危险的！

sudo命令的另一个便捷的功能，是它能够指出哪些命令在执行时不需要输入密码。

这很有用，尤其是在非交互式脚本中以超级用户的身份来运行某些命令的时候。例如，想要让用户作为超级用户不必输入密码就能执行kill命令，以便用户能立刻杀死一个失控的进程。为此，在命令行前边加上NOPASSWD:属性即可。

例如，可以在/etc/sudoers文件中加上下面一行，从而让jorge获得这种权力：

    jorge ALL=(root)NOPASSWD: /bin/kill, /usr/bin/killall


[1](http://blog.csdn.net/sin90lzc/article/details/8628026)

[2](http://baike.baidu.com/link?url=SMSJmsrS5OoJ3wlCqonc-93fZrhrK44k7MT7lx_zT6ifi0QSsZ9XkxThspWy3PJOOQZB1GIAbF2CVQfB3Vmc4q)

[3](http://www.kubihai.com/html/582261.html)
