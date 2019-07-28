leemeans translating
文档之七宗罪
7 deadly sins of documentation
======
![](https://opensource.com/sites/default/files/styles/image-full-size/public/lead-images/osdc-lead-cat-writing-king-typewriter-doc.png?itok=afaEoOqc)
文档在业务中似乎是一个永恒的问题。所有人都认为应该有文档，但是很少有人相信自己从属的组织拥有所有自己需要的文档。有效的文档实践可以可以提升事件响应，加速新员工培训并减少技术债务，但是不良的文档实践可能比没有文档还要糟糕。
Documentation seems to be a perennial problem in operations. Everyone agrees that it's important to have, but few believe that their organizations have all the documentation they need. Effective documentation practices can improve incident response, speed up onboarding, and help reduce technical debt--but poor documentation practices can be worse than having no documentation at all.

### 七宗罪The 7 sins

下面的场景听起来熟悉吗？
Do any of the following scenarios sound familiar?

  * 你有一个维基，一个谷歌文档仓库，一个Github文档仓库，一大堆放在家目录的文本文档以及用电子邮件写成的问题笔记。
You've got a wiki. And a Google Doc repository. And GitHub docs. And a bunch of text files in your home directory. And notes about problems in email.
  * 你有一个解释了某服务各方面的文档，并且你十分确定用于修复此次事件的所有信息都在此文档中…的某个地方。
You have a doc that explains everything about a service, and you're sure that the information you need to fix this incident in there ... somewhere.
  * 你有一个500行的Puppet清单用于处理此服务…但它没有注释。或者其注释引用了来自两个版本前的票务系统的票。
You've got a 500-line Puppet manifest to handle this service ... with no comments. Or comments that reference tickets from two ticketing systems ago.
  * 你有描述了所有基础部件的很多归档演讲稿，但是你没有时间查看它们的年纪因此你不清楚它们更新的状态。
You have a bunch of archived presentations that discuss all sorts of infrastructure components, but you're not sure how up-to-date they are because you haven't had time to watch them in ages.
  * 你带了一个新人到组里，但是他花了一个月来询问大量专业术语的含义。
You bring someone new into the team and they spend a month asking what various pieces of jargon mean.
  * 你查看维基时发现了三个不同的文档描述某一服务的工作方式，其中两个甚至是完全冲突的，并且它们在过去的一年中都没有都没有更新过。
You search your wiki and find three separate docs on how this service works, two of which contradict each other entirely, and none of which have been updated in the past year.



以下是你可能犯过至少一宗罪的所有迹象：
These are all signs you may have committed at least one of the deadly sins of documentation:

1\. 仓库激增。
Repository overload.
2\. 隐藏导语。
Burying the lede.
3\. 忽略注释。
Comment neglect.
4\. 添加视频。
Video addiction.
5\. 滥用术语。
Jargon overuse.
6\. 过度文档。
Documentation overgrowth.

而一旦你犯过一宗罪，你很有可能也知道下面这一个：
But if you've committed any of those sins, chances are you know this one, too:

7\. 上述的一个或者多个是正确的，但是，每个人都说他们没有时间写文档。
One or more of the above is true, but everyone says they don't have time to work on documentation.

所有罪中最糟糕的是认为文档是一项“额外”的工作。所有其他问题基本上都是这个错误造成的文档不是额外的工作——它是所有工程必要的一部分，如果没有这样对待文档的话，基本上是不可能把文档写好的。没有一个熟练于编写、审阅和发布文档的开发者是不可能获得良好的代码的，当我们完成其他工作以后进行的文档也是如此。当你觉得你的文档写得不够充分，问问你自己下面的问题：
The worst sin of all is thinking that documentation is "extra" work. Those other problems are almost always a result of this mistake. Documentation isn't extra work--it's a necessary part of every project, and if it isn't treated that way, it will be nearly impossible to do well. You wouldn't expect to get good code out of developers without a coherent process for writing, reviewing, and publishing code, and yet we often treat documentation like an afterthought, something that we assume will happen while we get our other work done. If you think your documentation is inadequate, ask yourself these questions:

  * 你的工程将产出文档包含为一项量化指标吗？
Do your projects include producing documentation as a measurable goal?
  * 你有一个正式的文档审阅过程吗？
Do you have a formal review process for documentation?
  * 文档是否被视作团队中的高级成员的任务？
Is documentation considered a task for senior members of the team?



所有罪中最糟糕的就是把文档当做“额外”的工作。
The worst sin of all is thinking that documentation is "extra" work.

这些问题可以告诉你自己是否把文档当做额外的工作。如果没有给予时间编写文档，没有过程保证产出的文档是有用的或者文档任务被赋予了团队中无力抓住需覆盖的重点的成员，那么很难产出高质量的文档。
Those three questions can tell you a lot about whether you treat documentation as extra work or not. If people aren't given the time to write documentation, if there's no process to make sure the documentation that's produced is actually useful, or if documentation is foisted on those members of your team with the weakest grasp on the subjects being covered, it will be difficult to produce anything of decent quality.

Those three questions can tell you a lot about whether you treat documentation as extra work or not. If people aren't given the time to write documentation, if there's no process to make sure the documentation that's produced is actually useful, or if documentation is foisted on those members of your team with the weakest grasp on the subjects being covered, it will be difficult to produce anything of decent quality.

这种经常不屑一顾的态度在行业中是很常见的。根据[GitHub 2017 Open Source Survey][1]，大多数开源工程的最大问题是不完整的或者令人困惑的文档。而又有多少这些工程招募技术写者来提高这些呢？任务中的我们又有多少会带一个技术写者进入团队帮忙改进文档呢？
This often-dismissive attitude is pervasive in the industry. According to the [GitHub 2017 Open Source Survey][1], the number-one problem with most open source projects is incomplete or confusing documentation. But how many of those projects solicit technical writers to help improve that? How many of us in operations have a technical writer we bring in to help write or improve our documentation?

### Practice makes (closer to) perfect

This isn't to say that only a technical writer can produce good documentation, but writing and editing are skills like any other: We'll only get better at it if we work at it, and too few of us do. What are the concrete steps we can take to make it a real priority, as opposed to a nice-to-have?

For a start, make good documentation a value that your organization champions. Just as reliability needs champions to get prioritized, documentation needs the same thing. Project plans and sprints should include delivering new documentation or updating old documentation, and allocate time for doing so. Make sure people understand that writing good documentation is just as important to their career development as writing good code.

Additionally, make it easy to keep documentation up to date and for people to find the documentation they need. In this way, you can help perpetuate the virtuous circle of documentation: High-quality docs help people realize the value of documentation and provide examples to follow when they write their own, which in turn will encourage them to create their own.

To do this, have as few repositories as possible; one or two is optimal (you might want your runbooks to be in Google Docs so they are accessible if the company wiki is down, for instance). If you have more, make sure everyone knows what each repository is for; if Google Docs is for runbooks, verify that all runbooks are there and nowhere else, and that everyone knows that. Ensure that your repositories are searchable and keep a change history, and to improve discoverability, consider adding portals that have frequently used or especially important docs surfaced for easy access. Do not depend on email, chat logs, or tickets as primary sources of documentation.

Ask new and junior members of your team to review both your code and your documentation. If they don't understand what's going on in your code, or why you made the choices you did, it probably needs to be rewritten and/or commented better. If your docs aren't easy to understand without going down a rabbit hole, they probably need to be revised. Technical documentation should include concrete examples of how processes and behaviors work to help people create mental models. You may find the tips in this article helpful for improving your documentation writing: [10 tips for making your documentation crystal clear][2].

When you're writing those docs, especially when it comes to runbooks, use the [inverted pyramid format][3]: The most commonly needed or most important information should be as close to the top of the page as possible. Don't combine runbook-style documents and longer-form technical reference; instead, link the two and keep them separate so that runbooks remain streamlined (but can easily be discovered from the reference, and vice versa).

Using these steps in your documentation can change it from being a nice-to-have (or worse, a burden) into a force multiplier for your operations team. Good docs improve inclusiveness and knowledge transfer, helping your more inexperienced team members solve problems independently, freeing your more senior team members to work on new projects instead of firefighting or training new people. Better yet, well-written, high-quality documentation enables you and your team members to enjoy a weekend off or go on vacation without being on the hook if problems come up.

Learn more in Chastity Blackwell's talk, [The 7 Deadly Sins of Documentation][4], at [LISA17][5], which will be held October 29-November 3 in San Francisco, California.

### About The Author

Chastity Blackwell;Chastity Blackwell Is A Site Reliability Engineer At Yelp;With More Than Years Of Experience In Operations.

--------------------------------------------------------------------------------

via: https://opensource.com/article/17/10/7-deadly-sins-documentation

作者：[Chastity Blackwell][a]
译者：[译者ID](https://github.com/译者ID)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]:https://opensource.com/users/cblkwell
[1]:http://opensourcesurvey.org/2017/
[2]:https://opensource.com/life/16/11/tips-for-clear-documentation
[3]:https://en.wikipedia.org/wiki/Inverted_pyramid_(journalism)
[4]:https://www.usenix.org/conference/lisa17/conference-program/presentation/blackwell
[5]:https://www.usenix.org/conference/lisa17
