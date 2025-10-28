---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TG52J7ZL%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T000054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBNeaaatuQkQeHVCj7FGwMHxh808kJAm5okXwvs9Dwh3AiEA3MIPiyGik8PhxdEfYFrTvj%2Bm1SfkNetXGC6Nuat1QUMqiAQIsP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDLg9YWpg3uoVp1cYCrcAxKAE1SZWLIxmU4SdBZIZAFXu%2BnGMKqQDrRBQcLqcGs5Rr0UL0ncSYC2IjWvTjPjJvPgM4dA0lF2lA%2Bk7z8r41TtkSrPe6d4QWjTsp80ShQG6JIiiTWpKOxG5KoRziZA7PWxteXg12otwOgEzNNVe5ndH0phLtvRdLGqmhXnEsCXFlrMfny3kIQcreg9RGL%2Bvu6eAXiJVMRtPObPjEtH2vkrm%2FZW3ZliMK7XmosDNKCyRTk1%2F6pFNijaIKFBlV8ZHwEHjl4Z2EbDTwMEjgoUEpRz6%2FV98IeYHefrmRoZd4eeHVGHrFp%2F4wLu1SEe28JxVftbOBoYfcdzU%2BLvoj8264nllWdMIPMJHfwUwZHECk7Kg6VuXidBFUWSD9nPMGDKpb4HWjUYN9mCaxvT0w2p0b%2BKCidX6ywKU2amoKzmBXBZYmWYcOWutd1sCmFkH%2FdUM03VAkYq6U4ljcEMJLI%2BaeO8y6uoZLam6BRGvlbLtjud4FxrgA30nzsFpmq6I8umzlO3hAZKsh0hGpqB5pHGcC8WAIQhU%2F7o6ZPbHMUPtesmt6o%2FOvwhM9lZHwvY0gDxwH4bX48BIbnQbYcBjbobhmlHdWHiRrck6BMnnayoe%2BMhE8A1i3pkrRdCrsXQMPD7%2F8cGOqUB9bKF937J9f12RKo2Ak08Rs63e0rrkF7VR0FlF45fr9Hl%2BiecIYNpRT45Qhz0ZSQnvt9nBSxzaU92k5kPtLPxzLASEy%2FkNq2eNimqXl2aDcqwdjpEnacLN9EyIgum%2FjiZMqrpVJXQyCyInfjozfZEu76AtY1Pa4Z81whnRqmZlUYPFxxzDI2rblzIO72%2B%2BfGUqkDklFd9FRzFMTPFPjNo3JIPn0zU&X-Amz-Signature=237862213504a924167a61c7162bc4302186f24a39993548cf066ae605ef9c26&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

