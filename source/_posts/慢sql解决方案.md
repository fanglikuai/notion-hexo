---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQOQVNYE%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T130101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDKVMwOUQn3Zm6SOCfy2RN%2FwzCK8h46HVYi07yascdFewIgcOZPjV8d%2FfWm6oVgnOn4l2iiuz5Wgi3BbPElLCy6%2BgMq%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDNNn%2FABtxhOeZzxUGyrcA4LJ1ToErrABMQScaIfbGqEUV%2FEMZR%2F87%2B5Gdq3jApjaXleqy84E1NReN5xKKuhg%2Bj8ZJXmYf8B0mjELRz5QOg%2F7fteT5lFwLXTYHedCbHXZdRpefdQP1t%2FgudJZ3MN%2BmVCxlEHIxEfAGx%2B6Q0OaCM3ZDJI5zm%2BrtYxZPGIgqLaOA1rHEULlBtEBH8ByIQaBRBREiVehOibRdg5xn2NsWNDFRqd0qbfuTGiLFIW84%2F8ourO5Q4g%2B9RNf5p%2FFxoYzfCRWLOWAeeJKz56pgSpu5AVmDjk7RBZxwF5b6G0doeooytIrZHcMPt6UNq4O69jIXXot16Om6Ki9kJPu%2B2c1hW6S88sU%2B%2Fc7kuWXHMZWldBJNDJHGh%2F2jUj8vZf5fRvM7b1%2BnuXHWFRA4iIizXGZ4DaZltO%2FUoB8IU0C5pS3Y%2BQFRl5F1JU%2FAfvFw51GTFqGy64BqklChr73JU3AR0LdOaPaR5Ag9xX7qxNFoqPZ3KwRnufP%2FOIM90v%2FujsnPDoeTINFsBEHSignSKGxV%2BMI1Xej2jQ6rAm1Z8Dav%2F7DKjWzNu1BFtNk%2BYzI2ZBMypeZi9ct%2B8mX9h407dRBDbS%2BXHfpeeY7jv4R2gsVgUOhPWWWK0bOJZb2WOXPemghMK%2BNxcYGOqUB5ni%2BxGK8K3Oz64u4tVTkuaQOYR4tcYKkz%2BluXEEbwTYr5QPSsJa9tE6%2FQgzDC470NJOjpNA4mgu8kjBfpNPDCBF1szB2uMwKREaJVONsEk4hYUa5UZnl8UdO3y5lf672lYF8eiDXBs2EwyI3HmFLp9YTeeCAcXGLU2ml884WD7nNRiVGmy2vWapaEpeBbgwe004%2Bnh7aD2YJpt6a3j4y0dknvEoM&X-Amz-Signature=b43898e3d84da6884cb0856ae60e468f0de88991cd20d858fd3251fbb7ae9eec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

