---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VB7WXFJE%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHyVquGJuqat2HRFOlxKK6ufWq%2FjmVOcCAh7QuvsiT2%2FAiBYbkbqkgMuzqPytFLSkjzAaE3tcu5nal9GbnzbecpjoCqIBAiN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcT4YoR%2F9FuM5iqOSKtwDAL%2Bjc%2Bn8I3JKrtlEB6JafKXImKKWt8VH2AxfjnZ2SdHEie7FEPyr74HjDhYSwDs%2FD2kPjGvYamdCcDJCnjlHIdeHj1w91cZm5mTcv1ZtiOLflSksxgpW7aBudeQ1X%2B8potMZt94o0KfDDwryWUSRvCRV1C0m43c5wPXE3BCzsRvI7up4D3kemrin6Y8qdsvZtlRRxzTAWz00UCeKwtwOFokqLaUJqyNo2KAtMG%2FLGQb%2FhxPRwC7ryLiHAsAVpiGFT6SgkO%2Fudda%2F8vpBXa5PErf0zLd4TMy%2BWV%2BJtFihy07o2qtTwOd0TXay%2FJmCdAHoKX58k32ALUickNESxVMNgfj9SFYMHE%2BOVlbPMc0gnwRSjCwH4yyz0ngHJrsw775xPhB%2BnsLB30K4ERnNszjBK%2FMC3dthsNX92LAW%2F1MHyaqMi5t%2FeZb1ffHxByyytC6h5jAOokRSV4TQgSBPvF8jPUPogbWtAgoGG24JqeLgarUZE0dEwiqwJ4Nj6svjsXERHM2%2Fl%2FqutNsBAxCy7sstYNJpEoUChDP19NspeWpn%2FzlW6Mvn6CwthLc10l5G%2BsZVW7ev8Sd5k97LhrwORNcgjK3KH43It5Wi1r1qPL8g6vp%2B8qnut9IBGCsOeX0wpe6syAY6pgEiz4dedaiKa5yE5YbuYQ3ncxMkWZIzl%2BWkGh%2BLiOA0a6HvjrJJ0oLYRjJalUYKNhFTN8lYVaPKfXOtwtU3x%2B2kjA0Pg6VgRsNkFHzH59T3xhEurdG%2B9IZfL5N0mHbeIapB89PnrWC6YrWzgjqny2u3GNGvKvKk5x1T7EEOe29nGmGe8RxgjEejJtNvYNWyDmfRuao09acbulvSbuQVjisgZmDDgub2&X-Amz-Signature=2b44223016f0a72364f7af92d53c76214fb09415befbe944d742bde4cce05ca8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

