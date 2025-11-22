---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YLQDJLI%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIDuj2KcDW4U3G9%2FBZ%2BhlVYJ3aPgSejf1VFpp1H1slEarAiEA3A%2B91E%2BPM%2Bl614iCKRopdmr%2Bhj3r5JAde%2BVajMXpDZMq%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDAV3%2F37px%2Fyx6qnpvCrcA6zINM1%2F8Nl9wvtDRR%2BNeJ8WdwMA8io9B5mjIVqIa4cD6AeR5YjOxKluac3y9XuYYlYhz9Roh%2FfTW0ocWkV1xnx1aMBCVriuPsARjZUgk9EnEd7jSuwUkQSjKsaWGADy4qo%2FNMsXtzFiKErcjoevPi%2BDhqyVsoT2Wl6DN1nCWGf5Up%2FfI6uEvGJaZB3kP3Qg7Zyz7aIIUq1ICW9hBnRPKT3ibXEWOyfwAwCE6tIEOx0W5STKF5NPjYGUWPN6NWDCjfGhAtoRAwAYTcq0%2BMWmR3sFMZKsY4xWFL5GXJMnuyZsRq9bflpfJA2u7x0rDn3%2FQgq9gZZ%2BWrIlaU8waOhSCqAvBLeM0h5H%2Ff39urIzr%2FqDJYkVApFMVlI6wO96Gol98fHMqyFvx5oGGis%2F4FhaEhN9ccQaQBf8xC3dYlPUuMHQ2vS0twYVPQISEDCherKkBJNme1ZDqZWqT4%2FxkroZGbmejqwS2g1yE0kCvuybNH3P2NNjHXl8GUrZKlRKc9BhsPQXvU93b4HA0xC%2BHXDPlgkRzCPAPfHf9WCILVhUmns4P1IOrfa8PCrpc22FgQlfv8%2FKS7EQDF7tNq%2BVzPseWaDGuTtwwD99zjUvsYjKy62YEDLZgLo1jKCpRk%2BsMP%2FFiMkGOqUBrtwrht0Jp8Je3hB2qiDYhusStsi2UAJ7VZV2lmSTeb1e%2BVMBOG62I3DBSTrRqXwgdZH%2FBQx6bIehluisBRy90blCTu9k62%2FoUOijRqq0AsyobwizhSilhG%2FS%2FSUJC%2B6f5BIbmoF3aH3gMT2jBtiQfvbZoKNlevS0CsHhofmsLasOA9Wr61ERhJcufL0I1sR7MXfKlNJW1157CJ8xGjy%2FaMWiQaxC&X-Amz-Signature=2f27ab5f78a8406b647a1cc661969c59e4a181c7d752bd6e1193c3206b658a64&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

