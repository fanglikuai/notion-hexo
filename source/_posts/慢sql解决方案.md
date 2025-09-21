---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EEUS7XV%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCzJ450AQHClT6dpcApp7U6QDHdDj8IoU9xd63BtY0pnwIgUBfCLZHKB0pjYUfmzbapgsQkEBsyAjbs2Q2uN%2BxhXqkq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDLpNpRu%2BJLeMn7CiZSrcA%2FFGnQgA4%2FtTuhCTmaVW9qPSNsfRWs61dJNdnIUoV0pc0AOD0U7bupiLNo115URzZ%2FclZdgV4g92iGxjFQ7r6jEHynz5dV35iumHWxh0bTTuEeGWluWVO4SQzaUHXve0gqt%2Boajzfh2Ykl94r2nB2CkeOyPpF%2FLzGm3mo4DFzO9bo767EXV6y%2Bd5uzJDFMFsKfdRkp%2BZYVXsVM%2FwutZ%2BurmOEWLYOs3Zlvgrb4T1E2h%2BhFqE8TTYwkQHW4B5iiP%2FbosGtqoIbwC2os6imAoQL1HcJEUceEVKpxsmcaFpffHO3QkLJ3FM2U90MvKgMDII%2BbqfkH8jmsru45GjtmH8T8V%2FZzsarvhx21DV6T6NO%2FXNeqPAXDpbkb%2FykcjXs44teFoIE9aBE6IuuPGF4DtNaSDIrXQuGxECzOyDUIVm3G5gd7ZlnKhLjFwzNKUnKDQgvy7QUJhbFAHlXOnkH2DKVVfT%2FVdhTDO8lxbj7oVI%2F%2BQZ2h43jfpyHWuHak7ibVrEG09Sn3db758dNpx259xBpQ%2FOiWT%2BVTmBah359gN3GVvPDOSYW6L278ZfuPnh7Yd9YYfj06uAHeRbmKyyQ1qbj%2B%2FJYwgyjj5MaTeT6nsCwv2EQ7fmcY4O6vNEBmO0MK2cv8YGOqUBDDpNtfSad13RLNrorvhNU%2Fv8mMuRv1OOjmQNDxJMm0oSMNF5rzeeIZoj8F%2FuP%2F61qHJvUkSTqCxQZisWYIk57v5OuwlqUgna2xkGyzQtXgKX4K2ktDfpJ1x0edvDscaWW%2FPRWhA968BoYqfHlZ9uqzTy6k3hVSGQRjtd65TEkYo8M8nqvoyIwzrLZAJ6Ov850V91pdud6Ee7cPp9cXXbfzcUBZw%2B&X-Amz-Signature=fa121208c769624d0f1ed8779e19a79ad09bcd6b54a7147b0d4529becb656e63&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

