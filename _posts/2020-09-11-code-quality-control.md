---
layout:     post
title:      "谈谈工程代码质量控制的事"
subtitle:   ""
date:       2020-09-11
author:     "qingtian"
catalog:    true
tags:
    - 代码质量
    - 单元测试
    - 代码review
    - 个人思考
---

# 1. 前言

从2011年开始算起，我至今也已工作9个多年头了。工作中或多或少总会看到各式各样的代码习惯，有些人关注模式与结构，有些人关注性能与实现，有些人关注“可运行”，还有一些人只是在“堆代码”实现需求罢了。

对于产品来说，工程代码质量是一个若隐若现的存在，好的时候感觉不到他的存在。而不好的时候，我们往往可以明显的觉察到“哪里不对劲”。

因此从保证产品价值层面来说，如何有效进行工程代码质量控制就非常值得我们深度思考一下了。下面我就来讲讲，我对工程代码质量控制的理解与实践经验，希望能给到大家一些借鉴意义。


# 2. 质量控制的3个方面

对于质量控制，我认为主要存在3个方面：

* 使用统一的开发规范来定义基础代码组织结构
* 及时补充单元测试场景用例（数据层，业务层，控制层，UI层等）
* 多人协作时的相互代码review机制
 
以上3个方面，我把它们定义为质量控制中的三个核心环节：

* 事前
* 事中
* 事后

```
“事前”的规范制定与执行，
“事中”的代码单元测试编写，执行与代码覆盖，
“事后”的代码review机制。
```

从质量缺陷修复成本来看，我强烈建议大家加强更早环节的约束力。

```
3个方面强制性约束从强到弱可以排序为：规范 > 单元测试 >  代码review。
```

因此对于希望优化工程代码质量的你来说，需要从意识与行动不断实践质量控制的这3个方面。制定规范并严格执行，尽可能多进行单元测试，尽量频繁地相互进行代码review。当这些步骤得到切实落实后，一般来说会对整体工程代码质量有一个比较大的提升。

以上的经验主要是由我在云信，猛犸，EP的开发过程中总结得出。

## 2.1 开发规范的重要性

开发规范主要应用在前后端领域，由于多人协作本身不同人员的能力，经验，抽象层次深度差异，因此容易在无开发规范情况下，导致代码“失控”。代码质量岌岌可危，问题频出，严重影响了产品原有功能体验。因此定义一套简单，标准，有意义的规范势在必行，行业中不同公司往往都有自己的规范，我们也可以采用“拿来主义”，依靠别人的经验前行。

开发规范主要涉及以下几块内容：

* 代码编写规则（命名，文件目录，文件内容格式等）
* 开发工具
* 开发工具需要使用的插件
* 代码分支管理体系
* 持续集成体系
* 部署体系
 
针对不同角度的开发人员，开发规范所使用的工具，插件，持续集成步骤都有些许不同，不过整体来说大同小异。

下面我从前后端分别作一些简要介绍。

**前端**：

