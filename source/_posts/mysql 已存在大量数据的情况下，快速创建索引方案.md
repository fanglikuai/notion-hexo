---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TW4LCSM5%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHnNc5Y9vLHRiHAHyrUN3Oo3owgAEuuQsWfekUzu2zxMAiEA45i9CFOGIE0E%2F7IQhvJi6e5obM4TIogZ9h2ihpwKE8Qq%2FwMIcxAAGgw2Mzc0MjMxODM4MDUiDAFY%2B5MJTJMLkam2bCrcAxDvumrow51LOU8CzQ3dVQ73iOvyXJbrAGH37fWddzYy2QGW6YADMJqppXvqtKKHHvSiMHPqngHgq8TIhSEQhM5QfgWrWoElwdFbqk84xk6nN6DbQuHRC%2F5l%2FMq0uRtRCsvtJyRv5v8hg0cZkVCvEg6TegWp%2BOfUWMwUi79BG1%2BSZHMmv4rQ9d1xGLlUCNQtktXswFBWlBD480%2Fh9QmE1cWrfyqcX6%2BBVunwaMc9nFEi7yMa4PStxEL1WUCdEXjwMzpFOU6J7Z8%2BwVYjMK2sC8EaDCHyithEXEpuGMOXSm0vV5eEPJHmfN1m%2FnjGGv2AQyYwkMvZ8EvhTBypzUKXCEnwVmOSAnDWYOBKTLfygxYA%2FRzkHa21WN9D%2BRCRno8x8tT66qESVSLvpGpy0AHZjQfl3w01HTW1Jq2VwBcmOfybgEKjg6R9dlsCHx%2BnAywi%2FhKTXUOh6CSOJQ7YQmbwbo26Kc92Z8opAYQep1sVxpxaHMtHpiqCQkXy%2BJVxZtYTT%2FAeg%2B78JFEgqo1vpyFRFFbvqi88VL6SGXJRnSSUK56zYYgpD2HldgqW9rnw6WUk2MjKECc59%2FV4%2Fo5wdJytA%2FAHo9klCYCyPnGpvbng6GQQqJmgmI4lr8nwAqiCMJGu38gGOqUBSvPr4q2mqEa6md5PrOBxngE6aK3G7Cik6%2FB%2B%2ByaEkJ8nhTv1dgEf4iKrfQYSV1tfkvCs%2F%2FnSpy86ScpS1DAgvdPRhTNVHaUfWzmMzDmgyhIi8WLeIFsr6o0xRdYLTnjIg2FAo0uGUBZIU6AmsG3UU58roPoVn9a8L20b4Exs8ev0xS%2FbCmvk2K7FqumdJD%2FL2jhxD9jxk699A9qePvvQbnZEKuHL&X-Amz-Signature=24e2165cc6b2d365c9f9fec720091167ee4ea0c0da2e3ea23897547fa4d18aab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

