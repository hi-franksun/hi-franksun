# Selenium 概述

Selenium 优点
开源、免费，功能上也不比其他付费软件差；
全平台支持，无操作系统/浏览器限制；
支持多种语言；
与其他工具轻松集成，pytest、Jenkens
支持并行/分布式测试；

学习要点：
运行原理
Api 调用
应用到实际项目中

难点：
了解 Web drive 运行原理
多环境配置 Selenium
Selenium 和其他软件配合使用，例如单元测试、日志系统、数据库、
理解和掌握数据驱动的测试
与 Jenkins 进行集成；

## Selenium Projects

Selenium 是一个自动化测试项目，这个项目主要有一下三个小项目，之间相互配合完成复杂的自动化测试；

- Selenium WebDriver
是客户端的 API 接口，通过调用这些接口，来访问浏览器驱动，浏览器驱动再访问浏览器；
- Selenium IDE
支持录制回放，基本上不使用；
- Selenium Grid
测试架构，支持分布式测试，使用在大型自动化测试中；
