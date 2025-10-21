---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SJHTKUBD%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T210253Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJIMEYCIQC4gq%2BjhilUiH1scIY1HT8x%2BZdKZNb7U1pxCYll%2BIP6EwIhAI6uX8L2iSIhxo9wzDKwXOdodb942pBUJ1L%2BfEJpmNEzKv8DCB0QABoMNjM3NDIzMTgzODA1Igwo4ShUMi%2BojPoGoPwq3AP3wxNywfGVl19G4ywZplrr9embDndBMqAJ%2FoMyYKTu21h7DYQXCua14hTjx7fNGv0S91%2FH4Cz%2BoOoh9sj0fRmAx5CWt5sCXSq3RVRhdojHjSOSSaRGf8fc8%2F2fdxLOpSChQz0S95X2RT%2FSQ1FvuwwF3pWHRuYR7ZEIWWF9O8DJiuEukI7SGLMlRh2URzBpTkwBLzt6fCQNcnTebDFMy6ZnDrwbUjAledKZAgtRLYwpNfOwPg2cJ8Ti6CgCuHrlUtGBqpNUOSC6iAZqIMTKeiknf0b9KAZ9u%2BAcmNTY1gYOM8U90uVwGmsEY1HUt%2BXr4oyR8vtBVweqEbnT5AyT%2BrfkdL2qVdIV2hC39%2BkMPzVQRW5nbU5FpsALmZcmB4dlFKgcpFrsH%2BIyMo8SbdHg1T6HZEKzXs%2FljSVHee%2BVdX0T2nG7szNJi5Sg5vZftY6pNYGdgozhE%2F%2Bj25Xul%2B1p7mW0nSlgatDVHzdppgwtjkbueSODP8XDqUDMOiH6maLIB0EflZrsmkvMcGEg7rXhFifvdaItqrYZ5KsVrv3MDTpqcPMe9k48EYhU4dJLOgmW0X03rrYKukrrFTXGgTZ%2BcfQfvq4glehdYXGJLp8clCG4Ub1FaIc6nB1wR28RZDDL09%2FHBjqkARWOlFJw%2FY%2FutAzsiazxiL%2FFSUELw2rgTCdqKUJtkGzzI3QscsTt0PVN4zZcLUrEPA9p%2BzS1hkDyCHEWsKVWPycOVwI%2F%2BOEe99VtEPgS2DJVxXFVa%2BAvRsHAZedLXpJOEfQgyHeVZ5VIINum%2Bw6SNifJ7%2FvWLH5jU3mgwjVzZz0Ln185gV08uzp77usxVO1fiTItzvPyDKyn3jhS6ULYm1CCgVEl&X-Amz-Signature=27650d1b1b7febc8909106c390578cb6c5ef2075cc1ee794da3f14b90adcbeff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

