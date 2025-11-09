---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QGYHCXCO%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJHMEUCIQDT5FPE22QYV%2B4fDZ%2FNNyh8xVp1GcwkDrjFwKXFmiIq4AIgFtswhwL%2Bd3vtuL%2Bd%2Fm5VuhLgifZI7SoULVaZV8qSsdQqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDNdsmnvrQMoSQzMGircA5JaXTUOPSl%2FRfjwPZFk5IFEhFZe6zm2e9%2Ba1e8MqPAw%2FQMH17F8tmSJAlukjhSgm4lLLw6cDm070zGIHifMHVBBcP%2BZUasTnq8chdoxuM0pdDYmNh43VKsq%2B%2FntFplNp3RcaSLJXLZNtOANchqanXIZAfrsPASsdDidY%2Fifv5crx20qkIAW6wUr0K%2FUZezcnzlYNbySMISz1X2QaoZa23%2Ba6PNAlpbDHZ8V%2F9iPv8A8drD5C2wU6YzBGf1K0lMsomCaAMcu0nO4bebuUl2%2F4%2FlY4Stkj8Ik1Aw0rSgYedI9Dhlh%2F4Zo2pFJ%2B2NfsadSbdAI1L1WnqE1YH%2BkzLlpo6KGwSdKY4DMuUV6zutcWcM7DRDXP91bROO7JWYmbz2OL5izvdfQI0riXrJ5%2B4okR9ZKCFTpoQPISw8ANrem4j5JB1wAvnipQwT3fbi6wzhVK8e17XWNzVUfJBdup%2Bgu4M%2Bho3uuJ4UsuacSuqShyzBgOz3WUEx%2FaYDH6PGExSfWb6iGCDsQGQVR2LF6JZ4R%2FI1x1meBoGb5b%2B6JmHAwyxqFJJZPvQlg9VKeX7m4sI3AGHel7bwLXrxuobZxxXLcSaDyK7H0CoIlCr49w7dA4%2F9h83u%2Ffu8pRcklCHcYMNeAw8gGOqUB4wH9L5bHymILTWiAiWBnbPJxBAVFjldzW%2FsGbLkeL1BvQWce%2Frra4PcbHuBIzGZ2d2OBjwdG4h%2F2YmEwxs%2F%2F6n7CXd7X5AtSsX75bFrDfKbM6rlJvXed5BaeM2bWC7dz4B2JqJpn%2B7sRD8oUTVIsWUgnW4NpvZQ8nCRzuZEMaY6odUbfZuB2d5nXrVwGLIvOztODo%2B7sJud%2BTFoOUlC3A3c0Kp3X&X-Amz-Signature=6bbcdd22a9891b0b7ef1805a2ed54fe6fef575d34481ee7f8f2dda31307336fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

