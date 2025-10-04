---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSWQ5O56%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDsmHmYDsWfszG9FaaA1IC1Nt0dXZ%2FhnPWqSoMNhXDaTwIhAI6ednnCFXWwQeuXW10lqWslg%2BtpzvMlnZQ5aWc3xxFJKv8DCGEQABoMNjM3NDIzMTgzODA1IgzPJd3gc4WvGcYA43Eq3AMeJypH7yWLinYDIOhhOgHERHwxnzTY%2F2xRdyvMaS0Zmrg52g6b4SunQDCHLdnm7N6tRcaxaWQkcA5aQdTPwM8U91OzvIH13MEJNjb%2F3ubDTE7yyztyWwSUkBm3%2BvibBJV%2BlRPS%2Bh5thXScuP%2BzsH0esOX3VcvSS6utHRTmDVjkva8WuTHjbCYGUEKOa8pBdd09xGUpmUp6i1cRJMPXqTl3%2FY4qXzikyapwCQKQ5Of9pAQnSU5tZi5zcWVEyIciUxr0kX4mFUdhAaYa%2Bh52f7fZtCMAK1yik1HCqykw5MgpiN8kI02CNnDeU2VVbtcpQYy6sUl8%2BLBPb5NqJqKWFL6955%2Bd6SxaltxgYeeEwWRuxagi%2FY%2BC0MSRBGayLo%2BOeoUXJPZ0hKU8O8SZ7B2vte24roY9vgTkKnApK%2BkCvJsj4QhEEF729qti6NZbpS5l6X1yCNnR0KRZdnMwI5VhfuGoYsqAmWO2ZWVDPsysn8InrY4H6jhsqjMY78EDoLmBUrkGOc4XHWBolN4UNk2mTytT4xgFQf%2Bj2JT4qrGAD%2BE2J5tbxUgDj0ohnFp74GprNOPgwjaBbfDLF25yRndXmMMRGMk70BUpOQxzmqltjV99BZhJiT78mQLvnvcycTC4kIXHBjqkASPa0URqAJv0Xgy%2FjOOzBA4VFFh%2BIbf3tpplv4zAjsH82Kl0%2FsBElm1caN87IEtb60FLxC5F%2FKrOCvygUlL%2FoCsH9DLJF2xlRs%2BOeF26OuDoGgyXDdoFfxJGbgNhUSnvqx44lKNH56ER82YNGhQ5UdRIwe1O%2BIt438FnIq34oeFdmBKmje6%2BTHk%2Fn9JxXm%2FWuul1X%2BH4b29ByHcy%2BbxeKEUGogsA&X-Amz-Signature=21956f9efbabe8a395460e55e59ce112e17699c08526ffc909256eceac342599&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

