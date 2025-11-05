---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VLUYGJA4%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCEeQicvjGu1PA5cFu67gSydZ1r4otQIxh33FWJsaiDgwIhAOLANn2p4DEVMs1DqGqjm2zFZlhFi%2BbQH5SJN3pG4fNnKogECJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx25yWEf5JMIIRltgYq3AOQsHMQHYYVJCcB%2FBBrGs0tAX3vAbz39mvYKJ7YLaQ9B9P%2F7jE96PpqNzpdCCIVqp%2B%2B3LfH8TTk9Z%2FQ4DsUv%2BPUhb%2BVAXeCB3lt%2B9T%2BYemRp2H0hVk2ZrGHgnWml0NeBRbcH3hr93DF01ZNQ94JvCAGOsoqzkUszvWWc1notsxovUNhTQ93EE9lxroK1GywJOR7OT2OR6GrAR8nvXP1t9Mir4Du5PpeotvU6H8IXNPKXircrrwKJt919D35NR0hLy%2F0UGGOaqaimLjiDU1mUrZJrNTEPOd2DgEk5mjLLgFod2dHaDrqp6edpc4yixZAml8NOfgP6MmYfQYrXYcf7gQIL5ljUpMYO%2Fh9DeSmaH1R8fGaZT6u5nRbJxMsMO3VJJLPfke7WgWRm%2FDqwamWZBho%2B0RSbJ7wYNNnPD0jgTyIb5So%2FfyjzcFtGW%2FOx0%2FjwpsSgBwWqGd%2BLjBxZ%2BfDG8j8Iu3h31SgptqhOB1wpNMLKCbwvFnoUtp76ZyGm8U1HhUIi%2FgT83oer5fAiD7WQ8q6eF4Sn2ojUUpjSGFRNWjhTXyEQM%2BCAEbGkhqsTRLESHx08D0%2Fm5YmtE%2BfT0FWLhRNjJX%2FQaqsoGwSAfk%2FezyYrvT4LSbq5%2FM5wMOT5jDGm67IBjqkARgkQWKzbmtg8VMK%2FUIS%2FIcOCN1VhEZOcvQZc585TLOPGf7SBbGzy40kJ1b7Pzh58rEb%2F%2B41JFMqt3McYjQvJX%2FPve9E0JXA7h%2F%2BNRREgzdzGfwxhsVoQRNbqhl%2BeZrCC3ghhmqd4zTjUZ5lqcKoohZUnk1H6EOcGo5LvoJN7dgC2t40JVhxF6%2BB2K5z9VFJEBV45dkOWqwYibbcrxDD7UIxqCgT&X-Amz-Signature=22f5b144e0fd9fc3f8396861eb3a43a775ca1d36bbfac6a1edee9d6cea3cb585&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

