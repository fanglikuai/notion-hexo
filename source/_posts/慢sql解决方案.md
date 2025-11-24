---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666OIEGWKO%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEGGtFBXKorbQiXWqdErc6M8OPT52HJALqXAv6iqc%2BdMAiEAvfHzI6kc3DC0fhWSeGXlAQ5r%2BtIxe3szPVmQfRdoEXoq%2FwMITBAAGgw2Mzc0MjMxODM4MDUiDOnwl3fk82p8L2xyzCrcA3L1bfiKFSpOXgdqd%2FC0Mw5juoJ%2BAXEsgsCOibf5nTBl%2B%2BOJL83h9v8AmsvRCTRHTTO9cMpi8zixb3leeYbSYDkPTmSsDPhcpvP557Cfm%2FjwGgwbWnEPyZC3vZWfF53PG%2BComuc3QLjh%2FPYGtYyMYZKRwuA5kuAbWX3iG5Qld929FTxUXV1vN5NOyuvoKsPFYW%2FFpcuZhtttlNyDPjwOejP9WNHIkr3ovzmV19HSEKnBxsGhfO6nSs4r80ei1A%2FXEoDAY4LBkvUWa4cC7kHG16lejSoPgzAJOstjUg3UCUwMGtb3k5a7DsXtIBmSdDSNb6FNPtBaJ8UTeLHxiWUprZdD881fDG6xHm2O6X%2FL0t8nf%2B8r3g%2FHM4MQ6h4hf3B6aK31HVOG4L3txAGa2TJfTBnKz%2F3%2BTQk1m0LWke84vHrXjrVWPkM2HExOTPKMgqheSzy7rBog9bfZnXt%2B86qUtaRdXBCYLWdSNSTAnxiL%2BJfTlCkWLs1kpWAEz9s8ryU70ixogr78IjoO%2FmJvMuCDlGmXUvV8Fb5V1%2FK7AMPILQ70KKMaO99YGjCmKDhhOQJT8hHugdBcI5vMQo0nv%2FT0CM%2FD%2FdSQLwYqgdxnLxUiNRoAAgaa4K6gFgvqhDUCMN2Oj8kGOqUB5UGb24vy9GPq0KKQ9RYANBsiJb8E2a3OQvmSCLfn5M9VatLdtGssOsBd8B8XJbYy4TuEbRipG6fl3jlZoeAwGxsDpFbLfCgsyuaI%2FnW6g4A34IFCrnNAQqrS9NiGwIVuLBkvoKwrblIYqyP63xiAGyK8qOQ%2Fx0%2BpI2mSMkjatmd3u8%2FKM1GOfLlXd9ydWsMigOOt%2BHwBTcuTSrFCClt8iVPPB5vu&X-Amz-Signature=557785ecb30026ec42f9c2c2d78556c4b1630a00ae4e4fee1a718b1bd7175881&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

