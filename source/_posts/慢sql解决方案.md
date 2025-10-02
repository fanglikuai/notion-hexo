---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UAODBNLR%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAgYJEJlt2x5IXl0JHWteCrdmnT9Lfh9iRlpIqj83LuQAiEA%2Bnrci0F%2BXAOuZ1e%2BD3Wjl1gs58AD5bDWHoUDW2T0tm4q%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDHPtf4Xn%2FBuFBxgfwircA%2BVYk4VAHeMgzoKaBEIJmjIh7%2FeygK5SSWr9OVqxBPhuQtFidGGV348tSKxA5TaMIWM7s9ZfZYwC%2BEUHkJaNF20hQUO5D5qeHkWX4zbIrKOlBY2VH9j5VsPtDva2mhgOYPkshKihZlOTBvpVvJqlhTbM9PngeOaJ9LykNmbVL9vqyi2DhJHv9rRx4ybNuAiFMXqNBG17jTvtkWFqohNzkbdYp1hhpZh2A2%2F22%2FEKXoEeaTXhqNsV0BnyD%2B4QO7BCevNWDGcMvPX2XACfzfpNoqSf%2BvXvktzmvyulKyjoO1v%2BVAx0PMhJTZ3R9r3kv4TfgUrddexSNc41Ca6i9RrKdPWXkLdziUdragMwwZGLSCnkVYn6yFuCn8tOUqrT6Etl432MEHuCx7T6C%2FhNcHEBKkL47MG573cGUFFXpIIHRwpNFtB9VuobJBz0pkGw9LgP8bbviGz2pmc9zYE5RmR3b7thAKVra46HTJVb02FZX%2B9i2tCd%2FYg65ZYTjFehaVWuzmrWyxVVmMnPOfflaDaotXa%2FFqmhZVpf0lFFK96M60BwKqiE6Duhc7GlDUyOPhzkqHZogNx14RDxkZu8SObH25DOpr77oXf28MzWRHk5yeemISoKKJ%2FVAWCM67S0MIOy%2B8YGOqUBZds6d6akE447NDmGLetn3ygQnptK%2FN8FHiEJGlsCWMY8j%2FoJRGM3dOfMMp%2BBbfqQZLhKuvr%2FkFHWjF%2BOnpWgKLlbPANnMKPqVnwsH4ujk1eVdo%2BagwpaUo%2Bbn0xku4mEz9v1TJOLfHgngf7M9DWCDmpRdv1E47PlF6lwMZQcJ3%2FNCsXY53wVeIPBry%2F%2FQCH5G%2Buz9kuUEdAZgXsuKY94NIX2qbHJ&X-Amz-Signature=46ef2c682eded90ca09f0ab53d13b81179607ff507da79755199e84b457bfd63&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

