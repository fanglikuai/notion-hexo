---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SI6KBR2N%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T070040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIGmgbueBVh3QgH%2BpU4tN70BDdlnCxpJ3JQNekWh0j7YuAiEAio8SZrVxJUrIyWT9LTmfOEsfIVZrI0taFZCRfTIimkUqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPrUIgtpxABY0gfZkSrcA6402w1BClLfo0NTobTBWrlSSdvsMk2lISUwZB8K69llalWvypmTZPfQotJGAto%2FlO%2FqeJqUd8BVFlNGES23umVjjyj4RBC1mTJJEJc7JojG2AHKxdn7IlCvdReQhvQKDQV%2FAud9VlpDH8wH574VfZ5gr2dCClRVSjB1mpPkgTeNkR5oWAR32vkS6Q4ycodVxywhDMINc%2FVgZMKsTedPSXfJjC41bfxlrxy5HLZKhtoLJoTbZ1N6jsoT16dDQ3zRob5D1zYH9nAESRBzwTDrM%2BmMqBbaHon%2FPX7NwSEaO7xbZgY5k%2Buhd0pV9FVlgBOeRYPVrjvt0XkhdcTKDSSWgMbpjgZiP%2BV06uAZqYoj7Z3CNapUfSa193G6oZiPlr6Jfyh4avFeS9kLdfx3%2B2%2B%2FN4jkORl2VC5I37CNA0gVmjQZlXOm%2FJKNFGJyDfKJCb8cLMbo%2BqsTVJ1%2BpWNCJYTi1zaAW5GuDEWgrFN62xNnHVCDia0Xwn5CsGH46aGKy6QN04kLHiXXNyLpCEQH7XVIl8a0c6E560%2FTz1lGrfImombBIN6GseaQD8cqU1%2Brd6eSYvNEyMkEzA%2BW6maJQj8Bpw3g2OUVHuXrqOtdSUCo9DS7%2BkQWqcp%2BENZnDtahMNea4sYGOqUBOQ5Os%2BuSMOpMd9%2Ff5%2BsDKD9ogB0R3I0mkDP0He5oxyikzG0e39zGFVCWvgFQNDH3ZwuRlIWPGnuX4tCX9e%2Bk98aOjwgCQYtJaaCF%2BjRXxT%2Bv54RTBgOF8MH%2F4n%2FY4tUXNe7GGvoSGn%2Bfl05%2Bx3HJgciNUOL6sAanfX7SJ7MvpkE8OufkPGPqHeue%2B86qPUIJ5ykSJJFzKN2KmA2iWbzneG6SztQs&X-Amz-Signature=afebaf2f644b16f91c021810db95f84ee88948b099bcad360c7bd90909e858ce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

