---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMWZ7ILF%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T170052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCBj9p5U87gMsojIKXaWLpjbqNTc7aBDNbKvcABcfP6uwIgHstwa6f13g6fgZQffa07j4UFJnE1rk%2Bj9fIc%2B9%2BvgwQq%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDAx%2BZBi9vmC%2Bd%2BlXeSrcA%2FH07rGW9pLmciF9mPFa6uDmaTmTxjZexrxLDAE5bURJmGIfIBX62TMsMTvHyQb7jzPwIv2tIKDKTsZnHZL20GjmGCcSSznq46dHrnuNeXYH5QQgzI8HBLTo6uzheuRX7JCWvh56IS81Igdq%2Bgb%2BXsJYCPDUlp2ZpLtDmUuJmaOJwm%2BuKYEzCfTbZDAOYoXN9vtYjhrhLdczoTbpiGrUdETIXlf94YQl9MEkKzU1Cjd520QbQj%2BxheKEqDEyLBaj0hv4LKkxN%2BdM%2FfTnZ8i9TWNIPwDtsHQPj7iy06V046qz6Es9wL%2B1lnWUfnrA1y%2BF4f6d0oHGIEF1ZOk6PHRGGJLlJMJbTwh0%2Bzjg1n16qYU5sxDc%2BhlMUEh9R7KeZsPxKbllZPxI%2BKh40Y%2FtGRDZ07PFwlncDJf4wRRy%2B6j%2BmHIGnVWaA3vVIO8d0y9%2FWx0%2FhcVP6S4OfoHU24P7Q5qkBqcIg%2FOVmzIBAFBkYO8qx5i8fEtiU0bylxmY8iEzHVS%2B0znHLUs3nBHB3fUj%2B%2B31R3xZhB4GKN3U1dLfc%2F9VsR5WJx9qjgveX8K359pkgqKWlOYDnINfdQQUBuwc3HNAsyoy0OS6KcNLkq9ZgoTPVmLsH5paV5wO6I6idC5XMJuv3cgGOqUBIp3N0rn0G0jVDLKFQep%2FXO4fIx1Hl9K9hMYDPZrf%2BP56iIQHKBjMkKF6fomd2VyFZylL2uYgxbhhhkESWgq7HzQJ4KOB02eaJOfkQLhoAWOqrmssHHBXQYoJVqq491RIpIuYwYgxd0I%2F%2F7HKISSJsx3Aba2nq7%2FbMAtsRup6EaPGRtgr9WMmDTdW51qhJ%2BTGURopdgD0xB9NGvEsr0K6RBstJwIA&X-Amz-Signature=da53a2c2d67cbe34291c07b47867d672a183d3e3787dea9e72da0eceae17c8d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

