---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7YEIDIQ%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T070038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJGMEQCIE99EJtcXpM3shgJeHjnNEGCPPp5B%2BFEjZFR8DmlxylgAiA%2Byw5L4q0yRaqv%2FGY0Y%2BiQXvqu3pH2ov2Aj1HptXoUXyqIBAj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0CuOAyAS6QjQp%2BoWKtwDLQ1%2F6Mkg9bpz4sT%2B3h6h8Q58T%2BSxJn72LN9KGxhGu0r7m7YeVTmhL09F7NTCo9xtuLKnVvYwv4O3xZw%2FQOh4ACnXT79vgMJcwQ%2FxSOY89GkehK7JOPp5XIKdgqqWoDIFGhmNXq%2FC%2BmrnEFQm9KC2UUWf7%2FcCk%2FbW6p3ZudzjJgYdoxfmbJnv7CECUFHpAICyqNlbB2Za%2FjGJ3hdUgo%2FaD9%2FmVKD28aYMPTNPbD2j%2BtRA%2Bjcfn7cLOL%2BB%2BJt8Gy5F%2B97x7Mmzl69cEk4rqWmDO4wCHFdhSNdIDsPu9MNj4Rkdk1KWPAnbLOvfwRORjdFE0Ni8Cy1opS32xwqvfA0naRCYabCwNt36zlxLtwOxbwiohLy8kpiHsU2Hx0LCrRFEZhbtEeMRe58iWlzY0n3yrwyX9j6Iuy%2F2ZszT3rhhAw92Rz32v9X3Do4BL4yQL%2FrQJDomUX7lc0K39xfaKcxfzMdnWsZWOK%2FBPu9MUR35dCBrU00Ia5RwdXKm4BRLwMqI0DfsLhNmT%2B7lzTj2cTBNL58UKN5EBmptbFE9Q8lORH5JNlI09lNWxgKhuNeTt2sNu7UW7wRg3oNVoSczlb%2FNc2exjwpLt44YiCQ%2Bs5TK%2Bi%2BpfcTtFQOt%2BnY6GSEwz%2BKnxwY6pgEY4LVB1mCa5TNW5blaD6EX9%2FgbGmP7piPEknJwUjvwu4pjMwbTGBg6fo%2BIN8KaAAaJCGaxyHxwAJyEXcKruRoi4YyB6j%2BqpsPe1iI8RGw4KVPjYQeJfJiMQDaMSwaYfPwcJK5UKo4zXvRieAiahYY6QtyxsDmSpTh0kHcc0Ng7R1TJ2FrY0O7bmkkgAtsUwqYi0u33Rw8hYJKSWBiTHStbG%2B%2FOaxAs&X-Amz-Signature=bf0cc8fd0ea2dd59b26f6f4b80fe9d93daabecfe43a48561cfd552bd026bd5e9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

