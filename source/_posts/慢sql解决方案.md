---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RJWBZ7QO%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T020040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGlUh9F%2F%2F9UfsmwQQsX85df%2BscpOyX2fUsLY8J8rRIq1AiEAsz5DBgacCRL2oupMh2QJ4TtFUahlsEHwlM6LkWMBGuUqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCgE%2BWnlPtYhpKEnrSrcAwC0J%2BVL9ciNn1%2B%2FDdOKZZ5s5l0EQMz%2FusbPdul77yJPMnsclN3gdr6Iirl6yJ9RPnSwVH45pd66aP3Z87eJ9oytXaiPsoThT44IqaYryCDjKU4REABQF6oro7EepNxpddbg4RhVKJ6lBs5dzmH4ps0M7MCL9pMeQPmWHnkccczVxzVMtaRI8durCWjp3jQ0%2FLPebqWmSHQTwqszGxQ6rlmic0Xxf4xHk46YNZ7LLfqRVpuPCprFbUFvRYLp6J4cbkrvwak%2FDqyu2ac9QoCIx3Jkx2Mu8%2Bv%2BHhBBf31LsWqd5HJD7qOp1GMzeavMXKCjoSZgS%2BYm75fY%2BEfO2Awh7q4lX3RfxVHhNAttSzRsiG%2BbVrl5N4S5NTz9Ar228keqZYKnKLZh5v0PK4JoL4jTFgdLQE%2Fk33HekakwgwEU91OR6xM29YQFvJ2yKZb0Dum%2Ff6ZIDOwj1P%2B%2Fafs7CyDrAinQWCiChZJUjTe09lK0CVDeEXVXo16w5oT2lGpFXTVb85BpIZefCGwpXAPcWYQxgyr9VO%2F%2BWXilWduJpU%2Bs4JawYzZNKWmAY6Jczb5OvOurLQyxmBnFrvugBLg20jNAIugzARJqSc453earVvCTGVp%2FbVx1KP0yLWzDfEJcMJzO5MgGOqUBZf2eq3w8cLBk6uQ%2F1vr7aAixydfekop%2FBuUClMkdBP%2FCUUFLwN27yU7MCLm17mxWDGoKjEeGNHJ4sVAl%2F%2F26hxeVyzDa2%2F%2FurcA4l75OqbxzlmlqmCh2gCrQZ8yNmX2l4WeqvPZNJHiNByE1LambW9beN66TMgoKq52DB8MQwQtEV1WiTUu7U9uZ%2BZ%2FGqZgYC79Oxz1Ju4geaQj%2BrXJyaW4VWeuM&X-Amz-Signature=b7813232fa3258ee3d0a2932d20d7c44fb1cd7bc8832b13f2ae0edcad8678af5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

