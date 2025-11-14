---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMWZ7ILF%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T170052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCBj9p5U87gMsojIKXaWLpjbqNTc7aBDNbKvcABcfP6uwIgHstwa6f13g6fgZQffa07j4UFJnE1rk%2Bj9fIc%2B9%2BvgwQq%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDAx%2BZBi9vmC%2Bd%2BlXeSrcA%2FH07rGW9pLmciF9mPFa6uDmaTmTxjZexrxLDAE5bURJmGIfIBX62TMsMTvHyQb7jzPwIv2tIKDKTsZnHZL20GjmGCcSSznq46dHrnuNeXYH5QQgzI8HBLTo6uzheuRX7JCWvh56IS81Igdq%2Bgb%2BXsJYCPDUlp2ZpLtDmUuJmaOJwm%2BuKYEzCfTbZDAOYoXN9vtYjhrhLdczoTbpiGrUdETIXlf94YQl9MEkKzU1Cjd520QbQj%2BxheKEqDEyLBaj0hv4LKkxN%2BdM%2FfTnZ8i9TWNIPwDtsHQPj7iy06V046qz6Es9wL%2B1lnWUfnrA1y%2BF4f6d0oHGIEF1ZOk6PHRGGJLlJMJbTwh0%2Bzjg1n16qYU5sxDc%2BhlMUEh9R7KeZsPxKbllZPxI%2BKh40Y%2FtGRDZ07PFwlncDJf4wRRy%2B6j%2BmHIGnVWaA3vVIO8d0y9%2FWx0%2FhcVP6S4OfoHU24P7Q5qkBqcIg%2FOVmzIBAFBkYO8qx5i8fEtiU0bylxmY8iEzHVS%2B0znHLUs3nBHB3fUj%2B%2B31R3xZhB4GKN3U1dLfc%2F9VsR5WJx9qjgveX8K359pkgqKWlOYDnINfdQQUBuwc3HNAsyoy0OS6KcNLkq9ZgoTPVmLsH5paV5wO6I6idC5XMJuv3cgGOqUBIp3N0rn0G0jVDLKFQep%2FXO4fIx1Hl9K9hMYDPZrf%2BP56iIQHKBjMkKF6fomd2VyFZylL2uYgxbhhhkESWgq7HzQJ4KOB02eaJOfkQLhoAWOqrmssHHBXQYoJVqq491RIpIuYwYgxd0I%2F%2F7HKISSJsx3Aba2nq7%2FbMAtsRup6EaPGRtgr9WMmDTdW51qhJ%2BTGURopdgD0xB9NGvEsr0K6RBstJwIA&X-Amz-Signature=ff8de36bb5a862c51faf18d87a2eeb4bf610fbe1975dcf66648642d680780db6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

