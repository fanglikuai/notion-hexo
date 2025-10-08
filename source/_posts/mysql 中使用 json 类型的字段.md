---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466333BVHVL%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJHMEUCIQCSloj9OxPCwH2oh0K2mLgBIbkveT%2BxhOSPcZ%2FphV5ljwIgHHalf3G656pgGGZNschkR1BrvsaAqu9UrgCOJQi77rgqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFzgqEB70xR4HWfKYCrcA6NmY4jCBzTMQcPIMhp15K4HrykxDq8otq0ZYX9PuyG9oo%2B8Val0CmTSHXMZSOs4m5%2BVomWG%2BF4rrR15mNvwL1hBDKGoz1aVMzNcdw%2FztmB7LUSYklrN9iERzwQJYLBHWU5dvXvV5OLAmBKDs%2F%2B%2F8roeyLpD7d7P9gc9nAXnU0r6OEOoKbCeuz6pMMtrJ0TVsD9cD8mVweUACmr3wzaLLo6%2FBSIsgwOjZZm%2B%2Bm0ogrydkc6Np3YpXZ1kai%2Fq%2BYfl7P9q5JBETY5WAgaENh9dAqQYT1y4FJp1k%2BJ0vmYKwIkWW%2B8KsByjC4dL7vJeppsf2yAlj0vuO9gPKeQKn93Ehn8QJBLvIu40p%2FXFxQ0Dq8dasA0cp9BkODcNktsckFqCePMfIuYVy9K16OsWJM51RpTND0Y8zvEhfkGqeoTS2yrujSNpdESm3yzUmLI7FrB%2Ftup%2BUvq34oKPNQg27ejGs2qMr1EtnOnJ%2Fx0A8yXck1z760sOKX9ruVEIewCaxStFTQ45gufrRWpdNvAAEonvSLBhiWv3aQ61cPohsBsJSO6uaEMmJvJVLWHfE0shZ91Ol7hDtP78Ejuc3ehlUs2LXB2b7RQLuak%2BhrGbv1bmXzV7qZdbw4QHye%2BJ56ErMMiNmccGOqUBgxec8daB0jv9BQWuTPYrCEclY4HpzrRY9wVlRQDmW1Ibkr9Ms5Cj759%2FB5I44p6tNhT0jX0PPISvbYp4qICo7syo1b08muglPFYEwPNTKxm08tLLzs3KWXVtMpMoptWp8sL3ZYlxklNk7pDrJM%2F1shbgOX43d1%2Ba5Tc2NBHI%2FEli3gDUFORyIXbV4BPCP8Q4VeeKOe3Y8SdiUXxKGaT70HyoY3Lo&X-Amz-Signature=d9e2fbd08bee6a51379949c5ef743c462f933da4da5c56ead30c06dd17aadc7a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

