---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VWDH2FV%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJIMEYCIQCbBQT1lPyRxen6eRKcvI%2BXSUXKeX7ORXDubz8NJIW%2BFQIhAMBwJPUygfGzSxBVaqnAK4Xu%2FNWeKJ%2BeHh3ro24bBseRKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwIE0KyizJcLAOZYKcq3APzd%2BoyDQCeIAcXyCTKE1AcdzYhy2CQN2iFW4qwCPhZpNrmlFp6Ry931T5RGpGyNm5uNL%2BvR9MtjxtzadScBmS1sszJj0J3CJgRu9kvkNsC5l%2FRF%2F5L1nAr733ExaydgVWkZzIrh1I1lKAC4Qu3ThbPB5BZF5O7M3xH3E%2BwVX%2F1yT0b6hvDdRNEgernmz81suVXrLY21AYDQD7dFADgvz92wqskVLjokLIjJyo5u5ELX5guotP2Ih7YBIAR2LuATCOdJdWF022hHqLE%2FWXggHoOMt9iShXPrFQEtA9DaYL3%2FaqUDg0c1R6TTnWz1SQQY9KG5w93KRfS9cdtoTw9wOZryLBKa5LYAFxjrPzI7UZzLLhjkdeJil6WJ9sDPW8vgCyf4Q6uQOroX1EHfS6SzAhZ7OtcHLsMhhItGeS3eTpnZRxMYeuz5tceUQXggCC4SIgNrvDwprrd2I6u26sNOeI9JCkNpt0ZEE6CsMurh2CKbA7aCdggLOy5dv71NJP9lJadx6nllcvHpvzNzDVd0LBU0MoqzVM2gmvLdKnIozwfxx%2BREjkG%2FmDo1iC7xfXSjH3%2FNp11KmA1pfkyvl8FZ8FoKA5tr7Ey1WQ3SEHS3prdjXROmSCE5op%2BkP2q7DDrpMTIBjqkARqJWT2m7nsnDex1PhS9ZC8177sd%2BVb3Bp3owaMLfsQRlXm1kkKwpcgU6DDGFggOmCPn%2F8nxnD2iMUkBNcczcxabCVQJGUhRb70HaK54L8G45DhsnHrmi8zRZuxHWWWypTM20ACz5UMHLK5fzWr9fxmpw11kdqSUcDxS7hNfnDw68ncNIcd%2BnzdCT%2Fnud8kELZQSQBZkhBfaCqIk6qJb4bzcPl3P&X-Amz-Signature=a168be1939bd5f4105c2a1e8f85e8b03b0f9fd5527228764f25ffb9a73bffd02&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

