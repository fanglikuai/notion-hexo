---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z74PLTBF%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T170050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBDzKdxaC0ZuLBsKltGi61zzCyZ7s681uEjIJT0uKHSrAiEA6ijtly%2Bu8L9noCXGFU68Uf9dU0LTCXQUhfX%2B0qOyvz4qiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHMjTv8AFgyivebSACrcA3uWNjTzBMCC0OLPssS%2FiKagyBQs13crB9qW0RAK%2BH850QMnSVFetCGGOlD%2FYUbGsHw6sTROL4CHi2IH%2BIYE4R7xpfsUvAv7h9LT89tZBrfQ6aVUvL4vT40fLnvx4r69p9bNZXh8X%2FZJy5YCMpoTQ6wSNngl4SJoCF%2FuhDS6B%2B2pjDgK2nUY%2FS6wersFFdAabZ%2FQ%2FkcNIlvz%2BOfVxUlNeCLMUHMYOejJ4OdFH4FGq8DdQqszwVtGbJ75GJlM0hSwK3%2BnFd%2BICfrZWtROvOVVbtwYhS1xerrFbBrNZbHbETDgLnJHVUpvPfl8u5tctHTVEw505ZPgHT5Y%2FMTG8wLxorTkchi%2F1T6%2F2ufFQyAUw59k6xhueZFiEDBnWuVbMy8SgQrCFH6lcVjghCKka2oJ0f%2BIag7jRvuk61QSrpMh9%2FP8NyGyYvE3UkfnqC%2BrwOG6ZkQGaL%2FEb3LpvSJ3R8uTgnGemVu6Krv6yDnpoHBwbmzZC3ik8r5jwV6I%2BhmVYSdew2BdW55n4quZitNJyqlJigLgfU0o54ziG7Ixfdyqik2g%2BLeaGpCRfuQL2ZjVlSbGLxdC7HMeAQqL%2FmNo%2FpdQeJHaA5TO4hWdUzsBWUP5tc%2BqdjLMtY2laoYynFOkMJXFuMgGOqUBmb0U3l1fD4M80X7Wa9tiaQgZS%2Btl30x3YunZ0Vp8JFjx5qXufSjWxacx2FJ22%2FNMYCmZscvoq2rU1z37Ds%2Fd1ky%2BcZqHhSJp16PjwZlZuly6o8UXMxVohqLnTc3rJDeAvmJ%2FLuV4%2Bdy%2FjRiyFNECia8Jn0%2BT5amiI6kyfwIZtJ3IMz0kLIFuyVbs4WOKMlcF3UTzI7DZM56cYIgIt%2FuXQGrSbfRv&X-Amz-Signature=a55a24fed53c89a33a413553ba9767c77eb8073865e010cfd4cb37d896597d00&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

