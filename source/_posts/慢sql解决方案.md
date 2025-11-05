---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664UYHAUIW%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T010043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3NNrKM%2BLXSmBXERpHP6z2MvenJCs2isbwCxJHk0jAJAIgUMfVgQUQSnmQ%2BXK6OlFl9aOpqyHyNKSAUx5W7rTUiOAq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDCYswQTq%2B%2FT1Ui%2FkACrcAzV6P2dxp3jVFTFVwuV%2FH3q0qbsPO0NvJuGmc9RHXs7nhh4936cExLbssaJPhM1%2BSMK6PVbBFgVCjpDdgw55tHpWbiDuAhv%2BhWhXH2M00D35qJpl1sqMFnbVqxlkjWN7eQHBJxKVx%2BOrXWnfMKDW%2F0bjsXw0FinboKeLSgi1pzMJqRzZx76zObNSOsYeW4aeOmq1xr6WAgfQ1iK8OvmMsSzB3X6rTjKa0jfW%2FSj4r7ocOnZUyLv%2B7sTFLLeMII5kCtd1mIOKvEVzQDzxcTJnVa5c5NBWrjhsrTKlJzPcK%2FD0CGRs9CTMNNwJ4PwFibmuAxOeIxTVAoaDYnB9Gp4Wh9TXTmHfSSP78jgDKqa2km%2BjkpB%2BhjsQYeorjGzLBcoW0BlvPjZZQHKR7FHGLZPx0uacQmyBvgU%2B9VLOQdZneqLm7Rs9t8vkHfTSQbmXuXgkowoMjmuxjXSUmSM14QpHQY4a7mqU3hZbKyveHwLT6wuPyohPAdvpRHxt03LOiYzO12qLeXPSnbphcv2uKY3NmvKX18MJ09OdCy7sZP8mX1g%2F9GH%2FsMopIefZoTV%2BlDzwOH10Bzebm%2BOTdp%2FZz4WASJHRJZnYLRqm8Wsdiz2N2pEH%2BYWfYT8J6fvNdH1wMOjnqcgGOqUBi%2BtNCBB9SRi5C1cdMc3gvGscIdY%2FrkjJLSFvv0WPeJ9u4YZwEFGEKsnLnuOu7%2BQTeQYn22x%2FXYgES%2FigR3H58ZKwWOTut5v32dOAmFvZ0gYL%2F5P78N6WyhTHZIjkLl6gD3IrNgn1OUTUnr4JdbaVPmKkZ6kRtYZNNaHFvjJUr0QRLzx7Ehwl0%2BYBK8HglMZFlXvMIu7rnbYA%2BBdroZWdLUMpgIhe&X-Amz-Signature=c236a0a5f7abb3732dbd9944ded81ab1e384fdf43ecd9aef7b6505940952f2f7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

