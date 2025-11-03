---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XHICQVM2%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCmCNT%2BFAzgb3myWud9zM6NS7WsAiDDMQxUcC%2BgjEHxFwIgeE0ZD1HshT%2BHjH2Lk9z%2B4%2FLThm%2BhWZ8eq9iwMMWjyhMq%2FwMIZhAAGgw2Mzc0MjMxODM4MDUiDA1T99urtOuNc%2BObXCrcA1PR3LPL2fZhG6qeV02OLzYegcUKspver%2Bo%2BMrwcNCBlWq9q6rCXjOjFwssEMVn1nGUILNl6lgjlVlCfCfTwByRfDcnNPImXEyDgc5pTZ%2FMBxNM1WfuD49eNL65sOs%2BiSl75OFS4xxt8v5WW4Saog3%2F60IJh7Z%2BLRVlS8co3hECdmlUYLhqCA%2BmOQAcQ1UL%2BqPwP%2BNRyEJJWR9gKlObg0ekg27XXtflZ0uPyITdeQkZ4bvo4tEuM7pPOf%2Bdp9yJpoSdF%2B7BdkFVBj%2F6wT8jp95rtxgMrfi4uV6ncqOG9jqXZm7exQbPbO3vt862oqvbYiRfDGAGo6IpuWyby1uZTovtEeud%2BvJOHBsQ8jB%2FyRzTkhU225zPMkFdZ%2FozE1OPAXRe9Wf2R1oTHb5NwSkh2wrHmiLjZEpI2Y4xd2cZVmmnclhJpYdQnxElSAA2SUECpYfmPVLVAVNdJ%2BSg75M4RvnJlKD%2Bb9fEtRDmE9%2BMJOdFX6ZH0ryyPKj350AWwd50%2FVH0Ay%2F%2FecXAWL92b7EYX8rLDHK8pJAIn8yJ037d2YrSM088lmfU%2FX%2B63wP0LaFT37L2qZIf6iPScNr8fsAiPJgXC619Zo93a4E18IgpDtPDLs3eXDe5Jm6soHmgkMNS0pMgGOqUB3qBFv5MtYAo6b9cm42NDpdOAocczU7aUe0CX9pJkBABlx0JYli7%2BnSNTmarIf2RD40r5jXwl3fHgPPHDcvLBcKPu%2BwSPmuXIXO2%2BMh%2FJtgipTbr5bkSBva%2FS4CH9a4lCoiENinoZs4W5%2B2pLatSWzbTsGX6e22o2MoRJCS%2Fx2u9ijKRtOrwMeD9hcX34WMAGaWO9D4Zm5x3%2BapG42%2FUaX0B3H5z4&X-Amz-Signature=8d113d298c7eb8847a245d222dfc32664cfd717d7262e2ad5b1c56e824b20da5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

