---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHZZLTGC%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T080041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAFyg%2BofLeBav%2BCIzCDtnlDLJHILj%2BnosPT%2FdTQV7BjoAiBzSD6mqmZWo4h6IjKjE1gIFZVyA1Qo%2BPECGTv8zcC%2BDCr%2FAwhrEAAaDDYzNzQyMzE4MzgwNSIMrgxru%2Fxxq5jA1kk2KtwD50IOA0egredVCBdmzF5scZgNQ867zUR4HUbtyp1c6lbqwOEr2Qba0wDD0i6iX2Q8whGl159KYFnfExG3Ieu80wN4aKCaZcPKKZwb14vEQB7K%2B9ncF%2Bsvul59Kx3rXLssi7WkRNa6%2BpWLOv89L2ljAmFmIyOxQPdUe9jxGdAvTS41DNIpR0Q0kxoujhflwA4C970LhC5CBG6zM2Goyv6ozUvtxjgraDBnWZPU5p74rrYuX0fV9UxID9dDmfgN7%2Fy2WCYQbI4yXnr5QGnA49dDfYTo4j5hx673LVM33%2Ffejulspm3MZVYcevVMwf8CmYGF65rw7SqSkEWuQknCsjYeL%2BcNaga7gnD0brN0WUYVRQsKveWy%2FG9rbbXNYVFBH7QjKpN6kbgmCjMKZn0gmBjWT46E9l0Ei8v5dkMLOwNf8rd5gnbzK5nfkoRzZsqEg4jEEqxQkBSOTb4bkSP6ocQM31AIO5qgN%2F72p2hKsFPqvpV2jtGSnjqAx43X7zUKILL10xpsyiKf%2BTWqMhITe%2FV2r9USqbGniVABzpA0c1GLj1W6qaHxHyxYD1hSY45POp3Lu%2Bjro%2Fa2Ps9H%2FcFRqE8QqNJ15ghTwQtb%2FkIkBDtga3aFGWRszeV9x96HeSYwqIm8xwY6pgGBKJMqtkrvMpRpUEbpOXeP4k0KjsP813KUjgvR22GD%2BzL6wSiSKnWaq86THtphcjotsPMMUw5Wpr6vEkoFu0%2FpVXWIPDqMqgCgehThp8pn7eleplIoxAaLwcs83%2Fe%2FIV6o5QZAm6W9FbzF72abicNzQi4FNrTgr7dXtJ%2Fwrf8mnKbewi8pna%2FKksHfqab43vQfgHUsiT7WG5YtrDKAwYo9FZX3gdRk&X-Amz-Signature=01a74f1cb863bed26e6ab4675a132ea4cefd2eaf267fb8eb6ca1ff07b616609b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

