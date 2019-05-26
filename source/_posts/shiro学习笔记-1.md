---
title: 'shiro学习笔记[1]'
date: 2017-07-25 21:43:36
categories: "shiro教程"
tags: #信息安全，权限控制
		- shiro 
---

# 一些废话
在我写这篇博文时我大概断断续续学了三四天shiro,期间因为看的不认真的缘故 ，我对shiro一知半解根本算不上系统的学习。在我学习一样东西是我奉行的策略就是边学边做，于是我花了半天的时间模仿着别人写了一个例子，终于对shiro的权限控制流程有了个大致的了解。对于shiro[张开涛老师](http://jinnianshilongnian.iteye.com/blog/2018398)讲的很细致.下面就开始我们的讲解。
<!--more-->
# shiro
## 简介
 >Apache Shiroa是一个强大且易用的Java安全框架,执行身份验证, 授权、密码学和会话管理。 通过Shiro易于理解的API,您可以 快速、轻松地安全的任何应用程序从最小的移动应用程序最大的网络 和企业应用程序。 
## shiro模块
![shiro的模块](http://file.flyfood.name//shiro/d59f6d02-1f45-3285-8983-4ea5f18111d5.png)
在讲解这个模型图之前咱们先说一下一个**web应用**用户登录的流程
1. 用户在登录页面输入自己的用户名和密码，然后点击登录。
2. 服务器在在响应这个请求时开始对用户的信息进行比对，如果比对成功，那么进行下一步
3. 我们开始给这个用户授权，就是给这个用户分配权限，那些事你可以做那些不可以做
4. 用户登录成功开始做他能做的事

一般来说shiro在第二步就开始发挥自己的作用

- **Authentication**：这个英文单词的意思是**身份验证**我们在验证用户的身份时用的就是这个模块
- **Authorization**  ：这个是**授权**的意思给用户分配权限
- **Session Manager**：会话管理，即用户登录后就是一次会话，在没有退出之前，它的所有信息都在会话中；会话可以是普通JavaSE环境的，也可以是如Web环境的；
- **Cryptography**：加密，保护数据的安全性，如密码加密存储到数据库，而不是明文存储；
- **Web Support**：Web支持，可以非常容易的集成到Web环境；
- **Caching**：缓存，比如用户登录后，其用户信息、拥有的角色/权限不必每次去查，这样可以提高效率；
- **Concurrency**：shiro支持多线程应用的并发验证，即如在一个线程中开启另一个线程，能把权限自动传播过去；
- **Testing**：提供测试支持；
- **Run As**：允许一个用户假装为另一个用户（如果他们允许）的身份进行访问；
- **Remember Me**：记住我，这个是非常常见的功能，即一次登录后，下次再来的话不用登录了。

**在此我们要记住一件事** *shiro不会替我们维护用户的权限，它只提供了抽象的模块具体的实现需要我们自己去设计然后注入给shiro相应的接口就行了，他回去管理我们设计的安全策略*

> 一个优秀的框架一定是对外友好，对内易扩展的

下面我们从外部看一下，如下图所示
```flow
st=>start: Application code
io=>inputoutput: Subject
op=>operation: shiro SecurityManager
e=>end: Realm

st->io->op->e
```
Subject是直接跟应用程序交互的，在对外API中Subject是核心
**Subject**：主体，代表了当前“用户”，这个用户不一定是一个具体的人，与当前应用交互的任何东西都是Subject，如网络爬虫，机器人等；即一个抽象概念；所有Subject都绑定到SecurityManager，与Subject的所有交互都会委托给SecurityManager；可以把Subject认为是一个门面；SecurityManager才是**实际的执行者**；

**SecurityManager**：安全管理器；即所有与安全有关的操作都会与SecurityManager交互；且它管理着所有Subject；可以看出它是Shiro的核心，它负责与后边介绍的其他组件进行交互，如果学习过SpringMVC，你可以把它看成DispatcherServlet前端控制器；

**Realm**：域，这个很重要也是一开始让我迷惑的地方它到底是用来干嘛的。上文我们也说了SecurityManager是shiro的核心统筹大局，**SecurityManager**主要是制定战略，真正的实行者是Realm。**Realm**负责战术，一般来说我们会自己实现Realm，然后在这里进行比对，验证用户是否有权操作，你也可以认为它是*security dataSource* 安全数据源

也就是说对于我们而言，最简单的一个Shiro应用：
1、应用代码通过Subject来进行认证和授权，而Subject又委托给SecurityManager；
2、我们需要给Shiro的SecurityManager注入Realm，从而让SecurityManager能得到合法的用户及其权限进行判断。

# 后记
在一开始学习时我们最应该先理解*Authentication-Authorization*这两个模块,shiro进行身份验证和授权的策略是什麽？它内部的流程是什麽？
另外在我们平常写项目时对用户进行身份比对时，一般与从数据库中抽取的数据进行比对，shiro抽象出一个模块**Realm**来做这件事，英文意思为*范围，域*的意思。shiro再进行身份验证与授权时，真正的执行者是**Realm**.
下一篇我们就细讲一下这个**Realm**