---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WDHKICWB%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICZ3yudo29IHhijihO4z0wsD0RUPAMjDBSaMGP5%2B0SdtAiAt30YyjgZ%2BY5siewpEdeiKzf2y4XI8MN%2F%2B5lTvYMDGjSqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMUw1%2FnclBC68%2BCwenKtwDVnFIR694SU39NCDdd9yRDPwuS709nC9RcThO2%2F4ncr5vUGEiVk%2B9DZQ4FzsOUZUzyCgTqW8iAxjm9ngcQXz%2BA1jpcOe3qh9x%2BcOTONeLBAuu1ngExl0%2BCfw1EnJ4IJ8pUhiUfNOMVgGsgelfIg4acpjoQgugKs7tvFkUF83UGsrdSRDGxUDtNjFfW%2BZiKcZ60RzfJc%2BanXlbYDVIe64FScP6ddxySwwBCmtozSRwQnzZ56GH0R5ivsRZVvPYaC%2FZI0PCjZHcXrWB%2Bp4zANkPzh%2Ff7F6x8p7FJbhyfIyT5vwlEr%2BjF9jLKtnn42uz87DW5rVfQvdbD%2FKuDO5%2FuRa7coH7mB2pToDsSB9WJuo2YjV%2FnWGVjAQE24DC9OTr8mxO7vXwD1Xia68EV9YXkJiIe4hx%2FNcXKZByxCOCPfaIQHJDzniTybBhZkvI42%2B6yWCk58yTdPdGBqAvWYmFCQCoUZxMlgYtTk1V15ZmV7xIS9UOeK%2FS2jglttQ6OCblxlT3G8T%2Ft%2F8y7%2BoPJ4R0WjvhAfrwjSDC%2B4RAReW13ZJGiOuAO%2Fc1rdvtumrTsVfgvBmKYdznRZ0poQ%2FTswRWrRC9E4d5KrWxcQ4EoMFVGJKRGjoF0yth9EuJ4gyCUeUwm6K1yAY6pgHnDYljwctp6OHA99Oo56QqCz8khAvnSnZKdmt0MBtefKKnfflq2cfUxcVGI0Rx%2BUf6fsbQ83%2BrjFbXtghceumnv5312DkpyOQLcEMxXgWI43f5mUxRtL45FrkUxHnIxgb5X6Uv4ctQ8mRxnA2W3JRaWTuye2zCuVfzw483ePkceCDdyhsV82RIRqIRSUDa8vCSg9ljUnV4z2xymSEyjScQ7DxpZt0B&X-Amz-Signature=5ef2620f03bfd2d7dd4175962c69a58a054d172e9094464933c6c56db4184d6a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

