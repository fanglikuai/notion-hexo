---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X3N6GOVQ%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJHMEUCIQCkd03GgzgF9wSebhtPB5%2Fjy9JsZkPYgm0idlXdrfz6NgIgNnNXZAGv37T4SyuaKs4ZSO5s9Lw%2FHmEU4jSgEeIt1joq%2FwMIERAAGgw2Mzc0MjMxODM4MDUiDGMKn4i5Xy0i30qatSrcA34owsHcX509GFLMUAHzckgggAUg%2FQmX8w2e2Hhtv6%2BQr%2Fp8yVZHZNGEVesbPR%2BkUDUJgRK7dzBwEiku6S127ubjuCoe8GV466Vd8IJu3wqt%2B0SByQlX1bYTkkX0FHBup%2BNWCfKdLiyz4RWXe9ojH8yfp2WJpQ6sfQdlZF0DiRP1JSF20qtUiXJu3Lme%2BgjDlNy7RTIKiE3nB8dPWJjUGraw%2FdrLXwdYSqGoXJmddHWoLpXUnw2XRW%2FZe8c75H2mpK7jgey7vZDmk4YxSBWkxssmHCYkUZTki%2BYVZy%2FX073LdkVn1Qc4WHMhK2lyaSnvqB4DoDRa0nTTyxyWTyPhFQzvgqk8qoX5uMuAhgdbXBUsd99XbB%2FMBggzEoVVhBaPiVjSZJi9t74DJ9IRJ6o3RNAg9IUukDovHIAPh3QHLiP7bgnPHL%2FU6u3ywuLanUMkQkQ%2B7Ke5OKhUQ4YbdoPcsytNkhsyCWGgUoduPzHBT24emfJXHEa34C4iU%2FTD3z5FH2uiEJa2wq3JB2%2F4syV9z8PB4cgUFThONYW2JeBenRe6w9oJ6yALmw3ebKa48mxqhOz1Rjkxi%2Bdbu4ZmXOelr7amusgDGitR%2F139ydJ7HHnHz8dl4iLNZ9bO4xunMNmKgskGOqUBUpJU7rfyhRqSD0IrM0cduLjb7Hmn6EFVLQSioor%2BKn3ITAyOPy9Pf%2FM5dVUvQXP6o1mfB4QoUUXcFbMhMR9dWqskWMGBRn26p7z%2Btk%2B2svXlMHcsxFiaQnHG7bfoBa3WmOZWP%2ByM%2FM%2FY9sVpqR92rVVUIHg9o30OwX2TcD4a9lU2ntdXm9YtxwnGFUHMRxsUtY5aXoMsocJ%2BR1fcghxMH6idEpB%2F&X-Amz-Signature=978378129ba8f7ad39ad860160989387edbe9b8977b847eb1a4c4ef257f46b9c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

