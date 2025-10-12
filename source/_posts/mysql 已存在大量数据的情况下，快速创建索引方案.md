---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VF32OY3B%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJIMEYCIQD66ChR81H1HCpFATNveJQUXIwuvOM0Q0imYfgCmAMk9wIhANXbFKM8tHTA4wTTjWWM%2BNGmh50zAfvEMV64nWgrMxMfKv8DCCUQABoMNjM3NDIzMTgzODA1IgwNe32jIFBL0wLmZVkq3AO639MWJfPfVbqw7WBB%2BxRozJuXcq6h%2F0wynlITeU78HhAUG4H02haTCPRNrNWU7Rl%2FksMsIKNjjAuD2bsVW5coy6zhW38ID7ljhRGLkQl%2FzJQy4sRInU0Yz7gvMDHRD09U94MHtBIfwcFexwzeo59bgElnK%2B6G2wOzDP5%2BRqQbqJWveULKuk51ptHOjoFHv%2FR5LLkhXoUcKgE%2BDNQjJYbSc44ikXdeH0z%2BSkBccOokffSxvMzXnhugU8YvpVDWFEksG4TJxCp3khdMjxNSmUQZi0yLk1Fg4V7meANfVIIo%2BbLOqUMzp9Y20dq3ETgsZDXnjvdWeQZTS4a7fDI0lvrB1%2BuHmFSbhaKUr65%2BwFIbzxpJV%2FESKCUMh7CDKzI65Fv%2FVjzyX14MHYtSS2zKnsx5pRX5kBJHBZcYueRM6ecSEwlOAbbMD%2B5i5L0DunKzLV9o0It2avBNPPs3qxfi%2BH7gxFKkzXsGOZ6VRRzQ7CsNPFdajDgLhuApCsa1tHPqEQKVgCy3eECxLWPU3bDgSiAmVareaMutXyXS2yPIJLk7R31olahkdPFS0Fk0JBbNjkG%2FVcVcyL5eCi2vzrQBUlt79q%2Fvw67g4B93DUJmQK6gLtTpEZAlPsllQ21JZTDOy6zHBjqkAcgJH2AP4ZGx2l5X2OQZBsw2vy46x3DA6b5fsWP1%2FnvAx4Od75b2NK2jdxszK7HRY65vQsJg5%2ByBneJv3EwDCJ2yi4HRIc9erduQuXSwUguFY8MIDcUTMEpf1yhSt6rokIWeAIMJeXr5ax2XYwyXO70O99oR44ylZP5QgQx8HlG90NqeMPao4I%2Bo7kgg3LkK1vLlFDJQbK9KqGkQHOjIMlIoxlYF&X-Amz-Signature=1d38c316a66c8086792962d27720ab3efb960d90abd4113d677190565c7f7990&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

