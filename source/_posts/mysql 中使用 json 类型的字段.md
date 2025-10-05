---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YE4SVVNI%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAjhyuTWcfUQ2Zazo9tn2AUS4iGNpW3Zl%2FFQ0gmqErgzAiEApgdEK1ATfHtARBieHZXu9GzKNUslyBXRuqNbeTWN4Icq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDJOFqEgxzBPeizYP7CrcA6m3wShbC%2Fux6oy7jhIBUO9iANEblQLvH4G87wnRnGGUvmKglKAPjY9O7vwRoRVdhrx7pPAPkucFDMYdMVFyciquDg9pvPzntFbM2pgLTCcto6PALQDfVwCtHJfufG2mxUc7XVu2wcxWgfLK0F47Xy8FFDWlEls6JW%2FEV0zscKNQOG9lunR%2B21fJqqXM7nH9CK47yNVYUarovI1FxrTD8EUKnGC3xJohsW1BP1exhTnEn18hBev4SsTcrww9ow6xbpM34k0QEEsYk0SUexFHEV0h4o35lqweIq%2BA7Z9oWDEzqtcN09Ez5Gn4KSpI6jW5giL6BTiWgLlliMWKYa1wJ3vb0h9Md6TKnSf4cGuSVedFVhJOQfxru3h12wNp0RDGIo9EciF7FTRvuqFdV6Y5%2Fd8yO7F%2FwjTAVbfLymMmB78WrLTZ4LEvXR2h5mzap%2FP0zUrTTqzLqnJyKzFf0WNaBD45WkRJIVpH1BZiLEQuVtTj1%2BfOLXkzBbv6qc%2BuFNBxNeCVpXyz8TdZywyUHQlNU6qx%2F8S%2FLhiFhRSyRtRELPgprj50imwzo8OY0y6hdoWRWZOn2bz3CoqrLjTaHQ0qoK89rxQiDo1Oi56T1FG%2Baa23Sdwg5W%2FX1gYxb4GrMNfhhscGOqUB3Gn4xG20bLbNHlqcrTxmdz5J2oWDPvIaFI0gvPZNORuhsf3%2FKKLSMXRNraTfL8l%2F5IEYHvCaM5TNB5UVkbElJ0Xx%2BzxWyXdKCne%2B8NuewvzPapRC20K7ruSkx%2F%2BiUETTqZEKh41wZBxpsItryxHetiO%2F7%2Bdjn7lW3MSxdDVWmyAOE%2BjSpla4S%2FsaC%2FDOj7Dzm6tiAHcCxxoQCWNmL0wOUUqtLcgc&X-Amz-Signature=52527e1be3c14745d416679fb98dda9b36de76476f60707bd7b7ccffb3686a9d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

