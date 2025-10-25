---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667UO7XC7V%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHXlBVyM2gxTD6xtDnQbcre6u1eGuU5BwyaOBE7RilXrAiB5uUL%2BnNexNMMD25bsOvdfD9gexphrJ7lSJuAzzyOBhyr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMrxIgqnRwF0GH3EeoKtwDyAvV0gM1GFYbwUN17dO66z7Kur96kGDZHVdeUVGn2cypmb2zjokhwJLRlMcnvM9PD2zaa%2FLd0ovMBu1EroUdH9Iqs4SL%2BqqqnUZvJ7Hzl%2BAkMujQFME1zIPk7yKfUAJcF9cKMdUrguUVspoKBBJbpf%2FKB46Om0%2Bc0jFaIFKRd%2BYyq0ozXMm3s5GEyEZaR2u2MeD0YUL5eU2pmQMg69qNpaxfx8tWDPYRBghGLJfzhWLSJKbD7t%2BXKNbkyz1igr7ILN6nuzoQlkrhp%2Fb3u4TGKYJUxYBrrZC6QnTyfhACEPaf9p%2BCMDRumw7zcJFaAiwvZo2mjx1fzjxrkaNSu5U1ocqcuIySIxyfdB3BjJ4WI47miLOANzrBIXFULP3J2v%2BsoxdecEhkHGIDA8RqekcHd7f0sYC9d%2Bsx%2FHmwNt%2FJVEhFJhbGmgQhUr9cmZ3diGuHeJ85Y%2FDcMqH8gCMY0w5XBzkYeB0%2BzzBGGrve%2BDEkprLJq1ATaxbgw5OCeVAIPmhQnF6fg0NlQjjVFOoyfIiEdGbdg4o%2BY3efkDCqgqhj7xFb7tI2MjjBq%2BdE0mrV%2BnZ%2F4o2Ofpz5%2FNOzPUWG6G4oic50tqzj3hpyMqIK6SVwd2nB5F%2FAFOzBTBsrBdYwxdfyxwY6pgHLemj5Zd4t6pqsCeqoMJiEEk7wfBdwEDcEzzUDWFHfySByXMss%2Frl%2Fl55NAJDT%2Bj6wPqek0HXcfXbR42ct03lmBiUYn%2FbBKVRgrbhWTJmB3KSaKgqajqfkWHSg5MfHdaO8S1u%2F69%2BbXkn0OZosz7rKIDv64DiYKVHNOJyPeZfjQqYKAFHx5lkMG2A2K9aGDRej4zYq1WcAeR9VaYHq1fLcXpKLZsR1&X-Amz-Signature=7a657fbb5a677a476a27f3c67b2236aca23746938448c20723b4ee083290c208&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

