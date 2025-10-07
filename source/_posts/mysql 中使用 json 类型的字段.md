---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDZESLIR%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T060140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJHMEUCIQCvq4JyN%2FxH7obRSWD6qUqW%2BwOE5b9yTyRcZUCYa%2Bj22AIgCbZSv1RAJw5s499Ktvv4teSXqoovYfMPCvuqIm2X5vIqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPooHboPAvovkYAPASrcA%2FBTD2YQSstk%2BGRVH03I%2FhvM6SL0ztfkL5TThXR6cUDHEcsj37zg05dPwcUzsAENNshJZ6LYBqsRwYwOR%2BynGBb5hFQp1GC11iP5txsdsvehQWDekC4ZJUkIKHYK2%2BsrP%2BWlEZltWAO5e9XrSzHdhhrsmE26EuYhAHxYZVdZtfSqyg9LDFdCf3GQ5UNwaSfEPCJ2HI%2FuEwew4Yi4qAqbpaAwDJJF3GKowDigWZfCYPCwFEfWQwsSdDzdfo%2BimZslzByhZ96SkfPhO7a9268QB9eujMB5%2BRBfPIioUPJJU5%2Bz7DtZtqo%2FzV6BrluG4T3GBYt7%2F0s7uMuAyAWTRbtDjOYfjDPDnN%2BBQ13w5FXaFMVKK3ubHwI6av%2FmpVYJwp3r2meEYL4j4fo8mc0rbFSGvRMj0iZa8bfim4NGtXNgsbJCG9lTfuTizj%2Fydk8W1GAXwVBWwQc0S66PN1Cx7%2FZquXC5Uq563rzxM1ZDacBa%2FSQaZHcSbUhzWiVUmxv1C5sqBhueQTGHIQLBT5mlBjVzTync%2FU87EoammUdGBdjQosjWBy2YXr5%2FAiqx7ChHXSa1GhB%2BF6UaTYYcvsgqaGu9YvJjUHtm%2F01J0subgUAE5J5D3GoTR0WufghjInIMMILSkscGOqUBp8LyOxkWoXLk8aXC8QQXzxpmtpALslBdJ1FJxerf0wzfradD5DDT5IEDQvWC9V3Wn%2FqeR4HxMBAohlvCGpdR2BL4hSl%2FKa3gpsSRB20oUuZgfoETHBEm6UCf8D1N9JGMP%2FUkx0fcn1v7%2F8IQDHrMtO8f9C6YDnylhMWtjhOUBieKrlmMIRhFDw2qRu7FaJlaNx4NTrlbm538HM96DqPKqfM2Pk6Q&X-Amz-Signature=987f607ec9e85f6a0d8e6ed4a1ad3022f6b99b5dec45416dcc33dcec71d9221b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

