---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q2GTEQOK%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJGMEQCIBy%2FUS6kJr6EfZgN7hB4c2pjuzy0TVQN8iIcKVzWC3LcAiBnWZqcq%2BEls6W%2BNR1Pwf1CXh4H0%2BkUP8BOuj%2FSUSgKSSqIBAjp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDycghE%2BkQUrda45JKtwDicsfEF%2FAoMruYMqDOnyznTatTb4udYiDYnl0sXunD1TzxmeDU%2Fad%2BgyJRjaIZg3kLbX6zzU5cOJA4PdsbYBUOSJBnuE3GJ8AgKiAy50rVeESLDf6%2FpILn0UgWv%2FncLHFUhKU2vhf2Ul6AkfHYqL86jY1NXkkzv%2B5ywdxKm%2BwD5Soc3aP9fCj1cygXfOHM2nAv2Uwo9bo1nQs7Jw4S4lmy01x12dSAa7uJM%2FvskifLc%2FRl0Hzo36NR8EwoVC6N7czjXEYR5P7h255exXqJWP1ZZz0mJBTMORVXM9XCU%2FP0Fw1aJDXs4HjJmRGDczK4KpXBDL5YXc8sf269fVlAqODMnYp0hKmgIRzfAzWVEYzi4%2FybU8GS1CgQBqu7SsALgd9oI852bQaQzCC3ysIUheeh3oNM5UFaw55O%2Fs4%2FZOpSkpYZKqkACWPtA3VvFRKuj%2Fml083lWR1QqB8OBGMebC8JbGqQ1GZwqJS4HM3BKzKyTVvKg3FE8%2FoSvoj5ekn%2BzsctAkpmGsiPmkaG%2F5U7pb5G2swIQ5QvFaZgpUY1nPk57jw646DO1CaWgnYxtYXkmsaeOPs6m8dh4NDLdkQDXCuKhs6a6IwwQ3zjTjhC6q9Au0%2Bu8a4PQFjuzpg4Y8w%2F7SMyAY6pgE6RR%2Bx6CgR%2FruVOi8s3B7CEVf9MoQZplkAQfPZBWGBdusgzq42xQYqrIUFo%2Fh4lVIzevAeWEknnw%2F4oAajdbitA2kSSo4%2FpeGLx2jZap9MpI2irlwTaJXEP9ILK%2FdepRJWg8cxzlC%2BTERYgW%2FqNm9cwfcQNCX3lMyz1sEY%2FhFekxX8a42sXWHiZ1%2F6ZPTdE4RVwgLMeuT2AdOhBtq2fkBzXZT5j8dY&X-Amz-Signature=7b546b2577af4fb4a30be14eead2d40ca7f124ae0c2984f303c715312762fd95&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

