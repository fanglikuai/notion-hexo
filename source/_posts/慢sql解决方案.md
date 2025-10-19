---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NYNPVCI%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJGMEQCIHYMJINH5NVIY%2BoUIZiRl5UnM%2FdlVH4t2kd1QuiBGE3oAiBApNgZi0tLZY%2BeUd9lJOHCWU6%2FeySKZNJvsAE2yKRMLSqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2vYbYW6QAn1y8xzDKtwDenZMwk3eARnt%2BLVU%2FBNpqADAJy7TRtk0slvp3MKjblFPI62Nzo9usKIDAz5UlDDh06A%2B0avILaYHDES6omjFaScVSleJzoHUSLJtPn4gKRrHnKGA%2Br0DYf3A8Y3%2FRnKYQNmNEnnglQ8CeQ0G7GMRiiahN9dRGxqsxbxCg8tEgIcpcCQDwqoguw51Y2hP5R01b%2FgO9a2FOIxFHFud%2BPVDcWf21uWWx9dogsOTUBYOnxSBA%2BN1azn7jzLmkFXBCmGR24MaGCQgpL6EU34fOTD%2F5xOiy2KmDR9aP5rYYc3uoU5p3wZ7XVLJjWbqu8R2kmzwlrmY%2FG6DnG%2BG%2B9bhRqMud1p%2FiJ0Pbxm7cWr5xpdUGgJIunRV6jo%2Ftow2%2B1xFpGxsvqG48rQ4oKsDFrt%2BKRnzLI9NJoIU%2BeeGUpGpj6K05amtLxEZxu99JFo%2Bqj7CM%2BeNoq1V9VRiU9a%2Bb7TtZsk5JLY6Ohn5ajqgkYw5U9c6fTU0I%2Bnij0Rzsdih7SJyvErlqwfJWjced5%2Bkhw3zp%2BGXiTxAk2joGKcNI%2B57QhOPp4BDECAoSY8DCwXDnaet8%2BTqdX%2FAHcrJQ69EevWSk7IaiQf8Q9HNhsozVNhywu75rtJI%2BGJFoACCt7X4aJIw2tbUxwY6pgFXsg9RlG0U3xF9fnBJ0Ph%2BchKkK0jZ5P%2BhG%2FNyDt0MpWYuMssakX9j2Cw1YnkoHPaU0vC6LeOGCIykCI%2BToN3RgaLuo9Paq9C7Ne0wRE1BEnlIg1WK9fa%2B7LMN7YqnSGbLn4lGaB1VKEEMaPd8aoz%2FUGBxhquDsLebHedFSrNfw%2BVaZIffcyuJ3tD46t6DDVcZb15sxu1ALDucYu3LxVfJaDA%2FAKd0&X-Amz-Signature=cd450bd19221977776265b5a9b6292a2642818b2097653ab97522f805190fceb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

