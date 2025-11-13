---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VI2HNJB%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQC4TI0uKMo1JwoGI%2F738x1kw52BihDnR8jGWyotzAXmwAIhAMgn8WuniM7AlBBI5zOLa05ailwtwpzd72QKqSlZpVK4Kv8DCEMQABoMNjM3NDIzMTgzODA1Igyu8aIEm1XrKLUCu64q3AM9KdOrtp5AzfXMsh3XdYPKnB6fuR5HFp93QNoWkBJe3ffdln3tb4HV0IYpz5e8l%2FZD5mIzJG4kWp9ADYoXqp6XM5hjS8LRP3zBZOUL9V6C5DV%2FvvxnShLzitgT%2B3WzQgJBJ%2BP67oXuWn%2BOD9Z%2Bwl2fX9iispTpB7Z%2FygI0l%2FhnwxNzhW6VXlGrxRFpkYyFSWoPKm20EHJTj1EKpyIyCw8dNR5CpScDTU3tskiBmBkHIItIgloxoSOTF3vrsqXQh1NYOw82HqCT9TQvqq5UaXAeA4D12%2F7s33fWA%2BtOc0vK9F9fkHOxJUvXS%2FGPwlk9PoyPe8cr5egF1Yf8URfXpOD%2FsHAz2L6UHt7UXHiPo%2FWpdCBVcGtGgNYsmGDjMvJk4J%2BArAiyHK4r3hqsjS3BPA8zl3%2FZXwmFL0iewFdfAZnDH%2BbQtAI365kzmcm3dVuXO6R12a%2FNEmiRC%2BvbjsuvAFjrV1mrHVQqnZvWR%2BpqRlCz94vNwof%2BndBsNQNDK7sz9ku8tLKzLoJMXKMSCebnxa0goTCMqw77hVYxnnEKgJTd8%2Fg%2BkjkJg10Z2gghEnajYeFPmGxOae2kp6bxsGraI1P1EnaSIUxdv2RUgZPkinvnLwepPuUcpesMAR422zCJ%2FNTIBjqkAXYPy1cpvN964iznqCJiJtOjzNs4l6d5UhJjuVtNm4Pj2ki3PmcWxrERSIY9JEombeSp1Yv3bp9CSx6Vm79SkGtBK0IhLMxRPNZCJA4WUtdGYZkH6y1FdtvuX3GwlSF9k3kNPjWnz7OkpmCOTUt%2BEsX7teMbbz40Ve%2BX72475DivB92UeZ%2B2dKHPDp0g5KFykAWgM49ALkMyRqAbynyjDf1loLnz&X-Amz-Signature=eb08c5d5be41c0f6d1c228c2fc47314bcc3f4d3ab7dc2ad9c63e3cb56ab0e355&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

