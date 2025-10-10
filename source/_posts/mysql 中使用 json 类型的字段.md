---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVF5FZLX%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T150119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJGMEQCIAFtl8Axq7vH1TLcP8WvBt3umu6tT4nM5YYO94LqZbBJAiBPY98jLuzPzO8pDvP4pGWsq2Jvh4Tct9%2Begrxk0MtcgSqIBAjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMCV7Z99yjNQTnZFxTKtwDKmk%2F6RsQGsEDGSaqd9zce%2FIAUo7UCiZoqGJ6jA5lPoYbxxUKxLhNqNGVob4sFvexf7kVZoZWNqmhY46v1W%2BCNL2bCtFRN6y3g%2FeBoa3lPK0y6niytcv6gHkxYnvm1tBfQs68yZNcrfpUbHJxDYffOH4qy4nNXjqQka0Go%2FLO7G80WHIPjNYw7Jqxkft6IoOiwvPYzPwyeMqZ3cfPbAXCc2CDmMRCPdEtVOjqOum57JS7dO5nk%2Fdhs9QYQUxj91cEmhsdDfUAv3ROqRzl6SlFj6EBSTyVBu5%2FgDW%2FrKQrMQ%2Bu1A8JATnWjrhHsbKKSI%2BdoERBcxh8sFxt5MTq2Ft4rz%2FIS19hJd6a31FSIE6LZDXjIg7xsuc18shjLF3MHkO6h0NR3U3rrqMHRlfHR4GgDigmUo4Xv6fLn1m1CS9QDXs4T8El%2FUFu523z690cmDOB8rt%2BNRzGsp9b7wvNTZ3ln0R9aXYJD4ard45uWf7yO5207tduF%2FO%2B3Mt%2Fpfh%2FQXbGQIACSoyx3lcdENvCJ%2F4z%2BJ3SFONHjghitqYbtoDI%2BkNMTGacTXsOCmmhxcSOJA0BHVMqo8%2FnNHq83GcpWHnsK%2FJe8GL0%2FDovQUY8yQMpRaaEywzLh2ZwQomh8GUwvaakxwY6pgHJwWhMFata47EpTpss3rV4rDgtjUxMtz7BpAr8X%2BLIw1k982AstW1CqLY6qwM3P3vrv3xv0H5B72Z%2BFtq8s3RFUwjhqHrTuCQGprvbiZ6%2FKbbvDJNN52YrNta6zelml9GeKOmFBq1g3TwVtBOHynTdyOsphJjVNyjn8%2FU2ve7RBGYIEEIkNwi7NvSUHtbWyEofasjvPfHpN%2FTA32wMwfZFaCup95Gg&X-Amz-Signature=51e5d23f2cc136fded6000c6e6de90761688130f07ec9ea3a254d2b7d175fb27&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

