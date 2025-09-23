---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EBLQOYT%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T140044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCJ6tL1QI8jf0rAFOCVahXeLpJbLIfEyzDLgUf4e8JmlgIgLfkadmb1f0ONqwakfwZyVR0nyf%2F617kIzPdOddOFXK4q%2FwMIRhAAGgw2Mzc0MjMxODM4MDUiDGN%2FWRzb0ssqUwdJmircA7%2FpFtman7k2kTSPW7r2SMfn98Ol5yVp64WEtH3TY8zXnau%2FNxUTJcMsf5LfWdyNj%2FpooNq5NSK9Q7tgIWUifWkMNXynMP2g3r%2Ftb40yECBsCzpao4CXGxhJKZ6wer6gOMEgaaR0VnGcO5n8E%2BffRq9CNZPLH9jBwQLPLy0NXN1pYyMn4aPXRKQ6b%2FGXHr%2FyXbSPcHWpVMBsc4wcqhISOX%2BcdAfbPySVjb7eUYijnPXK23RHurzwzC%2B4d6Cm4dKmBfHhrZxGCRjMEFks7wLEHez%2B3zCYsCkccGOOTuNeUoiDiPzJ0Zyt9jSpxWovaRVEQ6h%2BiE%2BvRGei05IDjq%2F6aUdoCq6zBdTeLxbPn2FFZWoRSY3r%2Fsz5X%2Bw4xg32hCZa9fK8y8r3zV%2FF3zRTFSqBcZtwQ9jnJlDgsRE%2BTqLU%2BW45PUF%2B%2BqJsjNEQNeTMwtKXoiQv%2FEnWDZ7rjdo8e2Sff8v3H%2F%2BmbIiHa3panec9yawc9r%2BlOk3xb7U7eBIXI19RvhPQZtlug6R7dMSMD%2B2UdJ8jUrKSl66rMTTDsaEWVXFl1NAV1hLr9x3gmxOkt52Khx3AP3%2B2Wjiu2cQNUY1ks5b8S40djKUFHPWUd%2ByWb9tIcSCENRRq0%2BklNrqCMJeyysYGOqUBcKJ5ERHWlV1kcFkn28qJZ0nPOhj5dalJVxa5%2BNyG6X6vcQrf1N114E%2F9L0VF4hVLN2qluPPfVdHY7nFnGl3bY8OKkk8B8bBfG2FvgucWVQs4bYujFnz43C6msLScbFIdLM1aUPm2BSJOhI5sJnC9Q7hEr2%2F8gQNQ8wl1Yz2BJqs%2FvrWAwmh5OQN6VRe1r%2BSkrS65OlCKvfyRWK9nYVMRBupvXMRn&X-Amz-Signature=6cd60d4838b63bb2bf4c0f2399056339e6ec30d378df3328eb3b8ac71d00075f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

