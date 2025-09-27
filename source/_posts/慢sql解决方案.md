---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QXOSBEBI%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQDFFPuurj0hy66zeIlOyysBTraVLvcvJxjNDsPSQEwe9QIhAJLuimNqyeLzZU5vTFoGfRvWIa4%2FXb68pH8EZk3CR3whKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzlJc7NNBHmCgMekuEq3AM%2FXpe5J%2BpmcuQxpbJ50aP1yYqa5QvPuQskzByiAVjGRMV6baHdsqsgl0VV0Kc92vRczPhc8Hepcq1%2F3RQSN2NODJlf3cWqsZKQwdQ62V9QV%2Foxy1gCkwEFy%2FPMNj%2B0AZzz4Tnr%2BVp4ngcifLvb1mhufJ4PoKV8yHqLRpIDAS6FOsNfHrKz1gB0oZn3gK%2F%2FguzbHffzmRsbV8MfZ%2FqQyA7HLmI4S8w1cd1pPV%2FHhDSmNks0rSRm6NGj4XBv%2FvEbeVgCAhsPsj%2FM5d7XWU90f12jYboAo3Kdm1BfTW4CFP6uP6YT4SpWABsaMJ7xxmmydHReK0TE8gEngwMeuM%2BObNM5L59FbEPIthpNJ7KH7DDvS1fPDMhwjiiD%2Bu8QA73qSxNiJfrNxxw9fAXRJCucZsI%2B2hJIyYq5unxQSH%2FKECLi5RhcJD7FM18VBbrXXpKllvtzXskeeKax9YyhohUTKzMBkEiBGvLJYq5mh15tfuOdRcLzgQB9s9mSF6QJ7sAe6%2BDG%2BPQWPG%2FxNMNurvZQHBVR%2FM%2FhOAYwC0XfJx336Hpp9Y7jKtFJpv%2FSyfxAm%2BxCM%2BGaA56th02bepCnAsibnszKRCHbavrl1cknjWvv%2FfOEERkmka5ZqJ7guwvZdDCzquHGBjqkAU4lLxJ0RSg7N%2BS8UNrj1sKXIn4KcUYza9W5uCq59TeVHV%2FnjqfTc4%2BKdugorc8igb2zrlXXEU2dS9v%2Bjvn9zRZxmAuVuYh0mVcKZv7KDelmXeKP9TlErgohK7ejUacESx2fHVcAuKx1a%2BYX8%2FYu%2F1E7gjxbPCEx6ZLp4vgop2u78yottodA0Qw5Xik0GhY7NJXleHFlQShndQ2K4itMUpYK9I7T&X-Amz-Signature=ef5078b1c470ee2ed82960e872bf5a7c4a62a66ca44b40a765b71dc96071a8e0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

