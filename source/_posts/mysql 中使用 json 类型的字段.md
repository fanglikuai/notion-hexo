---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VWBVD5OB%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T140055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBfVoPJ3QVeQ5cLv81d1cT2cYgLrVD9I%2BoKGYEtSTMWNAiEAvJ3B%2FH262brw1mLPy2Uz84%2FJR9QqWPiEdFkO1QsM0aEqiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDECaz5Upay1jv8MnGyrcA%2BDXmzFXC8NuCSFavyvahkaJhvHltB0a2yEQqqNwFgMxrE%2F3b6DD6xW9PmlE5KPfiaJC%2BQP%2BIlTiPuW%2FcLLxs7I7O9WMAWLRTuU17fBsAuVH%2Fz2WwQWROs26Bj%2FCQ0XqwUJLFyliL7Nskp%2FLL00HfUYmSVf3YFiNxb0V304F02DM3EMXA%2BZcWKUbq6uyzk4Pov8iYFNqnT9O9hBsdsAfOFqgr2zeKLNPxWMyAMHt1df0DB8Q5UT6fKzTH0UeVpe1F%2FSVYknllmJJQDRAzT%2F7%2BAsDItm3gH8cJJQZgURMxKk7KD1fs8Dk5G%2B7h2cUnvfSTSsapolYo25bAiVAWZusvm%2Fbs9qTnmO9HwwiAYxnMtnNeVUj3KbC45deEXWqoojhUAgSrDOE997LFjV%2FvYEa%2BUp7gRTLbnYX7O0IUEkWMVMcHqxVxqTwNwKTn76QJWDyJIWhfck4K1lIzjmJ9GeGcjclo7BPCdsRjPjSqFmXOu65n9m29pCIcc2IDyH1%2Fpe%2FI%2FNwJK6cKATzTQfpbtPqAR62MUpCXMUqyGVOQiiaEU5xqU7ATxzoSl%2B5%2B5nhKY%2BK7PMZa5z5uA5h1l72%2F30BqLxtEvK%2FUonjnJzyvTmDLBzMImMCVbhamNVhCXpYMOrJ7MgGOqUBRghCzS9OjpqRDBkIW%2Fcfz3cicSnvIv034QO44b1ABrYqX%2B9003PgYptGRSJ2gxcp3fzENzf5ByaGDn%2Fjc4xz6B%2FraVSyDYN8%2BgHN3Ug%2FABUIDrnP0CmUmZXIjb740droiDKlOg591s4ND0QbbobclAIPq%2BoqZgWkBnT2evga0abHL4v0LWDn0PqWqT03MV%2BBiVn4hmmf4HWV4qGNcj%2Bpshj%2F2Wbq&X-Amz-Signature=0b54872cb06de6aa1fdce8ef60669a49d7424a4fdbfb5b278cad989fcf72fe1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

