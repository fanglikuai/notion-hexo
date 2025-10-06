---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GC52MUV%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T000035Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDQu8Jg%2FlXgAz%2FUoZ%2BuxPiPixWlnmw2oAKOzWwhFbyypQIgWPpONnb8ibodOqfy3WJMZkO1OvTsRUJBfMcfHVoIY4IqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDh8GXFaMbDic%2BepKCrcAxjTgYgOgEout4O6AcB8FpdsRotfZbqIgAWRp%2FPfW98q0l8yfM0JLVe%2BUImyfPkYJCBMilnaoAJMHdrRBNXtGvGmLlh0JQcPJgukkg27CdChKpKqhoAMDE%2Fpjswij5TPmXp%2B6VLdQffIG%2B%2Fz7Nok27SmzlF7F6NwLEZ33f0ZBZNDeDEliMbnVHbWz85V7TX8v6ZExQjIUWEP7Y7BbDxLM17KN6A37k2W4u%2BffNio4oHgrmHzVYMwvX6bT4cFfbEOAYvBreQYh9Ps%2By%2BFx5p7y4dekj0esVsHJPuDw54ZEQxH2nsJz4T2J0UzAz18TzaEq4iveUVPTCcK%2Bmyc7LwYZfBcrYhoU%2BFa0O4DI7oTdaQX9gUUOi1CaHUHB7yn%2Bl9vreUAWVlwslBDkSoqZP5AgZQ9OZemB9VnUI3y4aYbUEt2eqrkPyqDmGvkKZtWC7TaXNbhvpbgvqd7cVNkMIGk9TVaL1qX5heh5zgq8sKK3Bm51uqcMhH21jprXRtTdfAhhA130KLjHNqBgCkdT5qsM0lkF6XIzCXM%2F6XOTyLRl4tfZXUT2%2FmyE%2BFJpadL66%2FOvIhNVb1QYrxCOKzJR3xo1yD6cYcfZcrzV11muvequqVsxtdu3v5I5aWDMcLbMIb%2Fi8cGOqUBxizIpSa3YNP3n7dqJVV7xTWdOPcGmBWA%2FZHAPFRARDROaZvx%2Fo%2FfXUH87LwZZmyxwp7qSAVXWTpG6zBNv7p67UCBENTzL78INTEGQH73vaZFGd22nRdG57%2FDD12zxYxDXHH1uJeNmft4z%2FcvFf9i6O5Hq0jg3rBarYJVBen3P0jKK4ixSaztQLQanWG5d8MJqeu4YLi5N9ZT9juLJH%2FoS4%2B8AFBJ&X-Amz-Signature=527cc35aaa01bd855e1fd9267d613cab145a55c87c21ccdf3d80c49b65bac3af&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:55:00'
index_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
banner_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
---

5.7 之后支持了 json 格式


但是在实际应用中好像不怎样


# 配置&使用流程

> springboot+mybatisplus+mysql5.7

## 代码配置


java：


![imagescce2478e5401f24de6234fcc9a70b5b4.png](/images/476a1133e7aaa3e257f0f6fe9cb407b6.png)


mysql 中的表：


![imagese0bbc4d10d8ec7819433a5e83f307a52.png](/images/e2532123fe03eee4705d5db2c2ecc85d.png)


## 配置类型转换插件


```java
package org.example.studyboot.demos.web;

import com.alibaba.fastjson2.JSONObject;
import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedJdbcTypes;
import org.apache.ibatis.type.MappedTypes;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

@MappedTypes(JSONObject.class)
@MappedJdbcTypes(JdbcType.VARCHAR)
public class JsonHandler extends BaseTypeHandler<JSONObject> {

    /**
     * 设置非空参数
     *
     * @param ps
     * @param i
     * @param parameter
     * @param jdbcType
     * @throws SQLException
     */
    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, JSONObject parameter, JdbcType jdbcType) throws SQLException {
        ps.setString(i, String.valueOf(parameter.toJSONString()));
    }

    /**
     * 根据列名，获取可以为空的结果
     *
     * @param rs
     * @param columnName
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, String columnName) throws SQLException {
        String sqlJson = rs.getString(columnName);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * 根据列索引，获取可以为空的结果
     *
     * @param rs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        String sqlJson = rs.getString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * @param cs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        String sqlJson = cs.getNString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }
}
```


在yaml 中配置：


![images944ad29a7fcf96a0c51a577d6bc43317.png](/images/4d25cc1863ee3e3fa6ae7e6d4c2a6cf7.png)


xml中配置：


![imagesd6de49b9a7b17849e0d393569b93bca5.png](/images/1067c14ea63fdd81764edc7b0b6e9828.png)


# 对比MongoDb


假设有以下数据


```json
{
  "name": "John",
  "age": 25,
  "address": {
    "street": "123 Main St",
    "city": "New York"
  }
}
```


使用嵌套查询即可


```bash
db.persons.find({"address.city": "New York"})
```


可以看到，直接被秒杀了

