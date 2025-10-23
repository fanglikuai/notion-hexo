---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y4BG73BG%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T020049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDEIPDxYdjBzDYf93c0DTunpdySXchD9KfZNs6kFlIuEgIgagwa5U9zaaYUEgEACzsG78x6pZn0UjpqDQD3WeahkEgq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDL5chW7FiPeejZ151SrcA4zWSYzRAC9A5dACtbG4%2FNHE9ELtkx%2BJH3Q0ntv8WKcgRt4MCS6dSOkt8yu74UY7KsMQPK7dq9kq%2BCySTcjmmJK8d9hgjeM9YPWyR4JS6t77J23tHM1ZFZwcFJuO%2F2EAvned9PMsqmESvGFJq0G7XBsCk9ZYmWV3Em0jVbE7Hh87yNtx6gj6WSc%2FGES%2B0MCSl%2FOZfjECbObZL6Vu2%2FVyiEvnbiF5e4LeA%2FOX8XNi6EyxiwXQlOItZTyLCPsmV%2F8oY6MDAg0pFGKZ%2FsG0xxMTWf7jNfbgkZJy8mw3GftHImNcmdC8JHvTpJ5unaMgDB5KZuCbQZlqAA%2BePhtGgm5agqIg59cH5DEty%2FZVtEo5EZi%2BhsS0UMalaps3ZWWbCDG0OHhz7mb%2FFLw4RhxTxcmiS%2FMow%2FX24Z%2FZwtATNayNtKTzyrbPeY8oMf%2BcWO4ZBCphESSAApbbIB0qpnybhOuEbRMkr6sUt%2FSU6e5DQ82PsgHfQoX2ScusniUMhy333kwVwzqlDZHtXD5abg%2F4FJVZlODzSsgl7nLSyDdEBMql3%2Br4qd9xO4UVNmk%2FuJz69NrsvU70SbkLx3TvF%2F76XxnTWsUCs%2FRPOUG18fgX8Ia3GpYHJW6dvxARx%2FrQ4H4xMMf85ccGOqUBSYgSTODlb%2BEhs865vDLtaYBuiLWHir0i4SO6ztMC4ku13OIFSRgKua2VSFp69Kp2qAUf%2B2WYHfdA7%2BCiQZ%2Be2B5kd7yqKqvfq1pdI8MLPPpylKRSuakzNV93SG6C59PjWjh2SS1BEAgKJ6ysGLLaUu0vjVt3IyfpeXtuKcvV9h7ovN6i6hnturiu%2FjcT1NZKfy2J%2F%2BmJLKikY%2FnQd%2BAI6a0gvflm&X-Amz-Signature=be1bb4348b7b5282c55ab5b6899f5254710bbb6085960c2ffc9eaadfdeb0cbb4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

