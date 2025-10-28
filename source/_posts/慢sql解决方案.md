---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T55NRDDI%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T120053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJHMEUCIQCuEqbPzlIyl%2F8N0eS5kH7A87owTKf7ZxyiLer2zaR9EgIgZLe5zpZ8iEV4hGPYON44VgkpRsP4QSaurwYrrEEt1ZkqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA%2BBHV%2Fq7eBahkER2CrcAy9nLltRlb4uPmrEVR%2BOcEUk7UB0sJKXeQOWKR%2FFNe8BG5h0rxXfgtf7gUtjWq4Nv2HVxRKS3TKy%2FZYukdn2vzKcMmBAr5W5r0NUH2JdsddxopzRX3kkksoi2O0QQ17g8dqSwpfuTzwMy56AgRNr6QAeQBX5mlWpCOs29OVew9tQ1QWBaR%2BdegRbLmm0Eps1Ksy9yWfSRShdJfWpgGmdZeg5l%2FCt%2FFIHN%2B%2BPLH1zUi%2BPaBgcJqQ7FZE0sXd1wqzagXmA9o77dyOQGJJ%2BMegvnLm3A2JnJUfvAuJXUBuj%2FPAoNdsgjag1nm4aJaFy88iv7bdPEhIbCp1iAkr8A8f1HzBCX6%2FfYHH40jt2wD7hAmD9fv5KyP%2FEPR4Iqoa52xh%2B5HJ7Ag3%2BhEcA%2BFyAuJ%2BF7xBdhfSCOgV%2BfVhHHLiPypizRg3CXAajbGTGCvmZlYKmZje8ca%2BACo%2BNiRfQhpqn6YbDqdXYmqV2o41cvZhkroxU3uRpasAq46WoqWk2JsVCrm3cR3lSJZ0%2B0MgvdNt5Kja33cKbfJ6TelrkUS9t9RuX%2F%2BrcDOqxcYKkuZraqev3zBXW3VXLe%2FWrNdOQAoYgzZr7N1nMDS8GoicncdzoovUYoAWq5zdNi%2Bk%2BI6dMMMfIgsgGOqUBwTYL9Ay0kJP1J3n0%2F0aOXMtMftbjo9QmdFTbQNQB8QXDLHUo3sknDZIsUJNq4PjGAETgvCkolRroEIOFtFFLyCsTN9UjxygkDIlDWWJTaw0eKRaii1Wm5CHyE41QL7%2FfGdWP%2FtDztCgFJv4E52Oa2VkCeQk0popfQVZ2t5mWrY1h%2FqUvCIg%2F%2Bj0tRD%2BnVOoOwUeIx%2FvorkrGoH%2F3TFXL4Qpso77w&X-Amz-Signature=9d0db0abc40286bf050d105e649b1b5ce941f27a4f67ad6c2fe535a5a129c488&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

