---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QBYUASQX%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T100058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCaVX%2Fi7fkxofto%2F99%2F7CCaA22rZ%2BT3g51ZrUlcYPePOgIgMkfjn%2FgaTrEhwwAI4Ww2mAGEYwITQkuHj4ofwEPWDF8q%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDLbWDJaL21ptxgSsvircA3HhALIq4SwrENfQCCpN69k3f4DVN%2F2ItvABQJwAGQ5zeUejIHN4DRLeDcJT75siaR3%2BsN%2FgIeT9HcEYAAOXrTwWB8heBJgxAGM0Q4mfj7w5tCAXYXPMzDYKXWTu%2F9o%2BFDFQoKMILhI1HpiuKG5X26i%2BzlpjaIq%2B7406CWgjI7W2iPbdSLlUlrbLAv%2FevRdt6C55nmB6uMWxldWQg3MB0EP%2BFwkEMecprmCgW5PLSGEMf1MZTQ6%2BsU37gr1PFyoqNTYvHZ4PUUwqGo09gqTO2V7FxOrPtgeDPDSNaa4RBdL9msZGlzQ%2FuaEEoy%2F78%2FjKsKJJBVgbW3z09WoAqvkEh81j7faoaLqGZs%2FuzyLS8Rn2kCpurutgtwLrJYZjYNSeqZ1mqkHuFA2B3I3yhgwXnOb3AvAtg6ZisLP54GVkUCUXBVmDfKml%2BjUYcbGoj%2BN0fQECzlyxdZNuFC%2FRtAFoqo%2FbCjtjjOFfn0ZHOxHPbSZAlT4%2BmHD2v5%2FB54klLpf5YfdRomoF6KlUrefxerDLJRBu4l9DdQssQL6GbyXRDgBmuSLS6uEFofqQeHq6ua%2Br91PIuUt9iYzamS5PiD17vpVF53b8b4hr%2B93tQU53%2BSRQ9yLMY8GjqvtbgarwMPzAkMkGOqUBlPevhsZtxuW8ifnzlvwwgkk%2BlivaJ80RrgwM7G4t%2FiTPaKAoVXBTOSJiMxAT3WwTfePYvE%2B2kLe80h1ngt30R7Qd0pqqSYClaYv0SQPxAEQDjSKs0AaaxrrQtAprfgq5fwKle1MAI%2FSqYBd7%2Bt41LBTLiDVVHbmc%2F%2FW3G8WbmS4IaCXHikUE8lQbJ9eYqmJXIKqAD3PN1JbIqyJqmT5CAr8V2jx8&X-Amz-Signature=31d62d6d7b114c949ed650edef32aa9da0f2c17e4a058f2b958e0897cbd9dabb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

