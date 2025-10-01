---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WUVMTZ7S%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T220048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH0ZRZaI0Gd97HjsuOaJRRN3UyqfTMudYj9MEIcsMYSoAiEA3tYNRzrDBqUK5Mml8j%2FdzgL7XCbT2%2B1VsZI3kR3gNawq%2FwMIHhAAGgw2Mzc0MjMxODM4MDUiDLXgnJyrpN7vua4vuyrcA4t7ubySPt50NDyn4YoOid%2F9Mi0VkgOP0Iv44R4FhdwvOFg5L9jWcwDinnvjlKmFUVonl3KZN3ycWoHFRB04TTV6D%2B8awi0rHkgsOtmO%2FlkySpV2YnNGcpoq4sPSUyR8qfFkyrG399NMSukWpBrKw1kaOWc6b3YR7YmreUrEJjbiFj%2BCCdFpJg%2FIquMUwT2hFJf15bPrCt%2FzxubrAcQ90MBMezTpHPGIWdW7oIvDbQ%2FamvUp8m5bNSaKCzbgMH7L6%2Fg%2FiRatAAYUwrmi%2Fs1BXYKDgL9iT1uzeWP6MITrmsLTxuejRH2FYGfhk3fcO%2FIO7m21JgILdIow%2B%2BXLpalOok31XSl6WeA4jcI7m4k6uFP%2F60uiwrUSDNKP3chww2ohg6XkumV8JR2JLQR2g5XzZDTCUelgloS4av2h9UkqiweTu0oiXYO%2FqEJhCp0NEoJR2iOg%2B1tikgGs7uaXLcJagscyv0qVdp9kkHAojOYtiKaHCSehP09sQS%2FtGLO07RWETsJq1G%2BBAZkqFH7IUfQYep79z05mGZQXxb4loZmom60mkgmxQxuF7AYniblNkDY1qgHZsny2%2FdCDreRjD%2FvbNajQ2XsHiaYODJfT38DfWPkupfVY4%2FiwvNRS092xMLOr9sYGOqUBny%2B%2FZ4IAkeyZkRLYg6GpXfcvhZx20n3n%2Bct8kjZ7UdXZ%2BNU7silZ3tCgEq9gsOdxF0%2FHecO%2BogsT2fZSb3md7e651qCtMjzjuvPgLSYqMU7%2BDs7KQ5ZIgLCVHUuDwDtzaZN9AeNGBS0E%2FDedJPMGAA%2FVG8%2FXMEGGJbq1atUTAyR5ksPD1%2Bx8mEtaJ4gQWQkaiQdvQn6nm8cPP%2Bhr2i61Wx%2FZmy17&X-Amz-Signature=aae4764aec7a676e9b3fcbbad2ea1f3a82da234f8e5da7ff3e0b5bb0297c9c6d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

