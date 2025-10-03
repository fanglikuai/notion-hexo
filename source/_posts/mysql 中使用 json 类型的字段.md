---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665IT2KLBA%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEKf15HoMDxjdOtqjmyld%2Bd%2Bw8xFc7v5d5o3l60QPbR0AiEA74n%2BbAGMkpHUBFbTM5KvFTdaP92xWsVaF0OedXPHiwsq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDCiKoyX0e0rjP44C6SrcAwTR0LFVCpCWVbzsRKuNks2%2BsjkNYoz2t58pWnA6dQxgpnYfweWBVUVB5IjqWmmJ9TZ7D1TWuDzWTrYD4GS2AIUbcrYWINeAziIRUMoaeBchJCGfUbGeSJQo8qfn26LLWZW1FdhYJTUUjo0l9Gg1tZ1b0xpi3fF3AkQf62qJb84cvdqkFY86z6MXaDmGf7tCxUkaAJJ4Henb72O1IUJK78P6W4gjLhKxUy4AIndiPPB780QtZJ4MRQGYxk2SfX0rg9mctNHWCyM9jHTE9U%2BOS4lrnU2aJrmEHEM6LET4FVbNdHT8TXghyMxrh6%2BY0hj3xFmbOWS8wj%2Bisl2RR4Axi%2ByfHAPJS5mUKlo8vPjKA4qoIJE%2F5mN7vPirf0V0nEEUy9dujgF%2BBrAsaMpandBP4EJB9XIY93Zv89a2ig7RChDfXalwTxEfaufJA4j1Bn1k41QXQKwvrDPeixMC9ZDMLGfUHle76ulv29KJuXRnaYfuiEhA46IxUWVC9YMt9r08d3V2OcGTBX%2FpbLyUAxpXtyEzqrDEmUDIMvM53gPGUiW9U1VXFgF6f7nvx8fyM%2FtHH8GlQxHRzT0oNdZHVrmH3FnFbBwvCpJuJpXZRk%2BxoCqpiRAuiDOe5sxW4KHEMODS%2F8YGOqUBkdGvixIrd7%2BUkiQyIlJa8pEhJ857U2KNIg9kbgvl7YoEAtPiXs2W829bg2at3oVluuACn%2FFZMSjomnawYxnbSGZ%2FzKzrzp4R1WVHC9kOM5vV6fyru7sGkyaOd103MQHcg3Y0II8R6gtTTKaUiKzgrYev4KLe6IlTTcTobPlqnGjlkGa6UqqjfcAPLIF%2BjMGphnzBwYmFypQDqC0ZBCrHruddNBZj&X-Amz-Signature=b1151f96102fcd747aa75bbe8bcb6f3099bcb38144750fa4aad2df7875df21f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

