---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7UTSFHL%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T130109Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH0hw0MGMgA2a7q7%2FQlt3%2BgEPvSE66WfvzkVPEm7oNYfAiBDxWDnniASR8R7KqWXeL7c%2FCI4Zq1dMN7lE7A%2FLHQBaCqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0RjBdXM2j7OqfXV5KtwDGh8pZMXZC720APlhMtoW%2Bf5N%2FBoHEfEv6iwujV%2Btp8JHhbf1qDz0n95UiXZ7iVVNjuaQ4yyJTv0NtyXcrnqivuoVy3EWowRj1Nb5x%2FWRd64k8Rj9Lh0s%2FT6R3%2Fan1kOB%2F0tmaBnugzRYaGelaYzg4BgZpTt2%2FSiEOZOaU68bKtL9axtAOyjeCqfw%2BnTK2bmIdCnJ3GOu0prqyxKF64iAf0IUgGQbL47NGIdKztN8XFt%2FZlKFX8BTUQAARFL%2B9UULgTLr5mLMaPEUAAa1MfG2f0TUOVHQeqgVnXEoD1E9NuftwsExFyGk%2BLx7c9UYvvyILI2%2FImXEPa3izmqOY%2FhkGMzzApz6Xt%2FGcQkRwXpTMhdnXY3%2B0bZHbr2RKSIPG471uPcRr9BBWIq%2BUdoh2YnJjBYLWl849DJNNHfe5x1MZNDUufJmG38%2FijrVmhCoj%2FW%2BDuUleeh%2BjQ%2Fu6ASH9Ns%2Baa1aLAzesJ4DXMONEBuD9btZyYHy4dG5KpDMW9hQ8lGfA7fxEahoGmgwjCFb%2Bq1R6ZHEqJT1fIOPuhfmJu3ygoBmwQ1e16tsYzudVYX%2FEX3k3SCT0jA%2BZqP2bQo9Q1%2BAvBpC3nba3SegpQ%2BHfG7OcAYXtRhLt3jkEILQ4lAw0tilyQY6pgEHQjXRqKNeqL6w2ULhYJcDKQYrdwTwAYaqKrRZUS2IaBOwY8Y5yFZ94CHiTrIAny4elItvFEYo%2B74v%2BzEmc0THfvWZNfurUiXP8nB6miYpdgELgmGPtuGpyjcR8tXo9XDHF1ruN2WxU8OKkMES2P%2FqkR9jhKVZiWfBay%2BklLyNZKs1IzCVjuuyuxQCCnB9EcEjQ%2FjwFoyNA5CyXxtauteE2r5bCGue&X-Amz-Signature=3148afb6d1f7501e03432a41f650f9b57639a2d751cc5050f4ebd865bbf33f99&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

