---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TM2EOCV3%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDXrBBN19h%2Fg1U6SNpOA%2B0%2F6i1VZAzrbdZ2fYvhq1ahngIgD8w0Wlll9YQijW%2BURp1S4eV6lILKU7Bg4UhhdZfV770q%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDFRkm0CIRvXN0OhIvircA6hyNn92wizzhK4WfpzNo4ax5UeIuUtI15ZXEOZIRWc4%2BUgO%2FNWZI%2BBGvDMWgqIUFgHDxg2SPGLekklovoDO2ABKR0uPscapulPGvCoQtEEL0nodD0nvJESRwD8FSW%2BiB1WNBt%2FPz0klcaEOfcrsOBnULrYnwiJ2cra1LZ4TnIy%2FS3u9PTCTCFud%2FyfeQVsXgcnOIM0AMyVs1WC3GHFgEQnKOCk59xZzOc5MuSnjd1e2xU9QVApKcr9j3dmTaNDGkSPZSWr9NwyZZR3BxlVjtZhwLt8OISOBi2ui0bCYQKXlNtvmcVyVdy1JiqVSs0%2Fr3B1F6zfkRFp%2FhFewos1kOJR4%2F0gNZCFETcl%2BYELataxBtRfw2DxmD7uCVv2EUpYIuSQKYT8%2Fzf8bzKeF1aadhJElvdOAh%2FOYLiZhUuXItUPmVBJuhso%2BUYdSHDJgTyBGRJR0nw4RxlM9ffRY93B6EWIFeIFDLO0hVQhq4JtfdymJDxYcv1IhO0eyD70%2B%2B7mPZwKl4IfWnUgW3Dg40uWYdgrnpBgIuCVb3%2BypCJQA82%2FTQWFK9onMoJ2kp%2Figx%2FOQPeyMLtWTOBc%2BOfjZaUfwu2ynA5NYa9gN1GoTcFx%2FwcnoouGdYKwTWcijYm7GMLyzz8YGOqUBRJfeJedOXpHp1puwIYAzRmZWETl5DrdCn%2BXbpkDgVqkB7nQeL%2F2uzVnK%2FEgPUzo%2FFoguKbZ%2B0cUpr4VplCNHcxC%2BVd9ogzyKiTwXj70zsaQZVV4AVC%2FAVaADhzPbZ7lZbuOAJEe7xf3eSRd%2F7dQ7FWOfT1cu2jUvnIsY70Xgn85neUXojrCE%2BOrdwaCOT3OapmyyfO1RwlRYtpWqeKMDvMPOLRDD&X-Amz-Signature=b78b6ab30a59c8d9bd32f18385d025f814be7dd99cd49ed1767705ac15313b2c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

