---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJIKXPXA%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T040051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDoXnZD2fPApahhX%2B5PkaXMjRHndyEzf5hweNlYtZYbIwIgbBGnv%2F0B3itudylFIg3C5OqV1icpHzoUm6nimi6jExMq%2FwMIJBAAGgw2Mzc0MjMxODM4MDUiDBoTnOc6t1XvD8QbCCrcA4Oiffi5nYiUfi9cmaMDwycZaydXQA655bJk0BsdpvdCI5XPJrwylK78aERlEUj%2FUb%2BQgkT2rRwgOGJW7vi8fSoru3Ll8Xbv354DcFZrt9OBuyyaGfb8ksBzb7cBq6sDzKeN3J1idGU%2Fz%2B6IoTt3sXsA%2BnThyhfZIN5%2BCBqoGP74YWGbWn9A3NwTq9gJ8ysnDNsH%2BkR9ygBaLsZsB3vhNp39Yomkwoyc2SUgXiMiUTc8%2B2FO%2Fpl%2FUdOzjJ2lOhcwZXIyV6dox6ok%2FmQbTfo3s%2B58Z9ubhNUhScYa5kJEbrhRVoLfNLAT0kzWB5%2BiUNm5FpU83lJGa8H%2BTuwtxahz%2FkP7h6NlBT%2BkfvwPyweIGhwGmLhxDWq7RCIJLlbjjoeaxpqhjqsJ0koAFBht9D6ghizjUl%2BN8czcympSKmpLNk6vwaAS4s2%2FsjqBJR9qCEYoRC48ufCjlnSAp7ndIFdqTm6aV3CX3DFurgQQTUnf1TGFa3LkqEkFbz59yZOi4woCa0FtW0pNBJttmVPXRtk0n4vTGE9I5QtfsggAXGQZnEDlAeayrMJerimL%2FG%2BbFvNXwJUNPUBxSJ1mpUri4afGH%2B8g9gM4bKzyTjfgCWSa5UPs%2F%2FQsGStOK1JpNjEmMNDe98YGOqUBbGEqOjqStAy3ceV36i%2Fo7GZWYc0LJ3FxMS4ARxW8UtJR5pjWJ5INCYmjnSGZo%2BAxzbhedfpjan12UyrzYPSRdgxmKGhgMQWbH%2BGvxj8SDEJ1Mbu5KzMumBeydT6Lp3stX%2FxOfb4zsfdKsqduC5C3TJsZ10QSqs%2FFxORrzGe%2FQXiPcLWNQ9peTlTWtRlO%2FAG5WuyN95%2FiP6VF71OOanIYM8xW36UT&X-Amz-Signature=46905f2d8559467761ac1a19c2fc56e33ad180ee11f33f4863691e0e554ad19f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

