---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCQESK66%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQDtKNJdS%2FEkQg6PqUDjnoeN46lLHTv0dDYBeOEwCr%2FMQQIgLQtuR1wVc1R8ZM2FNfqkzbkIy306q89TSptau%2Fm6o70qiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD1Tr47Z6sVrj%2F5CxSrcA%2BJqPeo49hZFow6kTSwgoqQuv%2BrK8l3T9uPW7K4PGAUwth81uYfekuGGKsdG42AwY%2FPVu3jWHx%2BjaocHku%2BYYi8YnArWtBWMWzMVt5fwvIAF7tkqg0vfmyK6Ga%2BOOqCzzfLGP7AIY3XueKK%2FGu%2BFFbjiD6ArV5PCUs2Bi8UWmVlEzWiulPRdmVtO7H35Lz0jAKbj9SMmvPdoxb9O%2B1BJda3ZA1I9T7IGECqhW%2ByoHPoozlq1TKVvVwLHiUz1GRiAJHSsXZL%2FJndZdAQ6BlFbMcZJL7%2FspCrIN%2BbtNdp8sz7Ee3Ikl7ZmuQhxa7pXEtOpoissDenXUNrk44DB6TfjQxcPfn0lRPbmiz7yLkXplRWNdiwoWbsTohN1wBgySj7en6Y%2BSwBtp45jwI2ryXU0zEP4Wc5mJhiWuOGJvwpkfTFxI2aTO4O%2FHeCDLuYQVkOv44iMeeSKtsaieHc9F13dHWUNNKE6gmBxpFGUlQph1kFpCnmoPVJUzyH5xz3jDbuNsy2XBt6iZGqa1S2f%2FUwZSkDOJcuAbiyYEmMv53CTulqITPVSbFh0cp4sxllXDAiRXOixEof8MQZh%2BxaDIEbqWjE9e1t1vszBG4uMCBJGxOJaozCCrDZDN%2FG1%2Fz7%2FMJXuv8gGOqUBg70e%2F6EF0%2FEQ%2Bu3wKeHV09wGi8kGtYuov%2Fs2eBkOu8HuPnoFqKvqBf7Y%2Bzi3r%2FHaCZ%2BVCz8vA0mTVAubhHgtkELU1X%2FTF3NwaFyNTapoav9uqqVbN8SvG%2FM76hh3ZDYtM%2Fd3kA%2FLSpiAZhUF5ljIijb%2FgBAKLV1RAC%2FQG%2F%2FEgtq0TawFbmw%2B3r5KXJ0CnpvRpI0OE0eNuJt%2B0VLNJlLDiAUkdG9p&X-Amz-Signature=703d859f3111fba54093eaa8c63a7a890f0d94cbdfe57c83159905737b8538cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