前端首推开发工具：VSCode。VSCode的功能完善，插件丰富，迭代快速，大家可以通过其官网进行软件下载([下载VSCode](https://code.visualstudio.com/download))。

VSCode我推荐安装的插件有：

* 文件图标 vscode-icons：美化文件导航的ICON，提高识别效率 
* 暗色主题 One Dark Pro：专注与防视力疲劳
* 代码美化 Beautify：快速格式化代码，结构规整，方便共享与维护
* 代码检查工具 ESLint：语法规则和代码风格检查工具
* 快速注释 Document This：规范格式与标准化
* CSS 类名智能提示 IntelliSense for CSS class names in HTML：快速在HTML引用与提示已定义的CSS类名
* 代码拼写检查 Code Spell Checker：单词拼写检查

当然我们进行前端开发，也会使用一些第三方的服务来进行基础功能的支持，如埋点，性能监控等。对于埋点，我首推Google Analytics，谁用谁知道。对于性能监控方面，由于不同团队的差异比较大，这里就不展开讲了，大家可根据实际情况有针对性的选择即可。

对于分支管理目前使用最广泛的应该是：git-flow ，当然还有其他分支管理方式：

* 单主干的分支实践（Trunk-based development，TBD）
* GitHub flow
* Maven JGit-Flow

针对以上分支管理方式的差异，具体可以参看[Git 分支管理最佳实践](https://developer.ibm.com/zh/technologies/java/articles/j-lo-git-mange/)，里面有具体详尽的介绍。但只要定下来就需要在团队内严格执行，这是一项基础要求。

对于前端CI流程，大家往往会想到Jekins。在公司内部，我们还可以使用Overmind来满足CI需求（其底层也是基于Jekins实现），只是用起来更简洁，与其他功能集成度更高，推荐大家集成使用。 

代码开发规范主要是两块：代码风格与代码潜在问题扫描。这里我推荐使用[EditorConfig for VS Code](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig) 和 [eslint](https://cn.eslint.org/)。具体配置可以查看官方描述进行，这里我就不赘述了。

对于部署，因为前端涉及的部署主要集中在：html页面，JS/CSS/媒体资源（图片，音频，视频）等。所以一般只需要将打包的资源整体放置到WEB server的网站目录即可，相对比较简单。不过从规范角度来看，我还是推荐使用自动部署工具来协助解决。因为部署工具保证了流程性，安全性，统一性，手工控制往往需要人来介入，不仅低效而且易出错。

**后端**：

对于后端来说，常用的开发工具是：eclipse / IntelliJ IDEA 。因为我本身是从eclipse迁移到IDEA的，所以整体来说更推荐使用IDEA。

IDEA上也有丰富的插件，一般常规需要安装的有：

* Alibaba Java Coding Guidelines：阿里巴巴 Java 开发规约，简介与使用说明[点击这里](https://www.cnblogs.com/mafly/p/aliPlugin.html)

* FindBugs：一个静态分析工具，它检查类或者 JAR 文件，将字节码与一组缺陷模式进行对比以发现可能的问题
* SonarLint：一款强大快速的能帮助开发者发现代码里的bug或是代码质量优化点的扩展工具
* SonarQube：一款用于代码质量管理的开源工具，它主要用于管理源代码的质量。

后端的CI流程一般都会涉及以下几个步骤：

* 工程代码编译
* 静态扫描
* 单元测试运行
* 打包可执行文件

一般我比较关注前3个环节：编译，静态扫描与单元测试运行。每次push代码到远程库后由类似Jekins的系统触发整个Pipline，然后依次执行以上步骤，通过消息/邮件反馈执行结果。也可以通过平台界面查看历史执行情况，有效衡量每个阶段的代码质量情况。

当出现问题时，即时修复以避免对其他人的工作造成影响。这也在一定程度上要求开发人员更关注自己所写代码的质量，提高质量意识。

## 2.2 单元测试的意义

单元测试，顾名思义是在局部控制代码运行结果符合预期，不断补充场景用例，不断执行以达到功能代码/逻辑分支的更高覆盖度，让代码的变动影响“自己”说出来。

一般针对前端代码，我们需要保持一个好的习惯（开发设计思路）：每个工具函数必要有完整的用例覆盖保证质量，再由高质量的工具代码构建组件，模块，页面，辅以更高维度的用例来保证每个大块的质量。以小见大，说的就是这个道理。

对于后端代码来说，我们在合理的设计模式与代码分层/构架基础上，需要额外考虑以下几个问题：

* 代码的可见性（访问控制合理性）
* 代码的内聚性
* 代码的扩展性
* 代码的可测试性（非常重要）
* 代码的分层体系以及每一层需要关注的重点测试点

通常来说，我们需要做到的测试覆盖范围有：

* 数据层全覆盖
* 业务层全覆盖
* 控制层全覆盖
* 工具类全覆盖
* UI层重点覆盖
* 核心流程接口重点覆盖
 
```
“质量不是一蹴而就，每一个小步都为了更好的一大步。”
```

单元测试一直有一个被人诟病的缺点，就是：需要额外编写测试代码，有极高的代码维护成本。但我个人认为这是一个"度"的权衡，意思是说:

```
针对基础性功能代码，要做到更高的单元测试覆盖，才能保证上层质量，场景量需要比较大，
针对中层功能代码，要做到核心路径单元测试覆盖，没有明显的缺陷，
针对最高层代码，要有一定的自动化过程，做到减少人力。
```

"度"是一个测试代码场景数与整体代码质量的权衡，太少不足于保证质量，太多维护成本会成为负担。这需要每个人根据经验来控制，需要相当长一段时间的积累才会有所领悟，那么就先从写少量的单元测试开始，让时间长河来证明它的价值吧。

## 2.3 代码review机制探索

人员水平差异如此不可控，那如何来保证代码质量呢，最直接有效的莫过于代码review机制。让有经验的人/团队来review新人的代码，发现问题，解决问题，改进代码抽象与实现，一步步查漏补缺，构建软件“大厦”。

```
好的代码应该被看见，被借鉴，被影响。
```

代码review主要分为两种机制：

* 培养找臭虫/找亮点的意识
* 鼓励执行频繁点对点的代码review
 
**机制1：寻找代码世界中的糟粕与美好**

可以通过固定周期的会议来承载，时间大约在1小时左右。

每个前后端同学都需要在会议前搜集现有工程或外部看到的开源项目中比较好/比较差的代码，然后针对代码进行重点分析与轮流讲解，内容需要包括“代码示例”，“原因分析”，“改进方面意见”等。

这些会议承载的内容最终落地到团队wiki中，逐条记录提到的代码规则，日积月累后再总结形成团队人员编写代码的执行规范。

**机制2：用经验与知识改造代码**

这种机制主要关注于“**频繁**”，“**点对点**”的代码review与改造，具体操作方法可以采取每日下班/上班前的"例行公事"：

前一天/早些时候所写的BUG修复/功能开发代码提交后，请求同领域同事当面
进行一次代码review，期间备注下“面对面”沟通所发现的代码问题以及与之对应的改进意见。当原有代码调整后继续发起新一轮的代码review，以此往复，直到所有已知问题得到改进，代码质量达到预期为止。

代码review有很多工具可以支持，一般来说，gitlab平台的内置功能即可满足需求。查看分支下的提交日志，查看每次提交的变更内容，备注问题与改进意见，这些都可以便捷操作。最终也能通过查看历史代码review信息，发现问题功能分布，人员分布，更进一步提升代码质量。

```
小提示：
代码review要求每一次提交是对应功能的单一实现，这样才能有效进行代码管理与代码review。
否则代码review很可能就无从谈起，无法切实执行了...
```


# 3. 总结 

工程代码质量控制是一个系统性行为，需要整个开发团队一起协作才能把事做好。以上这些措施执行后，短期即可快速提升质量，但团队长期的质量控制习惯会使收益最大化。任何事只要开始做就需要长久坚持，持之以恒之后，这些行为所产生的价值才会逐渐放大被我们所发现。

质量控制需要从每个开发自身做起，每个人多走一步，多做一点，系统代码中的美才能被激发出来。