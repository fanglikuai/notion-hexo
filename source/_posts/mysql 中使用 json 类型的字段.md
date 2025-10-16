---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZZPTKGLK%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T220041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG7AYbbNFKU5foKj82GNW7TovTKUoK2dMnBh68479EFEAiBMF9jwZbfOG2Ohdljn2v89UcKUltg67BvjSrsHcp%2FJFiqIBAiW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMolD%2BKdyAp7NTb4TEKtwDhpjRdWimelQyzP0mBVt6JekfFmILPVCYin9Hfb1oYKsH5xFvQSUiRsqOAKlPzHdoNaG%2BcnxxoGLhFU3FW6FI2ZDzGDGfhtLUx82vssswtbtpkvJc2xKKq6GBt36xa%2FD3oIe0w7Q%2BibpXaOYza%2FBGROc9CZEJd2kUEWStl5tadcbhLLpB0o6len%2BdZMbCkU3O47HQ%2BbqL%2BeAehV%2B22HyftJxdzJ%2F390pUTRDFeW40BjOX3k95Zzb8adk8LPgLb%2FI8IIC5Nvg4no2z%2FIGHPAGxKX%2B%2BA3XmFrgSmJE6F7%2F%2FpcWqptVtnvCZlyGifhbn1a9HgF3H%2BMMuJyd3TJ2TirUrtIg5n8U4x%2BgjEvtzKKBz2SfnXB6a9MJOIfvP6s8obOqKaSfBQ0xp1CGEPN2asVaEjH%2BNgHOcj3kPBgs3vl%2BQ7KlTQP%2BNT%2BuXVFxIoFblAsDpzFwUuhTsLjVPYUhDJ3G9ONipYOC2ZNimEhoRiwyYvlv%2BxoBO0sQVgb77gGIuR50BIW3Tu%2BgOJ3b6UUeXdhvR7lGNcJ9qdWJxrj4ah5EodYXEzXE13Yf8wY1g9MXETw%2Fwtp6%2BV%2FGxudrdbgi6qwF%2FMIsKurKytYFruA4%2FMEDLlUl2%2FmJ3yvT6y7zfWhEwm7jFxwY6pgGBzwTz%2BE5WUPsXRbWXrlLByCNTM8cqpvP1mFKbpSR077Or%2BSA%2B2Pck1Hx4aprwgNCBYHRI%2FEd%2Fq638UaJRdpKanFf31cystd%2FdABFw6w5aiHJIms7bkMVnpPy%2Bnox6eJ1VUGkEt8sp%2BagtUurt5BuRjsr%2F3ZC3PnfzC23LduEDadB6WChKvwrAZDTqpivlsTNpDeAp5%2BZsPT5RF%2FgzNoZTRqL2KI4a&X-Amz-Signature=467f1599febc2e1d75aa60fb0b12c30ba03c314c643b40a9fb0cfd8b78261e0f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

