---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VCRTND6C%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJGMEQCIFDHCUQPqQOtLdwfIVdxU0kyA1rXS0p9L8EQhX%2FLKcJPAiBWb6taLUVrbdoJQeX2eqKztQ4KITLEy8hgs0GExdcHJir%2FAwgXEAAaDDYzNzQyMzE4MzgwNSIM8s2YuJ517uoaJis7KtwD1eRb0I7irvgORz1e1uXQdNSL4cCEEur1RFh243fHmai081ZOISlekoaIZ9WiKpDsEJ1pWZNt73g%2By7wTowbK1cDabCg4RXUBLHk45wcmSLl3iXxjVZMc9ljBN1fKLRj0H3gw743fA4aZKDUCEt5p7f8KoCGKsdYkm4hehm0rwQSr4C3dRiaeZ9%2BgrXZpG8sSb2DNTq2UZj3so7aZ1jSEBKSp79OfdqhAHjX4%2FWUh9keQwS3BmFu85Vx8hJbvaEXWL5jTbB2yuiJ3eAMRgkqzsfnIhTAVO%2BJOL%2FqL2wHtXtns94PPkMAc1zVmJFgumET2iY55BndsoWvxBsEaIz5JhgTMKNIlimk%2BYky9ZXML88VpV3if4gkARjooN%2FqPQdYQnFBUjMH9YYxWao29KzFCqpnIR8DzMUMzTJxRDUamTPd0FKuUbKtqvnumhRb9T46pk1%2F%2F4wFQHRLZCE4pRIuxpkBhXpwNwgM1PZxNj2fTJVf4Oa27yTGkEEu03ZLCR51fR8pBwas3sdGBPXo56k5xsiJK1mzvLDjo1fEAKei60U5QsRxym8Gz2tTfF77DgFJSbYYmw2LY0BRqsadmA9RYkfFKzI%2BUU8lXnJtyjwHphIuZwEd6kjK6oKX5Fsgw1MuDyQY6pgGcnpCoTWlL30cv2QfOf1VjsmpeJKXLGkhtQRpbztSZDknsd9BheRitqAXX1i7WELu9TI6XTEJGKzBHa5Y4kryd2cZNlaBT8HvHvbHmpMICAwX8AAdy7ocE3GGVrFxqXO6w62ebI2BB9EvsPM%2FKh6Gm%2BWu%2BVz1zcpjtt46ZCloQUDrpmZSbo8%2ByP3N6x19UnovHGy3cGaz2iRgxkNeAUYk9cxEpp30e&X-Amz-Signature=45be905e2e682da55b57a54fe7493c6b598479abe04ada2c40450d38a6e139ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

