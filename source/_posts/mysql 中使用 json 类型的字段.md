---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666D2JGT7N%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIBvsszpZwN%2FUnOO7GNh6E0kUX4z0lVRcjno6pPntUHeTAiEAxerPwODFDXM%2FwJBH2lxd1rpLlSzNaSEiy7Lz0QTj70IqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGj8hE40QBlA9lEvZSrcA9iBw93jqfjRByiSopqDUX6zrr51ShscgnWRZQB94TSy4NRUcCdkATj1AQoxZAU4SKbVAw3jREYzGc4t7ObGrvQ6gtA84Do3qXwIRqRmOAx44T82mbvkBpy9%2BrCc274L2VriZ%2FKhjGCcqvPW5BrHgaQm%2FsEIThRSoQCsH4CX8hdzV41t%2FW%2BtAl7t70qMS0gMjD1X7FsiPgPlEtefePrUl5f%2FydlAabP7xiQKTvVIafA3ym%2BL1CTnFgzrKEV%2BXk%2BdDgTgsjbIHWIRcs3lCqrSekerVYYjCWhrHP8fHAoG03GRN9c4ovhn%2FBDLkxGqaEbVJtkEW074Fi%2BQgR6NAV6DjUP3LAglJV2WPgGRiGWlAz53XgGo8NOFwCkvPGxsuw%2BukS1IX6gVzcAHHdsOV3ez7dnvbIaVPJJi2GnXsHCqZaIm9NOZVAv6vf0quCGl7miZPcA6rbyrA6OYTUXtT3uu0Anrl9cllM%2FACxQAg32KxGQD%2F4Cd6j7t%2FkjOQzhGhG6KdrR6fsOB6ewE5huNrRis7kHVvznQprT7W3g1c1H1xpqsscNFzAiajxBFQHsP3jF7rYW57T0ZvgSfvTf3hKSYM%2BDaBYSxrQCOLtLiWF7k4WK2zfMqe%2FIAhkmw0TUTMIn2sMYGOqUBbUd%2Bg3OO9X6%2Fa6jEFpqMFeue6XQfOqGjmCzEFghocLWni1gvYnDAeg4%2FOPS1Hqw0A%2FcXGUAlSnIa37NNpbn56xTfqgsAECyg0Tv7USFni8L1%2FDpjzN%2FIMsJYqGc5%2BWt%2BRLJYTfubW9C1HmKv4YvYCsrV9AL1bvUzTlExYeulssRf4r9gkHmIYhPULwKMVzwL%2FOFuZERQZptgbI93jYhed3mljg%2Fk&X-Amz-Signature=c49ac4b026fb63d5b30561e026f3389d93f6c70a11f512d7324a674c22b98338&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

