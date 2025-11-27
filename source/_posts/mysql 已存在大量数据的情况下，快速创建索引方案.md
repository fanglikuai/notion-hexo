---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TBLEU2QE%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC6AE70oyVncerAJFH1A%2BhrCSay8tdT5bDm7wD1YclzEQIgXTfw%2BiTYKLOrlUZTZ4Ml9WeCLllIetV3kWnQgDGlQToqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMEIkycKS1xQRrJp%2ByrcA5dBxJfnf4TM1ansLkjKgOTWwvZ7bpUHaAfMf7gHoElmouFI9tZSkvg3849%2FvJoibfvWeVcCwUzeLLgZ6W6JSfwLtrz62h%2FxEpBVJJ%2Fvz7VF0NdoP3b0%2BqILYdHyOlw%2BNYg9FCpQGIr0GQFuSPNGLJnDFT8H0f%2FN0aFKNtU9oJzxKAHiE0AYNDUBr4spaUy4mEzNc66UAOFwsAVb6VU17axaUG62Y2bWKmjX2PLxolC0Yybi9HEpyYgFLm0sXpk2i3CjA5ftiYKpntVwjt%2Fm0pPSoHoCQapkSQkkr6m%2Bo1kWtVUU6rbnEHYyj7obDuRCnQY4JnliULU1n%2BtpaQxT11fyMy0%2BRG%2FgYlIFYD1xih5CkOQ7ZqzG7dEv%2F4stoV4V2EAB%2FsPlfDr7ZEIhK2bm3zZ6M7fGjzXyZusDwcpjL%2FwSSP2V2dOlhM4KipMBUAbWo73tfJOZZ40H%2FB%2FGWCMkcbqT%2F2jbWc%2BM72b%2BbwemNT4U2KBYtqghWHd8mz1Y%2F%2FhkKL0xZeBrsRbqkBVYqsLClqdSFGPpKFfcmks51d71MGO5Ea9SorZ4SMMiBHidiELwuEYtIgrSQ1q1uoy2%2BaXhu%2F7TaLqrfrLSw3EMvokVkA0RPMQzlapPPBUyhrpkMJOiockGOqUBil6O8OL9I0IprE0CB4VEnct52raWez1Xq%2BOVzLEgze2ii87CpCD1tH18%2B%2F1%2F0u%2BWQzF5%2BualuF6uB2wPrc8u1ioZtRaFldmRamhEzYLxFGjHDC0%2F%2FPrddkOd0HnZy8%2FwJkPaXd5N75PmxviJdacHr%2B3sNv6YXRgVydAnB8VcIzNTJ4cvlyHoDcbYc6UZgSUndlhD9W%2Fk9M78H4lLjNmfLn6p8FYR&X-Amz-Signature=3e11c1081a3dcfc49a324c0b7084fe4a63ae30cf518d38aad3ce796c70bb016f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

