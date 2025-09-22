---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKYAQJGR%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC4igPWfU9t78xsuvfooNK%2BczfrSC%2F95Q%2FHtupgFx55tAiA%2FSgHQ4BvTdODy%2BeagYEdYM7QoSqrCGJK1jzMGl6CSVCr%2FAwgoEAAaDDYzNzQyMzE4MzgwNSIMuBDWPzYjP%2BmkeHOhKtwD5awBxO9Xmkl%2FijmquThX8WHzNy3JLSS4S%2BxHcqN6GZ6qrFBNI%2FtrKsvCRq%2F84XTm5OGecATkYUL6G9fwptWXYJD%2FG5%2Fy2wm8CxtCxeYU%2FRb1jhCLgB0pVBQ2YaSUiQiXqOQpbjlHEjf7DYAfMtUVQ0C%2ByZ1r2KVVNyW9TseGAAg0XxulQY0f6uQm%2BxC7J5LQma7ahm531sQigiLh%2BYrPjeAxqrihXmcTeD0PzM2WhDc3xhOL5TuAdSJcRffNhrHzKrkXUavNx6btLewNo6VpIj%2FwfIrTd4sz27VM7eugRjlUMKdlNZ1buJNLgN7harSrnJPstNWBxxoXTvwv%2Bm2R6ulttwFAAV2Xk3n91LO6LULbMSSEB2c2pPrbb3BNOqKqHqD7ZGIvcBubvKYJt8IexcYtl2qoI3bKNCJ0mgASxNh0YLjkFALbcyFraHqh%2F2m%2FMyF74eX15EHy4TO2dhHsjC2oXHiO7R6sqH2Brjd4c5lc4RH51qVLj4bi89kaHV%2FbcernLHs1Fzo60qaLzt%2FVD8HFnv1acPFVkBTtFjnvCpod%2BLGAcAQI7eVPrVF5ngaJ%2BesaDv0lmtCdnAUVJ7Mj51bya589lUGh1PHnq5jcRB8nmz8SojTdq56Hvv0wre%2FDxgY6pgH8J4isM%2FTyY5oZv4jahBnYCRrgr0StCm%2BNCATjOUG9woDDnIeNtcAs1NpG%2Fi0U8iikXXwH3PvpHMqQLW67hfKNgIMYxTkR8FD5HriWCsB3Fl8OKzb1gbqPrXGeA0bnzeMdS6GMFdWLt8rnavIJ5eH90aoGIzVAoUBrBfcxiEiN6CK9J671hkExOzE6fdUmn5wI96o%2BXkHm9KvJ3GQsi7ME2uY8%2F3%2B6&X-Amz-Signature=8168220d12116046281979967af1425df4a7d3a2cbdbf9a4fc562c2d84f12bd9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

