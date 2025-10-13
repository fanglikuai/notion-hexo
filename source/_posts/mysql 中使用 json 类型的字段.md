---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664H5DZRHN%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T140108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDsUe4Wq5L3KNseRALcl8mx7%2B6GAroBVRtETM98x9%2B8BAIgWM03u9k0fObZveUprtRiCQvFvXpI7ZrQetHQZcCzBZ8q%2FwMIRhAAGgw2Mzc0MjMxODM4MDUiDJsRuApzec8547zWPCrcA3%2Bj%2BenySulUsaLvSkmT29IVJ5OEI8MWMr9csG99g91CW8cXW1zsuRjMB8izUvkqHL9Bg2boJR7PNQ9b8yg05q3RBfbl3QkmT8v5s82F%2Bd02QvOj1UJHc1IQm3cW%2F05Kl1r0lVQFxWq3Rb4jTBzd%2FsF6dRvFORgTJ3x%2BaDxXOO8cMvR9AblAMF%2FmDAE44f8xIYuod2C7VU8IrIdQXC8EcQtltwPoL7n2cXRSPOou2EmXG%2F%2BAb2556doH5qA9LLt6yDWa0UmGWn4hlIpGW8hRD%2BPDQLPeMmtbEZnSm6p6dBF6GU1g94gGFQpcO%2BDLqe7maixvbZHTwXh0%2FZ8K8vmlIpxdkHmyecAa32k6IPzcU7fYiJxQYccXpck8KYNEKKtWjHwyTMEHycE2oTVj26J29gTo6blJVY%2Bx0YgojqWOPOI9feefd4TgZlUvDk%2FwJPodZgua4ZFfEAerz8E3UGCWDgMDERra8huH4fONjprTg8KvnuWpVg5UvM7Yjc0FGCkAFH5gqoUob%2FHtZzQ%2FeMWjj2EhXMsBv7lyA4IMLa%2FdIIMhLLDm8t2RGfABzMI0L6Lu4aO9mJ%2FgWX13GVQJdqp1HFhuNuzgtdPxXnOBPxQsgOF8nRyqd7VdHPbxr%2FcUMJDvs8cGOqUBjEoby3Z5da0syqbEu%2FsYZ09%2B2SpLwgBH10YRUYFWJi6%2BOPGo6Ggz1MLbDZ7GnyS6GbwdlEbFc8qo0I4P07iSD9BwJt%2FFaLXN9m1jiaixxbgRWgq%2Bx9o0SzUCm9teD%2Fmjd0Dn55%2BWA3e5DOmYeRjheRqm8oXQK2JS3i9H29kNLF8PCm0nq4gCwO9RsqN6jVaY40a2YVKAwH2iDQtpKMWUQQpDgJ5s&X-Amz-Signature=38dd231b990dd91dbafbdb2c787dba2e30e341cc6e897b5f6e20722b98af238a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

