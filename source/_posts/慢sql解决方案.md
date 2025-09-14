---
categories: 数据库
tags: []
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
updated: '2025-09-14 20:03:00'
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

