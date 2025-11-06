---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466STO5A26N%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T030051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDreU6rZDcYwh6fO4EbjCALdv6opjr2rB7DRieNItuOHwIhALbYt5pU334QSxWzD%2BUN8D%2FTYHpx0YJ0lbLSQl%2FgKYM5KogECJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwurSWkoDqcvV5PI98q3ANlaL1PlUvWttnGz6GynX4BmlIOyaG%2FnX0IxMkPsYvfW6w3ht56W7XX4O68FAutcWvB98K1G91hTCGtcCJ8sRxxAqOYtpnVfBDjH9hPwHXspmOA4tjD0fr9Fn8NEKYTy6NDB18aWfsePHQrj91zcM8y3J4Qeoyo45L4Phc0LQg%2FjUf%2FnI4ByE4sqOD9hft8DN9QchNXxQiA4Tty1TEFN4Paa32Q4zwaT43VoYTOJtzxZRNXE7aerd7uyfCJnKImbknGEy%2FQOFfWSsnRSGA06eBqlMs6R8HoRlC%2FA%2BJg%2Bd54VRvmMH0quSyuDlIpwsQuflSfeK%2FnpySSIcGWaMcRYswWLSKXq84W2cXky6rr%2BZDOhgt00PHSzS2b7%2FAWNRkQhwi1S9%2BGUA8p3VK4%2B%2FSNX7rfWYSKEwW8gyu5nQZT6E2fm2GR1x9e83CZiFesawVC1zEfpb7USY442Y%2BhLOUiClU8mCu%2B0V09ynirqC71u6Rzb49oMzzHl3GyNiQEmvTnfP1eEaCqGValttsS80VquEKlqCd72BQ0kqzZASBUne1OBSP2KW1OOytN1VRu%2B1qp4RMRcsOcfGEqt%2B6k%2BzzbJoxXvRaqIWOUZJILdK2LSjwb8cPvJgtAzzKyNPjhtTDN%2F6%2FIBjqkAdIH8eg3fgEZDY%2Fus0HhhdFYX7giRXPuMlouZqyzdkAeHKQh3pfBrm5WfxIFyPkqjcV1VnrH%2B8KCd26Om49QYGkBbgCvT9xW%2BNGVjwVIEo2G2dS%2BI9P%2F3hdxNXEjTudqvi6RucbONHEaLQAx37bksHnjyUeyn56bqzLy0HoXktijMihwR8z6oqaTlnivPQnaJ%2B1QRLIiZzVcNYhDSVREFIUKB5iL&X-Amz-Signature=7ea285d74fe5b8acf3820516dced358de21114faf5a4480f1b93e31dc9549cea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

