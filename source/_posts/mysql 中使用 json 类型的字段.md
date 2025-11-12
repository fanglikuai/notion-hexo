---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664O4AEQ52%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T130100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQCI4bwGC9ULNDmSaorFu9Bpiwf6BiR0N2J0co%2BEfj7YEQIhAOlT6wf1HoFhpN9CbIsJunudLwBOxvaBwLZbOY5OF3GaKv8DCDYQABoMNjM3NDIzMTgzODA1Igz64M%2FNse4MBUjKWFMq3AOxMXn6psZxVkJzxuF4vINzsUi8YzjNqzkckAXL1ycjRuNbkwqcOOO78jL0NZGkp7VjvY4RdhGw139qIbkpvssphTmj3ppXoROizZVyoDGHU7gh50vN1%2Fl041rhcmxFkn2ZuAv8HOnB7CHDJCzI13l4Dtrq08b%2Bx6CqQY%2FKIz%2BuCxVa5KEagWsYAkxAdc0qdPjVQMeef7m%2FIXG7Wdq5A5mPBW5eQfPZmBSJWv4cVS%2FhesgZGC0L3qj7JyTOlPPLpFlLkZQ0wPnh0gnCoDjVyyJ3SlEi5M6wrkLp6gVVLJIeOGq0zGcT%2FGGz0ktsTCAcvRZiauq3rR%2BRGdKWCA9FBwf%2BjaY4JF%2Bwigex1fAwP44cfuVeFzoLwQdB%2Fqi9QQTmOK4aC27NyAsm7ZkEMZaGnTSA7fo%2BkTFKwQA%2FrJJ7tH1SLr6BM2hsvFZp8byVNRV%2FWxqykW6gCRXEsOQP8BS%2FjoHSjBgXBJQlDNjFZPw5TyNjEJ7Fwignm9hvPjIypdrCWTuewbQYCsw56ITM%2B68ai%2F7lqGDAk%2FU1CdyomBr2gVQsQi5grvcZ84Sh88ffQAuq84Rr49XoIAkSz2Dj4QzMnY%2FVBNhwW%2FYoX8rm5GEVdzJWoyRs73lcW9frYkaixTCKgtLIBjqkAcl4gCufrs2VpQ7CBxMgMW9Nb0GfB5hxJpJ%2FFFzrmY8pRsbGspQBc4CewJR79YjVoxttlulRtb7cahR%2FHHXbbffnRnrXAeWaCv9KfTofwG%2BwpI6YKv4RlJ2H7n2xAk4CRnHIIFTI8ObLhodEk%2BP0qz%2B5atEkVy8oAgdqIzT7Wq8cYVy5XTExke4uXDMWfQF9ZDLC2W%2FMivPAxPM%2FCotC4JmeTRdK&X-Amz-Signature=bad3edff844e13a0f53f4336dad0046b4b129116143e9229e1ef0871cae4a48d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

