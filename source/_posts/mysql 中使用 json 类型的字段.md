---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6AZK3QN%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDge5mmOKqses75ygEDuZJ0jmvHhz%2BLufRn0aOv8cG%2BvwIhAKgg%2FpJAh3Ue%2Bk34FQbzkxniDn2h3XGqnuGTPvADK5F9KogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx4idozHZSdzqFkus8q3AOXxtoGiqdsovaluFdDlxJ3%2BpcCkXlEVVK%2BgGvPftlXzmHlLwTaCzc5ENaPXGn4QOr7qgrX8rrv%2F%2BXF3Gq9zClVKsk7L8HV9Ft70osV5aOSET%2Fwuwdk59aFsTCwt3AlYaB3MbH3MJn3%2FB4T3HidhjuDXceQCUJuM%2FqqaLsp0t3WmuUCsxtJC7UE3YKDO8dksqvi1iqV01C4bdLy%2B4Ty9HH6khoUh%2FIPMMncwGKhWQGOjSJR7%2FZxK1Khg%2BI%2FRzjCHwhnXy8mGXILJYy1aLp7N3%2F%2BmO4ANApEIt4YRgVv7pMPpQd%2BiuRmmpsh%2FV1Dz2C7uR1K8qslUd0%2BfbLGlNzeR2GLDp%2BODKk1mugjaZvFJCP8m2OfVE4MsINEJKaOmDCP4A%2BdYJPtDGpIeDvx5QzpBRTvOs2a5cvaDushdW0bgdYMWY6A7yJgHu3%2B5DOQ9WAt95PHSiG18%2B0ZX1%2FpgmrqR0dePBO25ojKFudNe7xMwmEhtuAiz9I8w3ODhuN4g3R1P2fXgaPHuHouDsB4d094QEvlOzfVPu8fGmT1lgM1nKmTe3jWHcQt8D7hge50%2BTesf5dEwUJKI3RiSXPdaXMzTjWILScVobjMUmWRdx6xMRk%2BYCDBu%2FioqUSq%2F0xzFzDUsLfIBjqkAZmRDGo4CiPHqzdkDc3ljDoCdx4bjRKoLgE7mylX2YvVCE2DNBtl65noz7yhHMNOeC8uPxvH3eyvw9M2JxUvVq1RJ3JOFG%2B%2BKZVOAh%2BiYxsRoZVKDYCzJshWu0YmZid4RYrb6gyHDgqIa8iSWT0UP5sHv%2FIrB%2Byz%2F4oCE5FM28IQIqvK7Qw1%2BQAcWk8AuOxEC5r7%2B4ftEXYO%2B4uNLS4DSkGnMWwW&X-Amz-Signature=e78d1013d7d8ebaf7a34c423319abcf4aabb8f3f1fecdf1a2cc60e3506ed87d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

