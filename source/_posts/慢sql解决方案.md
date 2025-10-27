---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667QJ43LJH%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T130048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEsaw9FOXd7mvI%2B2H0wL7bW0HIeydzzR6ztSlNxmUzDxAiB7E5y%2Bnbr7f733Dzw4Mkd7kW9Ser6RJZbhtJSWfHCjoyqIBAil%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIML3ZQL2AyMnW2NJ4ZKtwDkjLf69FxZ2yFsKqCbBll7Pa8AoF6AQUJhqL9RBjbAV621AwcbZAAZa7Hby%2Fk%2BrL%2FwRtX0pde2gZWq6GtpIFqQ%2FIo8Edm8mgef3P4vibHDZqNXa6FaPunNAfANgTgZzY2UrjuMq3jrMMCrc3dUjvVDenvenc8fTHmk1bg8G0FQl8yY5MSMcn2t382EjTGTIKTR5ObD8bHoKPsvmLx8%2B52gXIq2%2B1JeApoQLXXRZFtWKtHF3RYj6x1zqZJNpwQlOfMknloafp6UitfqnHcdsigvLTesRVp48IkPyFhetmWR6dOsGpRMXfm16mlDF%2FE0DlBb8EU3D%2FjDayBQqHLicy3%2F2h0KaZWkvuCjOitxaq40fgQBXO92IR1WLoS9aGDLIDttFc%2FLnZ5rrlS5TWStnCJB2LDgg5Y%2F%2FX7LfcFLQr8GdvF1Xp82DtRLj5S4BF9oCp1C423VD%2B6NVbFS6Ba7OJal5xq2fxtFLR29NpEa%2B643w2aB1IdzC7VPdig%2BVagqLi28%2BnTuq9RK8ix%2BdBeHfIDF8%2FAHvAxtdagrGTUvSSVFWJknaTpnSSLwjnK9PY4ACXfhpR2U8BeHb2iKZcKuWsyDRg%2BIWCY2hRwA5hUqK17J%2BOVQHLYyD2asS95Hhswkb%2F9xwY6pgFCY%2BjbOQfHg4SC95AvgIgyvq1Jx1UtpixQH%2Bccya6OtGHkf9C2FJbefKJ6eGJjKRje24FTXhzYk%2FxuoZjOcoqIM%2B9I3PShULOaKc%2FNH%2FFJDHf8B9e2yo0GPu4zinEYH5MdP79d9Aj7f0%2BgkaKnAPDt%2Fr4bqF4Jj0CcQ0Vk%2BTfraIi5bI78ZFPhH8%2BtZDKq%2FeCwnqbNoXhfFSEaUwAjii8nsMAwdAXK&X-Amz-Signature=1d37238bf3956a41914134a804e770c4bd2554aca41036238849f7eb9e8ec433&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

