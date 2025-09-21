---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TTLF4CJO%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBQqfDnFKkxbaAAEa%2F9gqn8qNn2zSc785WoCNayzGLJKAiB2iDnL%2F88y7aOSSEZL6DKrx65j%2Bvgt8fAHvWBw5i0pxir%2FAwgaEAAaDDYzNzQyMzE4MzgwNSIMp1la1xE4HOuETwNhKtwDprSmcqPNsxhRjyT%2BhCNjc39ZGZAlayYsSOYEuJTqPuFR1pZbqa8tnovzH5IPzUsWKN%2BhkUQFJFunh62nQrT0jUH%2FN%2FyET1dmVscAfEqhJ9VGyrnoR%2FH0GQvqR%2B3DD%2FGzO0oBJI6ZwxiDQtXMTwqpFNcNLw0sXIRvM5i1zkaId%2FOwVxMdtQQi5Um0mkVTnfIt62qMuKZZUMEg%2FGopjfax9cBRo%2FDj%2FTqGzvTQrr2tW5WSjKuKsp1J67TQWOdSYqYYGFEcuqK2pImq30%2BRJV0IRnDsTPW%2FPKnydCTuUJBCXDdU7gSFrLoSJ%2BRfJeieMI8CEvkYfox227uGdyXmyZ8TdWGOMeMsHdMFBOKNIQeTrcdI%2FtR0vZXP03Xn7aWTDAj%2B0SqhIe%2F6ibsVQrttqnDviJ8mhznWwKQXkKs%2FT8QXhDzHu11fV3utjBe0fIxGOFAWy6IJlPwp%2F6crDngIUdFj1dNVle6FROaTD3T%2BGGUXvxHdYMnE78OgZxSFvPb9acj6K7wtiu8ElH0ax7%2FtXTjMGH2XJhpt8Lyt1JG3kLd6b%2BPLgPoRUMBmnKchSqPAtlu8QCCGO1FMN%2Bt0LqupMHVt4GxveavdfazERm%2FzU2JlhSd6ZG%2BIWZAjcfsYSsgwvOjAxgY6pgFLiQA%2FXl%2BWZrwASni8ppc2I2JXDau9iD3ejiAViZPcl8PVazl%2FexAqXDh%2ByRCD3h1MsSx0Kn4NT90VDBXH8%2BV5zXD5rCkPF45b8J80OEN%2B1%2FzjMPE9ZnJfuKQbZPA7CoyxnuFMVaJVEB00tt%2BlEQw%2BMweX%2BBfuilbZVWtMezshbMMo29OzNO9jAjXKSzdPnxQGz5NVmQ%2BaT44CbDMUigZJ3HFRE2X7&X-Amz-Signature=fff0897fa48c90de5640471d7f5a182b73fd55c4f518efb20b8442a5109c3451&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

