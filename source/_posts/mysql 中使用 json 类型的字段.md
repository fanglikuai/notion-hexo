---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663Y7FOWEW%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJGMEQCIGE0ylWi7ifEERfqTWrbmPHMf7BkGuoQUYJ7VjHmKndAAiBcQEg0jbE5afSJiRka%2FQP9J%2FOmUgWPmQV6TzRsqkm0dCqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2%2Fyef%2BWPAIH1NQglKtwDrO%2FMdsNQJ2ixtfznGe3o9h%2BFIuIBZQjgtHYZbm%2BBINpHI6usB9qMUzFShm8DJVx5bXwU3AXl7nTe46%2FLNrEgsBneYz%2BBHaaZqi%2BKHVP%2BTmDUINxUvfPwjtGJ%2BnhSzTEZrjxNqIGfw7P6pNg0EXZGuI44yB1s85uBZusyHJdrTwAt6m0xLJUvjfFhDSP8l3HfKqGLLhb32KseninMSeB6A7RYdA35BuLnkjdR0%2FE0HWOOcEENKYncizrGR3nNaVbjc%2BnzgjXJJSjkcpne9aFNEV4vKyTthhOVUuMzCsg2a0oMS06B4arEklU365Ci4sOjHbpGbuvfTQ74P6VN6m4mrRMb57zMJbq8qv5yH2iJczJ%2BHYZ%2FWvAr3iXfZHRawQysGNurvWt4p9K6Ft9dlYNN6%2Byh0A3iIGX3U7fONP%2FTq9NIGQwULR3gg08W%2FDyI6TRO3y80L%2FaaxbivI3oRtA9lEqoq%2FO63xXdHPcywHJaMJmrYEDpwK2XkV2NJyEZXzs89VuT5cEhKo%2BPC%2Bt9bkZuPr%2FoSSD0xDvE9hNTGp9%2F%2BgGcqN9olWbFXF8FK4hHVvNheV1pxV0HvXnWrxllKi4pZmI383rie%2FrW5K4pFKnConvNbuZBTiQaKQqxPsNwwwKC2xgY6pgEw7pr3SJxfW98OLMpfza2%2BofEoThDOp3JFPwqADd%2FOznoGnbp3RHg06l2u8KbymyLGU8CMSdqXEwz4xXekz3%2FyfherAnvWtWGxOt9PHFKa9MT57VKX7QRra6mjAsjCFTBBMQ0ccCZvrt%2FQP1Z0qMvoJuMR2FQaoPDOknenWpkxRrBf0buCt5BgdWYlcmX45osLRqng6iR2Fk4dQyuu8GMlsEfxfZ1s&X-Amz-Signature=4852ead601217ef30a57f9f2919375bfa877b7da1384f15b1506dcdf96d00006&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

