---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z42A7IYG%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDkceeB0jiBNTov12bqwQAFHrp5yzTk92VxoiMgX%2FhlgwIhAKywiVaGPFrYjP7mAZ95g2N%2FGo1ReIPcuUww5%2BooC%2B8TKogECLr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxHcnRe14rhx9kPoqYq3AOz0k3KIn%2Bav22auNeT6Z6eN4N0tW4C3IbGzWGbB17BmGbQDQfvNeZIeJcpsdpbnlH60nFozML30Ftgh6UsJyQt%2FbA5JHIUP1ZInwMWBty6GuBhFV4HlrMnWehm%2BZdSUab6x4lvltdzYtohR1znm6ndemgwPTV1W1%2BdSuT7ZRKSO0TucGMn7ZB8G9Li67pTKDhgZHy8l%2BBFlisqs2sh2qFWxotKKHYjkdVLoIBr2fzC0OfRUC5%2B751ziyvTaxTdNDExxsYwMGim1rbnMQ%2Fep%2FWlbd6XY0yOadFi4DWjrDcUsnwM5ZNCyMtHboRYREpf3%2BPbfIhj9S0RPLtBDk%2Bwmb88wsDh0Ju8MLwacjix87A7RQJNLPC6rA289lOIuA1uZ7Q%2FXeOMPyvsRikEo3mmP9VYJW0yMXe2SVy33dxOTVDmfBgUIHil%2FrPF4mCUVGy%2F%2BXfBuuVq9kI8A%2B7Q01PJcufyIwZvncZn%2BnjHcBeuo0LXovWPeneN0RwbIoIGEuZ21BMXVY%2B0o0HSAB8qDGfH3wj4HU%2B1%2BFnMbEYi28SG4TtQisg%2Fwuj%2F7HrQ0guWH5P%2BnBiLoCtMKDnQPG3r7whEsescpshWazat9fghRi8PQBj8fy8RoCnISeqlhpXxNjDmg4LIBjqkAWMgPqoBxkK47c3OV2i2dtRxGZA021Rnkgfupa7u4OiEPhFnrc1%2FAZS7amB9MKSkO578rPAAmQdE6qeiWYAiCiCKYT5U7mywQeKGw9lwu60NGCBc0Qn%2Bt%2F7G57k9GE3a%2FL04wBWeMWe83U4iAVDzJ%2B5Vw%2FBW69YD7SavpNZpH4TqctC9ySQZ2ixE4g2mXV0CymbNS2qYSuigtituKyKMkJQE4iW9&X-Amz-Signature=ea670cb1a6fee66f7aa82e8ff3868e8f77d6a6e2511af8b2481bef03782aa1da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

