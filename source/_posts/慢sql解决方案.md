---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RSATOVMT%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDxCbxXONySD9sDGpAiD6c5wU9xn2gKZ3Ys5AMWSukFqAIhAI5CUGOKlWvFixH0CKX3MYv%2FboYMycBMg9IIDTmtA%2BCIKv8DCEwQABoMNjM3NDIzMTgzODA1IgwgkrCzhi%2F4bVV57cMq3AOCzHM%2FXCrfHtH3rGPyRcC0AchHkfOqcnq4P49H8R7XYFJIpMfEb%2Ferrp8CA6tz4GooKCc6dxMwYMuihR2N%2FAbl9HT9DRihy0%2BNwdvjDUrHrbPryR1w9IK16nhBPBnyt3%2BWCBZi2BNGptN5ctyfgixTAk6GVBzMkQZNg7snmbj4OiIrSetUzQRmqt3ssrL36mt%2Bt1dRLQ38EV65sW%2FqdXGd7wDuOoq0%2Baxg4lltEPou6AxHixi%2FAxr6qgILZEwGQ8bG%2FdMnv5G0QgXoHHbqBBc%2FKln%2FJonRv2LEZ9hPEFbqOgapXUtdDz31JDOf0Xjpm0jv9wbBpR2MMQDG%2FDEEizfj4nnpqxFcQGFith0732SNhl2m8tocwEhUBbdFu11TVbvDT8qBrxcVuXz1qRxnRQAJSrQO4qeS1RNZ0ze5C5x3rbPqE83iWOjXmB1Qq1%2BNc8XzOM%2FJIM%2Bm69Fj4RNY3XFkyNaRNsmdjSO6pI3p1hoyNoK3eciPOR7qA7CBVOHtSHSBJSUi6gOsezOx%2BYVakqlKNVH%2FzYD0adG6nrU2C6LZxlXvZwEqJU3hIIQokoN%2FSq0JCruRH9IFAiBFwMi7QeJ6A%2B%2BvnHiTgRgvP0IpwEJ8IZBl8zrxRZFyftxIjjCl3Z7IBjqkAVF5SPTotNK8sxJzMz235mpnS5YNv45Jzv9WN1dPf32AfYVJBeSU2kb%2Fq%2Ff8GY8KlHT9fYdRD7G2C47bCowtz%2BVbja0BartMrjBr%2F1G65vxY2gfYxqzp4XqDOup9p2PmGjbzXTOC8UxbtuW8bmPQDRISpRluixR74QSMsiwcg9w1lhtm9I%2FpDYNKKcBxpuR4yLMqnrblkDgWxh2T%2BIK1zAoMUqnh&X-Amz-Signature=2a9c39e5a8538dd550bd32f4bb01fe18c724f77f322cedee0da42c70f669bf75&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

