---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6ARI7DC%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQCu5XCb1RKbT7VZ%2FAfOqOg45UwG%2FKmFUkT4Ff%2Ba%2B24EtgIgHe2ksVJsvA%2Fd4hUMs86%2Bwd09UyYMAOQONtRpKXIMcekqiAQI7f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPOP0%2FKlRGDSBgfKKCrcAyOPyceIXMRNMXbT8TayO18K9gSW3iuLHmpFRW46Q%2BtfxQjNxt%2B%2FrCvi6hrQnf1KL2SR6iXaLrQmzO8bTUdFZ7suUOpZy8ObDtx6C87JLNORvWi%2BIBi9Kak9tAG85DFacxcUwCZZ4O%2FXC7CRl6iVt1ZgZpqq%2B1WRIsuXb4eUAbsLRQkeUsomykRDKsr27ZBpwiUJDgn76R0LSAuU8nW2v4HaScUSNApBN2sZoHYpBp7qpxrxFXXkKhQbUoc12pS7BkfIF%2F6OjjqxwQY4EFDAqpEzaWDr0Y2qIlS%2BkKYqGAs9XuoSlQlr0bL0csiHCoRGHfsvgqyNQLpsWcLXL5bfxAN11efOCva6zsNgQ1lZmsBEKW0DACsuJLbyA9JAKb3J7ONhupfosTC8Ar00qKp3zIIkLh7jLxeDABQ%2B%2FF5WOygjBpd614rMQ0q7zaUuYL0QEwZkwyNWNf5Q2NrTDg21PssWafXqGKsvZ3KOhn%2BhjeQg8%2FJSJzGEFUX1oO73Pr9MMZqEL%2Fxzgj0iICaXnihRTb%2BtqtG%2Bvl2xlBd3Pr%2F4wTlwSursIHCveInvu6ndT%2B3SNAbxZ4IO66nqQAeZoUB7sLP%2FJ9ukldFwBI5vQNf%2BrG18RS7wmM%2Bu6cCAk7m7MIOL78YGOqUB%2BFtxqCG6C%2BZ4tYrxmKThdkHXHKtiUCcW%2F%2FEuSxSi1jy%2FMv%2BCJOS9QC8jmmEwi06ok40zU51pLFKCwR%2BnG51U0jcNHPVonaVXvEW3gm04WzcAv8nsiufXqCN44NCdmH60cChQOOQAu%2FV%2FgrJzTxzc83TU6wF6vAzLS0IFn6mG%2FzM6%2FP4vPXFKAFRuU15JK7pQnKv%2BVmorRQc93UePOFhR2IYsVUd7&X-Amz-Signature=a507941d193c596289e755643a9031124eaa62ec56cfcba69bdfa1c1a1179829&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

