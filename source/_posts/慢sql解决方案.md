---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SLPEH66L%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T070106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDprYvx59Vj7b0gaZMq4ENc3IvtO8LZBucHVU4brKhihAiEA9UaN3uvYriIuyBDv4xyQBSqABWOv9Wd8RVSjHwpEfdgq%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDArfzUXnrc5WTgUlRSrcA3t0d06EDwuOTCscTRfdeY4D2WkAb5whjOH%2FlXQ%2B8ydtKoF7nmtjmBdlvudVczA5g%2FM%2FnaGMeLjukpaPx6HWy01Sp2yrmk8%2BDIzaatCVpg%2BvvyGtJKE1OoUzqXAnHfX3MsHZlU3kRRT%2BCTierY4%2FNds98BVnvvoImc%2B0hOxgl4ImYbJtWYhqvmUxpyNGbIOIXDG4CKLfeWj7BS%2FN8R2TXcFpD5n4IW6NAEKJoZKPIfLXoX2CTep%2By1QiDoOSXHf2bSIxoOEnZKlA8lJgv6s5xnquLOgAi4rj6vJXtP8pQNKEresUwtTpfdP1PhwcFOah7a7nbhHwA%2FMXjj0dvysYlgnqqMp0XHGWtaK7iZRLtwo%2BU54Dv5obxNJl4AeLcNlODRztrjRALfwWV%2BMtD54rFbQyuK2PvLKzlNuI2ZQNbaUCDfvCJE5t7qaxba%2FQTlvla1xttEISzIwQWzSIEI8IFvuiXLj6o1rCgnTvRJ5wh46DS8FXbDzi7N9rk7q2fYO7pk4FcmYBJTiAPVwBNTv0b7YzE5LG%2FV4Q98PWOWdLM6p3SJC4ouTDo5evMHiHs5mf6tiVNJukwFQiIlXTLOylKR93PtvGKcArEUyiEkFujveEXyeDAi2rlub56Sv3MPvq8ccGOqUBleANX2a%2FHTqWg4Qe9sVRKMLFYisMzyWpnE%2Faf8PLQ6RbhhQxV5VUuV%2FOlNKr%2FI9%2F8wnf4f5kpbw9r%2FdQ0FCBPENBfHEZEk2bAfW5nDkQz4jjM%2B3SO%2B8VlG3YCdziYhzxIOYZg8sio94ACzRWH8uSVngGL%2BE3373uNvOK1Ft7c4lv7OPO6et%2FgH%2BkbqvBXajR6pRpW9k32itrz9TzJ%2FFkoOmf1WLa&X-Amz-Signature=de282e9c582896cd02d119e197f9eefeedca9fea40572c8371765149c64ce465&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

