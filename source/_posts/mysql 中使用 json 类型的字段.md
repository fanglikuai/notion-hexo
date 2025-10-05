---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EZRDGAO%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T130042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH9TeMLFtUL0wb9o1Rr2jK7jhqnKj00i7X7fVzQitBisAiBbaysljLw3zi711uac73HrBsqECXSaxyDwiEsMRCKRTyr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMz21BkyV06jr24tcmKtwDXARca1I8lCwSq01Wip82jup79xmAWimnd1FEuoBTB0NvvjpzyuSg8w%2BJN3vYRHZhgB7JkAikp5cEGFg5mcQi4lxRxCCKi1wODRQjXl3VHO39QHpMbfN1sArmLrkiULh4lZ1OAvJFwpwt%2BcWqijziFe6sGtLECNz2FSXtZ82qFrEIRsoh89Bf16rKF%2FrOB25SU8aCv1U%2FZZp4CSWV5UWHzDklw7nlK2%2FEqBekCVuf6y9DlnEBEHFBs6%2BMAPb8%2B0ZJxoZmSQVm%2Bof9Tr1vX%2BP2qLDFFk%2BqWAAhRZNGSP2HEBAlqicPWAdic%2F7KO7dkUjFwevZsxKv7unQ%2Fu5dGUxJxPiRIDUESQldAUt8Ef32llnDXKm6LQR7MD%2FGQ47hgU57G4nzQMHEfwXtAUZEYPTs6FdSJ8Pr9PiR36s%2F%2BsAKhTpO2lIwPJ3GTdBQhCWa0ZgXsqgi%2FKD9gawREeMYc6UcQFY56mSPm%2B%2FVqHpkUs4pAZPxDS5CNnK6DxzZv6GtD0zz1c1kd5fiTbZs%2BInu6D9tYTXYo27jw%2BCIm8HDodYxNdIdtUftvtUxE%2Bsr70Hkhf06LpV09Z6zCxiDXrdJBzb2IrLQ96OfRWWXjSVKBfguv%2FWIn8ciXBWYjet6LmWYwvqKJxwY6pgEOeyg5q6g1XyXl7sf%2FK7K6hLhMVV9FkMBScbALZa4F70I20rY6GloyV%2Fqp3iS0kFpJyCU5DZGStweZblaKQ9cEB8BnOT9eRg%2FyRR34c%2F8Bqju6LReXZRiCspW7RXjw%2BHwxZsjMsfazL6omZgigGmpyeOIMWy2Qc5pJ6%2BixHUg8Dk39t7fevoYktgAugXpXiIDZpIFHH7S6HFoaNuGq9G1vkKwGjHrI&X-Amz-Signature=a652e0d1634c6a7b13aad1fd9e472f26392d0cfc7c091b159be45c3b433ae83a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

