---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EBLQOYT%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T140044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCJ6tL1QI8jf0rAFOCVahXeLpJbLIfEyzDLgUf4e8JmlgIgLfkadmb1f0ONqwakfwZyVR0nyf%2F617kIzPdOddOFXK4q%2FwMIRhAAGgw2Mzc0MjMxODM4MDUiDGN%2FWRzb0ssqUwdJmircA7%2FpFtman7k2kTSPW7r2SMfn98Ol5yVp64WEtH3TY8zXnau%2FNxUTJcMsf5LfWdyNj%2FpooNq5NSK9Q7tgIWUifWkMNXynMP2g3r%2Ftb40yECBsCzpao4CXGxhJKZ6wer6gOMEgaaR0VnGcO5n8E%2BffRq9CNZPLH9jBwQLPLy0NXN1pYyMn4aPXRKQ6b%2FGXHr%2FyXbSPcHWpVMBsc4wcqhISOX%2BcdAfbPySVjb7eUYijnPXK23RHurzwzC%2B4d6Cm4dKmBfHhrZxGCRjMEFks7wLEHez%2B3zCYsCkccGOOTuNeUoiDiPzJ0Zyt9jSpxWovaRVEQ6h%2BiE%2BvRGei05IDjq%2F6aUdoCq6zBdTeLxbPn2FFZWoRSY3r%2Fsz5X%2Bw4xg32hCZa9fK8y8r3zV%2FF3zRTFSqBcZtwQ9jnJlDgsRE%2BTqLU%2BW45PUF%2B%2BqJsjNEQNeTMwtKXoiQv%2FEnWDZ7rjdo8e2Sff8v3H%2F%2BmbIiHa3panec9yawc9r%2BlOk3xb7U7eBIXI19RvhPQZtlug6R7dMSMD%2B2UdJ8jUrKSl66rMTTDsaEWVXFl1NAV1hLr9x3gmxOkt52Khx3AP3%2B2Wjiu2cQNUY1ks5b8S40djKUFHPWUd%2ByWb9tIcSCENRRq0%2BklNrqCMJeyysYGOqUBcKJ5ERHWlV1kcFkn28qJZ0nPOhj5dalJVxa5%2BNyG6X6vcQrf1N114E%2F9L0VF4hVLN2qluPPfVdHY7nFnGl3bY8OKkk8B8bBfG2FvgucWVQs4bYujFnz43C6msLScbFIdLM1aUPm2BSJOhI5sJnC9Q7hEr2%2F8gQNQ8wl1Yz2BJqs%2FvrWAwmh5OQN6VRe1r%2BSkrS65OlCKvfyRWK9nYVMRBupvXMRn&X-Amz-Signature=c3b8f07b4acf66cde76ad97929de180393289c65557f922cf7d4c34920d8be0f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

