[#]: collector: (lujun9972)
[#]: translator: (vizv)
[#]: reviewer: ( )
[#]: publisher: ( )
[#]: url: ( )
[#]: subject: (Sysadmin vs SRE: What's the difference?)
[#]: via: (https://opensource.com/article/19/7/sysadmins-vs-sres)
[#]: author: (Vince Power https://opensource.com/users/vincepower/users/craig5/users/dawnparzych/users/penglish)

系统管理员与网站可靠性工程师对比：区别在那儿？
======
系统管理员和网站可靠性工程师（SRE，下同）对于任何组织来讲都很重要。本篇将介绍下两者的不同之处。
![People work on a computer server with devices][1]

在 IT 行业，成为多面手或是专家的争议一直存在。99% 的传统系统管理员都被归到了多面手这类。[网站可靠性工程师][2]的角色则更加专精，并且其需求在如 Google 般有着一定规模的公司中不断增加。但总的来说这两者对于跑着应用的基础设施有着同样的目标：为应用的消费者提供良好的体验。然而两者的出发点却截然不同。

### 系统管理员：中立善良的化身

系统管理员一般都是从基础的桌面或网络支持成长过来的，并一路习得大多数系统管理员都会掌握的广泛的技能。此时这些系统管理员会对他们所负责的系统和应用了如指掌。他们会知道一号服务器上的应用每隔一个星期二就需要重启一次，或是九号服务器周三会静默的崩溃。他们会对服务器的监视作出微调以忽略无关紧要的信息，尽管那是个每月第三个周日都会显示的被标记为<ruby>致命<rt>fatal<rt></ruby>的错误信息。

总的来讲，系统管理员了解如何照料那些跑着你核心业务的服务器。这些系统管理员已经成长到开始使用自动化工具去处理所有归他们管的服务器上的例行任务。他们虽然喜欢使用模板、<ruby>黄金镜像<rt>golden images</rt></ruby>、以及标准，但同时也有着足够的灵活度去修改一个服务器上的参数以解决错误，并注释为什么那个服务器的配置与众不同。

尽管系统管理员很伟大，但他们也有着一些怪癖。其中一项就是没有他们神圣的授权你永远也获取不了系统的 root 访问权限，另一项则是任何不是他们的主意都在文档被记录为应用的提供者的要求，并仍然需要再次核对。

他们所管理的服务器是他们的地盘，没有人可以随意干涉。

### SRE：灭霸将为之自豪

与成为系统管理员的道路相反，从开发背景和从系统管理员背景成长为 SRE 的可能性相近。SRE 的职位出现的时长与应用开发环境的生命周期相近。

随着一个组织的发展而引入的类似于[持续集成][4]和[持续发布][5] (CI/CD) 的 [DevOps][3] 概念，通常会带来如何让这些<ruby>不可变<rt>immutable</rt></ruby>的应用部署到多个环境并随着业务需求进行扩展的技能空缺。着将是 SRE 的舞台。的确，一个系统管理员可以学习额外的工具，但大体上成为一个全职的职位更容易跟的上进度。在这里成为一个专家更说的通。

SRE 使用如<ruby>[代码即基础设施][6]<rt>infrastructure-as-code</rt></ruby>的概念去制作模板，然后调用它们来部署用以运行应用的环境，并以使用一键将每个应用和它们的环境完整重现作为目标。因此测试环境中一号服务器里的一号应用的二进制文件与生产环境中十五号服务器的完全一致，仅环境相关的变量如密码和数据库链接字串有所不同。

SRE 同时也在配置发生改变时完全摧毁一个环境并重新构建它。对于任何系统他们不带一点感情。每个系统只是个被打了标记和安排了生命周期的数字，甚至例行的服务器补丁也要重新部署整个<ruby>应用栈<rt>application stack</rt></ruby>

### 总结

对于一些情况，尤其是运维一些大型的基于 DevOps 的环境时，一个 SRE 所能提供的用于处理各种规模业务的专业技能当然更具优势。但每次他们在运气不好走入死胡同时都会去寻求友人系统管理员，或是 [(BOFH)][7] 那身经百战的故障排除技能和那些系统管理员用于给组织提供价值的丰富经验的帮助。

--------------------------------------------------------------------------------

via: https://opensource.com/article/19/7/sysadmins-vs-sres

作者：[Vince Power][a]
选题：[lujun9972][b]
译者：[vizv](https://github.com/vizv)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]: https://opensource.com/users/vincepower/users/craig5/users/dawnparzych/users/penglish
[b]: https://github.com/lujun9972
[1]: https://opensource.com/sites/default/files/styles/image-full-size/public/lead-images/rh_003499_01_linux11x_cc.png?itok=XMDOouJR (People work on a computer server with devices)
[2]: https://en.wikipedia.org/wiki/Site_Reliability_Engineering
[3]: https://opensource.com/resources/devops
[4]: https://en.wikipedia.org/wiki/Continuous_integration
[5]: https://en.wikipedia.org/wiki/Continuous_delivery
[6]: https://en.wikipedia.org/wiki/Infrastructure_as_code
[7]: http://www.bofharchive.com/BOFH.html
