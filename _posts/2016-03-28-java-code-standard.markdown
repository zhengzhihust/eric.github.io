---
layout: post
title:  "Java 编码规范"
date:   2016-03-28 22:39:00 +0800
categories: jekyll update
group: navigation
author: Eric Zheng
tags: Java
---

#### 命名规范

1. 类名：大写开头，每单词首字母大写，如OrderStatusEnum

2. 方法名、变量名：小写开头，后面的单词首字母大写（驼峰式样），如switchCreateOrderStatus(Intger orderStatus)

3. package名全部小写，如com.alibaba.service.trade.core.singleton

4. 取名上尽量用广泛认可的英文（优选英文）或者中文拼音（如Haitao）,

5. 关于长短和缩写：可以长一些、以可读性为主要标准、但也不能过长（不能啰嗦），如DB，X509，B2C等

6. 命名要与内容吻合。举例：RefundShipCompensationTypeEnum VS RefundShipAmountType

#### 文件编码、工程编码

java工程/ide编码：UTF-8

#### 目录规范和类文件结构规范

1. package规范：服务化代码package全部以com.alibaba.service打头；其他的以com.alibaba.trade打头。模块内部的代码统一package带上模块名，目前有order、refund、logistics、cart、snapshot、base等。

2. 类文件结构规范：

3. 所有类属性声明要放在类头部

4. 新方法不能全部加到末尾，而是按重要性、相关性放在合理、靠近调用的位置

5. 私有基础方法可以作为简单api使用，可以考虑放到末尾、或者提出到专门的util类中去

6. 重载：构造函数有多个或者一个函数名有多个函数、重载时，代码必须放在一起，不能有其他代码插在中间

7. 类成员结构：必须有合理的逻辑顺序

8. 导入：明确、直观、无重构提示

* 导入不能有“\*”，否则无法判断依赖了哪些类，且更容易类冲突/误用

* 导入代码里不得有未使用的类。（IDE会报未使用的warning）

* 导入代码块按ascii排序（IDE默认）

#### JavaDoc和注释规范（\/\*\*开头是JavaDoc，\/\/是行注释）

1. 类、方法的JavaDoc必须自动生成并加上必要描述信息。如QueryForShopService容易和ShopOrder之类的概念混淆掉。不写类的JavaDoc，后来者改的时候、用的时候会有潜在问题

2. 重要业务逻辑、有特殊处理的逻辑必须加上必要的注释信息

#### 格式规范

1. 空格符：只能用标准的键盘敲击出来的空格或者ide自动填充的空格。不允许用tab键打出来的空格字符。

2. 大括号：if, else, for, do and while 语句，就算代码块是空的或者代码只有一句，必须要用大括号括起来。代码块外部的大括号，是在方法名或者if等关键字的那行最后一个字符用{开始（前面有一个空格），然后再回车换行，紧跟着是代码段的内容。

3. 逻辑代码段或方法之间用空行分隔，连接符、运算符等一般前后用空格分隔。

4. 格式化：IDE代码行长度默认80字符，不允许调整大于120。command+alt+l可以进行文件格式化。如果发现格式化变化超过10行，可能会造成跟其他分支的严重冲突，需要撤销格式化操作，只格式化变更到的代码段。新写方法建议做格式化（编码习惯理论上写出来的代码本来就是符合格式化样式的）。

5. idea统一配置下载：主要包含：

* 格式化 代码行 宽度120（默认）
* File Encoding : IDE Encoding&Project Encoding UTF-8
* 序列化的类没有serialVersionId 提示warn（alt+回车 可以自动生成serialVersionId）
* 导入类时杜绝导入.*。配置成use single class import， class count to use import with \* 配置成99

#### 编码风格约束

1. 类php的写法java中要考究性地使用。如unset的风格、xx==false（必须用!xx）

2. ==和equal。只有原子类型short, int, long, char, float, double, boolean, byte 才能用==。其他只能用equals

3. 对于List结构的遍历，用for(XXX xxx: xxxList)来遍历，而不使用下标遍历

#### 类和方法的规范

1. 类的纯粹性：Class xxx 不允许有非该类应有的功能和属性

2. 类的大小：不能超过1000行。超过1000行要考虑拆分

3. 方法的纯粹性：方法粒度应该尽量小，每个方法应该干尽量少的事情（干超过两件事的应该考虑拆分）

4. 方法的大小：不能超过200行。超过200行的一定要拆分

5. 方法的参数：不能超过3个。超过3个的要抽成参数类

#### 数据对象（DTO/DO）规范

1. 必须手动写toString方法。ide自动生成即可。有变动后必须重新生成

2. 网络层DTO必须主动实现Serializable（新的DTO必须遵循；现有DTO不规范的建议看到了就改一下），serialVersionUID要用IDE为每个类生成唯一的UID

3. 网络层DTO不能用于内部方法传输。必须用内部DTO/DO

4. 数据传输对象的命名上，DTO三个字符全部大写，新项目不允许用Dto的字样

5. 微服务体系中的数据对象问题：综合服务层：只有DTO, 在微服务的DTO和返回DTO之间转来转去；微服务层业务逻辑比较简单，在facade层定义DTO(序列化)，服务(组件)层可以DTO透传，但不能透传到DAO层

6. 一体化复杂服务（如退款），需要多个接口多个DTO，内部考虑复用可以采用转为BO进行处理。但应该限制其使用场景。用BO的场景需要先进行评审再进行使用（以免大家风格不统一）

#### SQL规范

参见[SQL规范](http://wiki.mogujie.org/pages/viewpage.action?pageId=39229268)

#### Redis规范

1. 所有调用redis的地方一定要加上try、catch，记录error日志

2. 如果业务只是用redis缓存一下结果，调用redis超时之后，cache住异常，再去查DB，不要直接ruturn空或者抛异常

#### 缓存规范

1. 应用层对缓存的使用规范：缓存是非持久化存储。我们在用的时候，应该有主动判断非持久化存储拿不到的情况，如何处理的逻辑

2. 缓存应该配置主从。消除缓存单点

#### 日志规范

参见[日志规范](http://wiki.mogujie.org/pages/viewpage.action?pageId=36366255)

#### 重构规范

1. 原则上少写V2. 或者V2的存在时间不得超过一周，一周后，老代码应该被替换为新代码，v2代码删除。V2只是短暂过度。服务层代码决不允许V2 V3或者oldMethod newMethod之类的代码长期存在（不同于手机客户端代码管理）

2. 不许注释代码。只能删掉。（git版本可追踪）

3. 写的TODO后面必须加上解决时间

#### 测试规范

1. 所有DAO层代码和有处理逻辑的代码都应该有覆盖率达标、断言合理、可持续集成单元测试。详见单元测试规范

2. 核心业务的服务接口必须有接口测试。有变更时接口测试通过才能进行发布
