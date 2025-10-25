---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665GRJB5ZT%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T100052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD3FhPLgZGVo%2BVS5VxS1AEaxDeu%2FA6gz2I4%2FzhZ86GbiAIhAPZF8YYUIiEDsKrFy3lHAv01IGCYsEDvHPUJeNfDPCbJKv8DCHAQABoMNjM3NDIzMTgzODA1IgyHpI%2FZeOMzCX1W%2BsEq3AMGzPhOJMjK1bg1SgAyfE0Fv8sbKkGKs2iAB4BIGj36biLAxXqvz4LVKdyttwxFri7F3ymb6oYtu%2Fc3CCZNvsbZLOlkSyGbvs%2FFskI%2BwqamxuPYhF7g8pF6cS%2BRTQjclMAwZQ2VTZx9AGVWMbBOWOgRj%2Bu6Bzlceo7CsaCnGUILFyX%2Fu%2B2M1VBhmq52UYGlcMnw9oCIvd%2F9meXkhFZAHkGwP6TGQpLmjKTF1VNdixdRjgnC7cfQ3%2BbKAY6AuzJvetjcpsI38lkF7edsKMq1OQ29qztYQBK8%2BDdGYwAxVTC2Ks15Zz09RZssArwpgjhqtLyZ2xFMJeydWHk5Ls5K8dHEozxAJ7j%2BgGYfPQZI%2FAhjWRd4BblMe7BJ9%2FHFARYQQ9zEbkj0dCkvgSF2FGYgnfiBdV93DPrXzQJ3EpS2QEt72DguziSwfQbHMYtpcHA0ncUatZcFWD4KQJRVnGnkO2i0y7kqk0ff9QDdVFRCo%2FAOcBMOdZew9AJXbVKv%2FwTtJfHwMe0OT7gyv5QZvehBRcBdlRbmKKk87D8Grrzg%2FI7LeTqne0%2Bx88gpj61f1DTiJIo%2FKnXdABaEh3TnVkWXk3A5S0i0A5nvnqDah7XgfAJoZtf0dfqTsA%2Fx4k4eGTDn6vHHBjqkAdkfH1khIlFzmN7QCmjzwuxVCqNrpD14MWb5Tm1tuIGUdxd9gdaNA2B2VB2PCtwasuSjq6Z%2FssMdNblyQsBmeskG59UlNZQ7HmHk6Kt9YYxjk6yDbsQN3WssAKrSZL9PYGiQw5qNSqGJA5LRVwxVkm5fja2UbJFY8Te0AuAHizoafn8rVT3mfgLzla1qQfDjKwGO2D0RzlGFwnW17s8bVgiEYmrz&X-Amz-Signature=04be53db7b5cd7f3a8a6cd3ae5ad2e55a32767c6676d5bf12bca41e10035b922&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

