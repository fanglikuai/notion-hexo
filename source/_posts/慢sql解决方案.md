---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666POEOM7O%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T000039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJHMEUCIQDKNEkm9GK1qsxPH9%2BKxyQ8kDHq%2FML0sJAVpqsQpCXvrgIgegHkKp27j%2FI%2FF6VXa3W9bZowIkn4UJaQAqI96f3IVmgqiAQI4P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA%2BUblcoi2EMio7P2ircA0pf%2FbCl6oQbfSHbMz9t8GsR9RaeEO3HBUgHwSw1HD70pXbf5VYJqkRk1TDzIA8KTQLPAvCalN%2FIrm7l1XhAzZS1nJHVECb4Zg1gmJ741f4yMm8siIhvKMZgjAWESEM3yMZTZuqHRhgias9IZ3yRdkYwAmysxAdaNsVhMPeD7XljVE3Vau4a2b6OzHA8KblJyjcBECUb%2BtbqZGK5nKZUDmzdXuVWMQu%2BShbEoFl3EhMb9mTj30PEXmMcMRcX7fKDtytlxKM1qONQz%2BTTp12oiDT60DX7ceNr6hRIFPUuv58iLFg9%2FoH%2BaHeqzOszWxAxKUOVJ2dof%2FZqc2XeCGVMouINMvZRq3LHdl4ENyD%2FMGI55WcpiUkTkMkraACuliHlBGXK%2B88jPd0J3AMmP8hHqReuSf%2B2pJRkrZZO4jp05HZjmC8KjhTEDM1ziQ22iMtBclU%2FB0E%2FJi7vz9LY1yM7yjC9czelmAieCG9l2qMbIX0PcmvK1xOAlI7omWh1oYy7Q9LJ0LaErh%2BxuUHCNVevMgRg9NcKkC2it10GwSnlFPtD119vsR5lcPEfO6xnAD5V%2BfE8RqPSfZSieS0bOVMLq2Y6HOX2N%2BcDjdJCv1t0r6FmmFx3L%2F9D7Xsy8kPZMOqp7MYGOqUB0OGizoTGC35VdOnnw66an2ZfaNV6INBQKrEZWLc%2FRnL7XQQSTE4HAIhrb%2FCwZKerKfo6Rxq1wA84nU%2BWvQ10vj2EIS5RvZWr4s%2BPknPOcSMU23VbcNjRuIanyDc56twv%2FJec0HycojQQt%2F3hJUXWw3Uhb3zQCOYSwhJF5RqZQuDoHYRXGnpUBANgq0k%2BuNaDXERC7DVj%2BgU5sXge12p7SCbqlP3P&X-Amz-Signature=88a34ffa23e0851fbe2aa2c04572e3d4e62240a0f1be8e6f4b989ab5ff546e06&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

