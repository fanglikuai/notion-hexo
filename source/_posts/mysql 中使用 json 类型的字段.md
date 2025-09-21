---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662XDV2HVX%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICiA9k5quzX%2FOumdcLqbPnXhRABpK8oCF59kjvIpKtwEAiEAx1alc9VjtM3yfpVlYMu0umop0avYTbPJ9ILk8JagbKYqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJRHd3Axr7gV7Vig7SrcAwnnbooe5PDPU2ZFdDMRAdv6nK4%2FbVwEaeN8yo9ZzPPIbUR2cE%2Ffv4f2r%2BT93BXIhjm2JvuM3P8j6rU6xbBOPJUyNZpA%2BObSGWzHLpjc1RQpE0mjQm04D%2Frr8r8wT8usC4riN2WdlY00eB2aF5L2Qsx0uBgBNkzcKdiKm%2B2YNEQX5tX%2BHla4RdhxXzz%2B7eZW5hXUQ3lK8qdikSMwWnzWcogZhP%2FKx0avQIN7nBNmKN9Kz7iQK2b%2BwNHGO5oUWllPBM4zzCRhwjSNJzMdieQnK6Ud%2FRp0cACUTH6pO1QUe%2Fkn6%2FFRBHjGGyi7u1ssvk%2Bh%2B%2FldkDqdEnX7q%2B3hkRi%2FGpJ6%2FdAubIG8Eq%2FLsknwptS7hGkAaiRJO4OaMEg8fquxiAaTl%2BQUsj%2FyZ6AfnHzHNVIbEfv6KpGxYy0icXb99ZgB83gv2KgWo1NANJunjJ7bN4AFLfZPPFHrV%2Bk0%2BRuSP0AqWPDwELsDSiGCFj9nw%2BB3RJ10vRhsRq0uGlQaHll7muxy%2FNa2D3vvZeYWGc3wVfkhTskN2mRToDLysJd1ZjIpfJ%2B15lXR6ni1H50sB%2FlxJQIF0D9Mi4o7MBMoOHppR3Z2U0kZQz61BueiNy6XqBKjnoFQJr8wXV6MewGpMOP%2BvcYGOqUBJzArr%2Bu7ZEPzVS0CbtdjxoBujngQ6Vjwtxj118O%2BiLRM8IKmupvYcKb3Fnb9NEnDIWFASLXZVcW3GDMNJvYx5BpQqFR9ThfsC7JtMYIozrcxUWh9SrPmHENNkoOQtjT9ZmLUNP%2B0wq36l7b89diLgxFQBHpjYPfN7T1pKudX%2F6L2U%2FDXJz10zfUvajErKSv9L3MF7du%2Bo6Ayv7EBEShu1eaumRpV&X-Amz-Signature=10d69acc1c972a8cf2c9d34d238176041415864bb38030f40b3c86a43512b516&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

