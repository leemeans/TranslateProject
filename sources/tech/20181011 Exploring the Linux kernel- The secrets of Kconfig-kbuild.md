translating by leemeans
Exploring the Linux kernel: The secrets of Kconfig/kbuild
探索Linux内核：Kconfig/kbuild的奥秘
======
Dive into understanding how the Linux config/build system works.
深入理解Linuxconfig/build系统如何工作。

![](https://opensource.com/sites/default/files/styles/image-full-size/public/lead-images/compass_map_explore_adventure.jpg?itok=ecCoVTrZ)

The Linux kernel config/build system, also known as Kconfig/kbuild, has been around for a long time, ever since the Linux kernel code migrated to Git. As supporting infrastructure, however, it is seldom in the spotlight; even kernel developers who use it in their daily work never really think about it.
自从内核代码迁移到Git,被称为Kconfig/kbuild的Linux内核config/build系统已经出现了很久了。但是作为支持组件，它极少成为(人们)的焦点，即使是在日常工作中使用此系统的内核开发者也没有真正理解它。

To explore how the Linux kernel is compiled, this article will dive into the Kconfig/kbuild internal process, explain how the .config file and the vmlinux/bzImage files are produced, and introduce a smart trick for dependency tracking.
为了探索Linux内核是如何被编译的，此文将会深入Kconfig/kbuild内部流程，解释.config文件和vmlinux/bzImage是如何产生的，并介绍一个用于依赖跟踪的聪明的小技巧。

### Kconfig

The first step in building a kernel is always configuration. Kconfig helps make the Linux kernel highly modular and customizable. Kconfig offers the user many config targets:
构建一个内核的第一步永远是配置。Kconfig使得Linux内核高度模块化和可定制话。Kconfig为用户提供了多个配置目标:

|  |  |
|---|---|
| config         | Update current config utilizing a line-oriented program通过命令行程序更新当前config选项                         |
| nconfig        | Update current config utilizing a ncurses menu-based program通过基于ncurses菜单程序更新当前config选项                    |
| menuconfig     | Update current config utilizing a menu-based program通过基于菜单程序更新当前config选项                            |
| xconfig        | Update current config utilizing a Qt-based frontend通过基于Qt前端更新当前config选项                             |
| gconfig        | Update current config utilizing a GTK+ based frontend通过基于GTK+前端更新当前config选项                           |
| oldconfig      | Update current config utilizing a provided .config as base使用提供的.config文件作为基础更新当前config选项                      |
| localmodconfig | Update current config disabling modules not loaded禁用未加载的模块并更新当前config                              |
| localyesconfig | Update current config converting local mods to core将本地模块编译进内核并更新当前config                             |
| defconfig      | New config with default from Arch-supplied defconfig根据架构提供的defconfig创建新的config                            |
| savedefconfig  | Save current config as ./defconfig (minimal config)将当前的config保存为./defconfig最小config                             |
| allnoconfig    | New config where all options are answered with 'no'所有选项设置为no的新config                             |
| allyesconfig   | New config where all options are accepted with 'yes'所有选项设置为yes的新config                            |
| allmodconfig   | New config selecting modules when possible选中可能的模块的新config                                      |
| alldefconfig   | New config with all symbols set to default所有符号设为默认的新config                                      |
| randconfig     | New config with a random answer to all options对所有选项设置随机值的工作新config                                  |
| listnewconfig  | List new options列出新选项                                                                |
| olddefconfig   | Same as oldconfig but sets new symbols to their default value without prompting和oldconfig相同但是会自动设置其新符号的默认值 |
| kvmconfig      | Enable additional options for KVM guest kernel support打开额外的KVM guest内核支持                          |
| xenconfig      | Enable additional options for xen dom0 and guest kernel support启用xen dom0额外选项和guest支持                 |
| tinyconfig     | Configure the tiniest possible kernel配置最小内核                                           |
| | |

I think **menuconfig** is the most popular of these targets. The targets are processed by different host programs, which are provided by the kernel and built during kernel building. Some targets have a GUI (for the user's convenience) while most don't. Kconfig-related tools and source code reside mainly under **scripts/kconfig/** in the kernel source. As we can see from **scripts/kconfig/Makefile** , there are several host programs, including **conf** , **mconf** , and **nconf**. Except for **conf** , each of them is responsible for one of the GUI-based config targets, so, **conf** deals with most of them.

我觉得在这些目标选项中**menuconfig**是最流行的。目标由内核在内核构建过程中编译的不同的宿主程序进行处理。虽然大多数没有可还是有些目标提供了GUI(根据用户自己的喜好)。Kconfig相关的工具和对应源代码主要放在内核源下的 **scripts/kconfig/** 。我们可以从 **scripts/kconfig/Makefile** 看到有很多宿主程序，包括 **conf** , **mconf** , 和 **nconf** 等。除了 **conf** 之外，这些程序每个都对应着一个基于GUI的config目标，因此， **conf** 处理了大部分的工作。

Logically, Kconfig's infrastructure has two parts: one implements a [new language][1] to define the configuration items (see the Kconfig files under the kernel source), and the other parses the Kconfig language and deals with configuration actions.

从逻辑上讲，Kconfig的基础构件有两个部分: 一部分实现一个 [新语言][1] 来定义配置条目(参见内核源下的Kconfig文件)，另一部分解析Kconfig语言并处理配置行为。

Most of the config targets have roughly the same internal process (shown below):
大多数config目标拥有大致相同的内部程序(如下图):

![](https://opensource.com/sites/default/files/uploads/kconfig_process.png)

Note that all configuration items have a default value.

注意所有的配置条目都有一个默认值。

The first step reads the Kconfig file under source root to construct an initial configuration database; then it updates the initial database by reading an existing configuration file according to this priority:

第一步读取源目录根路径的Kconfig文件来构建一个初始配置数据库;然后按照下列优先级读取一个已存在的配置文件来更新初始化的数据库。

> .config
>  /lib/modules/$(shell,uname -r)/.config
>  /etc/kernel-config
>  /boot/config-$(shell,uname -r)
>  ARCH_DEFCONFIG
>  arch/$(ARCH)/defconfig

If you are doing GUI-based configuration via **menuconfig** or command-line-based configuration via **oldconfig** , the database is updated according to your customization. Finally, the configuration database is dumped into the .config file.

如果你正通过 **menuconfig** 进行基于GUI的配置或者通过**oldconfig**进行基于命令行的配置工作，配置数据库根据将会根据你的配置进行更新。最终，配置数据库将会被导出到.config文件中。

But the .config file is not the final fodder for kernel building; this is why the **syncconfig** target exists. **syncconfig** used to be a config target called **silentoldconfig** , but it doesn't do what the old name says, so it was renamed. Also, because it is for internal use (not for users), it was dropped from the list.

但是.config文件不是内核构建的最终素材;所以有**syncconfig**目标的存在。**syncconfig**曾经是被叫做**silentoldconfig**的目标，但是它所做的任务并非其名字所表示的那样，因此它被改了名字。同样地，因为它是内部使用的文件(不是给用户的)，因此它被从列表中丢弃了。

Here is an illustration of what **syncconfig** does:

下面展示了**syncconfig**所做的事情:

![](https://opensource.com/sites/default/files/uploads/syncconfig.png)

**syncconfig** takes .config as input and outputs many other files, which fall into three categories:

**syncconfig**使用.config作为输入并输出很多其他文件，这些文件可以分为以下三个目录:

  * **auto.conf & tristate.conf** are used for makefile text processing. For example, you may see statements like this in a component's makefile:被用于makefile文本处理。例如你可能会在makefile的一个部分中看到类似下面的语句:

```
     obj-$(CONFIG_GENERIC_CALIBRATE_DELAY) += calibrate.o
```

  * **autoconf.h** is used in C-language source files.被用在C语言源文件中。

  * Empty header files under **include/config/** are used for configuration-dependency tracking during kbuild, which is explained below.在**include/config/**下的空白头文件在kbuild过程中配置依赖跟踪的，关于这个过程，后文有解释。




After configuration, we will know which files and code pieces are not compiled.在配置之后，我们将会知道哪些文件和代码片是没有被编译的。

### kbuild

Component-wise building, called _recursive make_ , is a common way for GNU `make` to manage a large project. Kbuild is a good example of recursive make. By dividing source files into different modules/components, each component is managed by its own makefile. When you start building, a top makefile invokes each component's makefile in the proper order, builds the components, and collects them into the final executive.

分量方式构建，又名_递归make_，是一种GNU `make` 管理大型工程的普遍方法。kbuild就是一个递归 make 的好例子。通过将源文件分为不同的模块/部分，每个部分都由其自身的makefile管理。

Kbuild refers to different kinds of makefiles:

  * **Makefile** is the top makefile located in source root.
  * **.config** is the kernel configuration file.
  * **arch/$(ARCH)/Makefile** is the arch makefile, which is the supplement to the top makefile.
  * **scripts/Makefile.*** describes common rules for all kbuild makefiles.
  * Finally, there are about 500 **kbuild makefiles**.



The top makefile includes the arch makefile, reads the .config file, descends into subdirectories, invokes **make** on each component's makefile with the help of routines defined in **scripts/Makefile.*** , builds up each intermediate object, and links all the intermediate objects into vmlinux. Kernel document [Documentation/kbuild/makefiles.txt][2] describes all aspects of these makefiles.

As an example, let's look at how vmlinux is produced on x86-64:

![vmlinux overview][4]

(The illustration is based on Richard Y. Steven's [blog][5]. It was updated and is used with the author's permission.)

All the **.o** files that go into vmlinux first go into their own **built-in.a** , which is indicated via variables **KBUILD_VMLINUX_INIT** , **KBUILD_VMLINUX_MAIN** , **KBUILD_VMLINUX_LIBS** , then are collected into the vmlinux file.

Take a look at how recursive make is implemented in the Linux kernel, with the help of simplified makefile code:

```
    # In top Makefile
    vmlinux: scripts/link-vmlinux.sh $(vmlinux-deps)
                    +$(call if_changed,link-vmlinux)

    # Variable assignments
    vmlinux-deps := $(KBUILD_LDS) $(KBUILD_VMLINUX_INIT) $(KBUILD_VMLINUX_MAIN) $(KBUILD_VMLINUX_LIBS)

    export KBUILD_VMLINUX_INIT := $(head-y) $(init-y)
    export KBUILD_VMLINUX_MAIN := $(core-y) $(libs-y2) $(drivers-y) $(net-y) $(virt-y)
    export KBUILD_VMLINUX_LIBS := $(libs-y1)
    export KBUILD_LDS          := arch/$(SRCARCH)/kernel/vmlinux.lds

    init-y          := init/
    drivers-y       := drivers/ sound/ firmware/
    net-y           := net/
    libs-y          := lib/
    core-y          := usr/
    virt-y          := virt/

    # Transform to corresponding built-in.a
    init-y          := $(patsubst %/, %/built-in.a, $(init-y))
    core-y          := $(patsubst %/, %/built-in.a, $(core-y))
    drivers-y       := $(patsubst %/, %/built-in.a, $(drivers-y))
    net-y           := $(patsubst %/, %/built-in.a, $(net-y))
    libs-y1         := $(patsubst %/, %/lib.a, $(libs-y))
    libs-y2         := $(patsubst %/, %/built-in.a, $(filter-out %.a, $(libs-y)))
    virt-y          := $(patsubst %/, %/built-in.a, $(virt-y))

    # Setup the dependency. vmlinux-deps are all intermediate objects, vmlinux-dirs
    # are phony targets, so every time comes to this rule, the recipe of vmlinux-dirs
    # will be executed. Refer "4.6 Phony Targets" of `info make`
    $(sort $(vmlinux-deps)): $(vmlinux-dirs) ;

    # Variable vmlinux-dirs is the directory part of each built-in.a
    vmlinux-dirs    := $(patsubst %/,%,$(filter %/, $(init-y) $(init-m) \
                         $(core-y) $(core-m) $(drivers-y) $(drivers-m) \
                         $(net-y) $(net-m) $(libs-y) $(libs-m) $(virt-y)))

    # The entry of recursive make
    $(vmlinux-dirs):
                    $(Q)$(MAKE) $(build)=$@ need-builtin=1
```

The recursive make recipe is expanded, for example:

```
    make -f scripts/Makefile.build obj=init need-builtin=1
```

This means **make** will go into **scripts/Makefile.build** to continue the work of building each **built-in.a**. With the help of **scripts/link-vmlinux.sh** , the vmlinux file is finally under source root.

#### Understanding vmlinux vs. bzImage

Many Linux kernel developers may not be clear about the relationship between vmlinux and bzImage. For example, here is their relationship in x86-64:

![](https://opensource.com/sites/default/files/uploads/vmlinux-bzimage.png)

The source root vmlinux is stripped, compressed, put into **piggy.S** , then linked with other peer objects into **arch/x86/boot/compressed/vmlinux**. Meanwhile, a file called setup.bin is produced under **arch/x86/boot**. There may be an optional third file that has relocation info, depending on the configuration of **CONFIG_X86_NEED_RELOCS**.

A host program called **build** , provided by the kernel, builds these two (or three) parts into the final bzImage file.

#### Dependency tracking

Kbuild tracks three kinds of dependencies:

  1. All prerequisite files (both * **.c** and * **.h** )
  2. **CONFIG_** options used in all prerequisite files
  3. Command-line dependencies used to compile the target



The first one is easy to understand, but what about the second and third? Kernel developers often see code pieces like this:

```
    #ifdef CONFIG_SMP
    __boot_cpu_id = cpu;
    #endif
```

When **CONFIG_SMP** changes, this piece of code should be recompiled. The command line for compiling a source file also matters, because different command lines may result in different object files.

When a **.c** file uses a header file via a **#include** directive, you need write a rule like this:

```
    main.o: defs.h
            recipe...
```

When managing a large project, you need a lot of these kinds of rules; writing them all would be tedious and boring. Fortunately, most modern C compilers can write these rules for you by looking at the **#include** lines in the source file. For the GNU Compiler Collection (GCC), it is just a matter of adding a command-line parameter: **-MD depfile**

```
    # In scripts/Makefile.lib
    c_flags        = -Wp,-MD,$(depfile) $(NOSTDINC_FLAGS) $(LINUXINCLUDE)     \
                     -include $(srctree)/include/linux/compiler_types.h       \
                     $(__c_flags) $(modkern_cflags)                           \
                     $(basename_flags) $(modname_flags)
```

This would generate a **.d** file with content like:

```
    init_task.o: init/init_task.c include/linux/kconfig.h \
     include/generated/autoconf.h include/linux/init_task.h \
     include/linux/rcupdate.h include/linux/types.h \
     ...
```

Then the host program **[fixdep][6]** takes care of the other two dependencies by taking the **depfile** and command line as input, then outputting a **. <target>.cmd** file in makefile syntax, which records the command line and all the prerequisites (including the configuration) for a target. It looks like this:

```
    # The command line used to compile the target
    cmd_init/init_task.o := gcc -Wp,-MD,init/.init_task.o.d  -nostdinc ...
    ...
    # The dependency files
    deps_init/init_task.o := \
    $(wildcard include/config/posix/timers.h) \
    $(wildcard include/config/arch/task/struct/on/stack.h) \
    $(wildcard include/config/thread/info/in/task.h) \
    ...
      include/uapi/linux/types.h \
      arch/x86/include/uapi/asm/types.h \
      include/uapi/asm-generic/types.h \
      ...
```

A **. <target>.cmd** file will be included during recursive make, providing all the dependency info and helping to decide whether to rebuild a target or not.

The secret behind this is that **fixdep** will parse the **depfile** ( **.d** file), then parse all the dependency files inside, search the text for all the **CONFIG_** strings, convert them to the corresponding empty header file, and add them to the target's prerequisites. Every time the configuration changes, the corresponding empty header file will be updated, too, so kbuild can detect that change and rebuild the target that depends on it. Because the command line is also recorded, it is easy to compare the last and current compiling parameters.

### Looking ahead

Kconfig/kbuild remained the same for a long time until the new maintainer, Masahiro Yamada, joined in early 2017, and now kbuild is under active development again. Don't be surprised if you soon see something different from what's in this article.

--------------------------------------------------------------------------------

via: https://opensource.com/article/18/10/kbuild-and-kconfig

作者：[Cao Jin][a]
选题：[lujun9972][b]
译者：[译者ID](https://github.com/译者ID)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]: https://opensource.com/users/pinocchio
[b]: https://github.com/lujun9972
[1]: https://github.com/torvalds/linux/blob/master/Documentation/kbuild/kconfig-language.txt
[2]: https://www.mjmwired.net/kernel/Documentation/kbuild/makefiles.txt
[3]: https://opensource.com/file/411516
[4]: https://opensource.com/sites/default/files/uploads/vmlinux_generation_process.png (vmlinux overview)
[5]: https://blog.csdn.net/richardysteven/article/details/52502734
[6]: https://github.com/torvalds/linux/blob/master/scripts/basic/fixdep.c
