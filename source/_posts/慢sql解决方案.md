---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XHICQVM2%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCmCNT%2BFAzgb3myWud9zM6NS7WsAiDDMQxUcC%2BgjEHxFwIgeE0ZD1HshT%2BHjH2Lk9z%2B4%2FLThm%2BhWZ8eq9iwMMWjyhMq%2FwMIZhAAGgw2Mzc0MjMxODM4MDUiDA1T99urtOuNc%2BObXCrcA1PR3LPL2fZhG6qeV02OLzYegcUKspver%2Bo%2BMrwcNCBlWq9q6rCXjOjFwssEMVn1nGUILNl6lgjlVlCfCfTwByRfDcnNPImXEyDgc5pTZ%2FMBxNM1WfuD49eNL65sOs%2BiSl75OFS4xxt8v5WW4Saog3%2F60IJh7Z%2BLRVlS8co3hECdmlUYLhqCA%2BmOQAcQ1UL%2BqPwP%2BNRyEJJWR9gKlObg0ekg27XXtflZ0uPyITdeQkZ4bvo4tEuM7pPOf%2Bdp9yJpoSdF%2B7BdkFVBj%2F6wT8jp95rtxgMrfi4uV6ncqOG9jqXZm7exQbPbO3vt862oqvbYiRfDGAGo6IpuWyby1uZTovtEeud%2BvJOHBsQ8jB%2FyRzTkhU225zPMkFdZ%2FozE1OPAXRe9Wf2R1oTHb5NwSkh2wrHmiLjZEpI2Y4xd2cZVmmnclhJpYdQnxElSAA2SUECpYfmPVLVAVNdJ%2BSg75M4RvnJlKD%2Bb9fEtRDmE9%2BMJOdFX6ZH0ryyPKj350AWwd50%2FVH0Ay%2F%2FecXAWL92b7EYX8rLDHK8pJAIn8yJ037d2YrSM088lmfU%2FX%2B63wP0LaFT37L2qZIf6iPScNr8fsAiPJgXC619Zo93a4E18IgpDtPDLs3eXDe5Jm6soHmgkMNS0pMgGOqUB3qBFv5MtYAo6b9cm42NDpdOAocczU7aUe0CX9pJkBABlx0JYli7%2BnSNTmarIf2RD40r5jXwl3fHgPPHDcvLBcKPu%2BwSPmuXIXO2%2BMh%2FJtgipTbr5bkSBva%2FS4CH9a4lCoiENinoZs4W5%2B2pLatSWzbTsGX6e22o2MoRJCS%2Fx2u9ijKRtOrwMeD9hcX34WMAGaWO9D4Zm5x3%2BapG42%2FUaX0B3H5z4&X-Amz-Signature=317dd777f2cbf7820a2f3d5d1bafad5dabda51c4c54592a95edcdb511523406f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

