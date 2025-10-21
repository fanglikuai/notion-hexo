---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VJUQRME%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T090101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJIMEYCIQCC6frgrB2IX9WPpkwezsMWwm02cX9noyU5jLGFQmMb2wIhAOa6Lm2H2dnHYiSRLYPN3NY%2BXLRGfoSybCD1WFQ0veKiKv8DCBIQABoMNjM3NDIzMTgzODA1IgwRK7bE8i7m2Jep9Ogq3AM5fyHBx%2F74QdnLYH2oK8DnN5N6xgVDgL2GdzvF%2F88LooDq%2BSClv%2BE5L4DyUh1MCNREgZfQuymIVzcxdigd%2FjEx2qDLwpTAL7LLNzhXQw3aOK%2B7SXQkJIUNE5piNMz8xXbJKIocsECMto7ct6r6r7BPggOmZdi5TgubQt28ToZxy1lGwvOSAR1rxXQ7KrZkIbdw34UOVRrf65izZVMtPMbfxNMPKsLrZB9uXBfmZG%2BRL9HGHQhwFYH%2F8YXFYQ1sZKzRKklYZhufLqYlMMskwCEIu%2B9OVX2LYwhiV5q5JHLyL6VZ0boc5RcYhUa4pfW3qFTYXJwYnZUI249ZUBIpFPBGPMz0kyUm9bvcMRodsIc9gPwg31MWDaAuZbTQOff9cE1y7El%2FOhCMLvLlj4IWP9iwJLTfrlBzGosKQ1SG4ChJA7MW56v2hABRU%2FunVIu1CtxOMQjOq7MQsxjY%2FBu6H2r0W%2FfI5lvaXPekMoqserJG%2BM%2B9w2hxMLA7ZSp9Aj30KDG0JMftQUHH2uQEZR%2Fq8mW1VcYUkYYbeP7vtVqqRY2OnYGdNYwjdesf80mFpAhgoaES1HOaNTM0wx8qHgYHJ5gqJbSHlDzwUfv4TdxXfxfSaT4NJmLQYoMVsN%2F%2FqTCElN3HBjqkAX5zdPPWGCEDXkpcTOugkB%2BGeum%2Fa0%2Fgf7xjdRwL2SfPtroJDxdqtP8mct6uIa0ufou1RonoWZwXiF1OuFnr4NXVd2%2BMSP4KBsHhAcQN14%2FQNiN4rRnx3YlMftC2uCi3kxzv7BRz9I0Pt4w7JETouYVh38QSyJv41%2BrQyu3s990lLsIH8cCsW%2FNCnOz0u7OtiXsN6zTifjq65cnKC4TZRbwzgUgC&X-Amz-Signature=5782498929a96531b2960916ae6e58b9147e5ea8286a490c306c54a542e89cd0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

