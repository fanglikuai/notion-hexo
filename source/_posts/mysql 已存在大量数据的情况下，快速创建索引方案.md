---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQQSCY6I%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T060055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJHMEUCIQDzo6LEJpWfBvtnc2V%2Bw5NtC2qyeE%2FIc%2Fs3PXZ7UAKjUgIgLJ6S65dAFCvZIx3U6bVPla0%2FM%2B9XkfriIwMpQmJbZ8Aq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDErUmEyAXJDzYnuGryrcAxoSsnv9yZACjcFtKbBM1jPzzdQ33NSU2U%2BBzJYIMlem7zM2GhSR3BGgDW81aYz4evzsfCPDOnRq1L72YEjmP7w6J0e98kGGVJMRR%2FfMYi0UoMn0QtI2Fabj3UL4vF6%2B6AgiWI%2FKDzLrqb2XR91YmqGRAApU9TlgrHa8LUWgDPwjHhXa23Yeku3km3UAJzlHEHIDDlw8zsBjs7KnriQTFv8Ix73f6D7kW1AAlCPcvXQYNnn7LCYb0uW4GaBYxq71Cq1IYHqI15Etd5AyyHwEZ0U%2Fou4qlyuuowxmY%2Fz31XvK%2FbzuH8%2F5M%2BmAZzVgTg1s%2FJmRJ%2BP9Nx2%2BM7BKqOZftBl0aOL2y2AXqVeRWIzhhlUD58ncy0uzNKL44a52LMZgNIUoLTgNbfVD5SKHkB6Fg1dvdtj5eq7NhXQ8SW7pjH9UKbsmO8thP1r1fqme%2B271cTSKL2eOd9MUZv5B5MhZ1eZdtfQCeMQIFKrfvmldg9dIWPNXgIee1JiA%2B2n5ilp09Yr20HgbR3Z3eNICNqC9DjdWIww%2FWo6hkOp68qlizJDwfyJPLRWJfelXk2mP9VKfhAEcLMWjPh50ECPl2rOD2INMUsY2W76bX3KRu4UAVL2ceQIQ7v%2FAlnE%2FIDv6MLCEy8gGOqUBaxK8AUpkmzPN0MMlq8gCrvVOThVx%2BCLLKatJtK2jgMqaWjEf6Zg1Wx%2FWqA4tpBsD5C1fNk8Ft%2Bf%2BxhmoBNI%2FJXml4mDv9%2Fmc0SwjnUrFjhImg%2BupWr2oZrbsoeXwGbDuNjnUZOHis8chPH8UuYcjDZ33aC997OAyp3xFourzEDZGJxmg74mpGMzfl%2BM3omZ80uqMTRImNZ05Mg8jSVxGJeDz1Rl9&X-Amz-Signature=b7d4f118bae88bab42f4e03a941aec9782bd087b9e158fe3b60ef3baf45c3df9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

