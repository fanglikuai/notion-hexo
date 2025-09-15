---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666LBA3QYN%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T183042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJGMEQCIHOeh7XpkTxnoOGyKdf%2FfXgkFYK%2FdNrXl3oI31eYrXsfAiA0QrgXJRqpTBQE5J4HROSNj9UOGyieUloRoVI09KNyLCr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMrkFAoH40cv%2Bpy9VTKtwDX%2F0ffF%2FA79zZ75pSe7hvCOH6iGXD4Ra89RjrjXC%2FEQmzcsEjAVRMNuEa4F9EUkCNnMSVJFAx9S42Yk1FMKmjEfKnaab0139Ak%2FQR%2F5BdIW822fUBTYfoDTxqs77RemT7MElJpKw%2BStRrBRDozjEJ97guZxk5hO%2F3uohiNjPVbgqhwkCIJt7mYGIizOcyFPQSAZPX%2FP8eZX40NZYr0C09pRQ8V90SKUjvWJwtbWgtwt9GP2GZbwwGGPf0lf6jaDxbb1jO8xAP3On%2BQjWGPL3gLP7mdfWgaRY0ljDyAFQ%2FRTEEd2yXqO7E7TTdaN5cFSfIUpIY228JfBKdnjRkw0MeTPyZ3jiw7A6XOLoCPFoaDHmiuLrLNyYaoH4KfPtoDd0L7U9KDly%2F8abjpB%2BaJRSXhnYEz%2By%2FklCJi3cbCsxJ8fIV5aSGWi5P8kQv0MZ7hITYWGd1%2B3qMR%2BOcX%2Fb6XARBGGpcC4b%2BFT7h%2Ftf6ggXPJ%2F9OfMLYksQkwbWbdo8BCnvKJCBQJ1cpr6OabyUx2yElNFDvhPdj%2FI1OjKZUP4cRepZkWansO5ORK5gfB83CmK9Qtbzz5MJWDmbQXnOEs4usTTtVRtE5M76Cp1%2B523kpOE7Qw4q18yMHsoA7SEIwr4qhxgY6pgFh1oROvIznyl5360xBdwiC00mzwqHMwsrFZ4s2WMrgkl5XItkyuVqxE82WVzcE1Ob21PKpZyZ%2FrADQzUtxpjyYMQWPiMjtDV%2FpVm%2FA%2BJJfev9o7uwzcO6yxU23sc%2F7lkS91esMZwuupqrtJ8dCA9Ww6AM4BP0Z9mkss4ffjCZjF4wrERrhd7H3ozzPGRnE7Os5uqyFZMBzjKQVs8tcOCfqmqxuT9U%2B&X-Amz-Signature=f522d68f5217d8770074ceb67f6b1e670402785c7b8ca5df8c5544ee0529fd88&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
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
package org.example.studyboot.demos.web;import com.alibaba.fastjson2.JSONObject;import org.apache.ibatis.type.BaseTypeHandler;import org.apache.ibatis.type.JdbcType;import org.apache.ibatis.type.MappedJdbcTypes;import org.apache.ibatis.type.MappedTypes;import java.sql.CallableStatement;import java.sql.PreparedStatement;import java.sql.ResultSet;import java.sql.SQLException;@MappedTypes(JSONObject.class)@MappedJdbcTypes(JdbcType.VARCHAR)public class JsonHandler extends BaseTypeHandler<JSONObject> {    /**     * 设置非空参数     * @param ps     * @param i     * @param parameter     * @param jdbcType     * @throws SQLException     */    @Override    public void setNonNullParameter(PreparedStatement ps, int i, JSONObject parameter, JdbcType jdbcType) throws SQLException {        ps.setString(i,String.valueOf(parameter.toJSONString()));    }    /**     * 根据列名，获取可以为空的结果     * @param rs     * @param columnName     * @return     * @throws SQLException     */    @Override    public JSONObject getNullableResult(ResultSet rs, String columnName) throws SQLException {        String sqlJson = rs.getString(columnName);        if (null != sqlJson) {            return JSONObject.parseObject(sqlJson);        }        return null;    }    /**     * 根据列索引，获取可以为内控的接口     * @param rs     * @param columnIndex     * @return     * @throws SQLException     */    @Override    public JSONObject getNullableResult(ResultSet rs, int columnIndex) throws SQLException {        String sqlJson = rs.getString(columnIndex);        if (null != sqlJson) {            return JSONObject.parseObject(sqlJson);        }        return null;    }    /**     *     * @param cs     * @param columnIndex     * @return     * @throws SQLException     */    @Override    public JSONObject getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {        String sqlJson = cs.getNString(columnIndex);        if (null != sqlJson) {            return JSONObject.parseObject(sqlJson);        }        return null;    }}
```


在yaml 中配置：


![images944ad29a7fcf96a0c51a577d6bc43317.png](/images/4d25cc1863ee3e3fa6ae7e6d4c2a6cf7.png)


xml中配置：


![imagesd6de49b9a7b17849e0d393569b93bca5.png](/images/1067c14ea63fdd81764edc7b0b6e9828.png)


# 对比MongoDb


假设有以下数据


```java
{  "name": "John",  "age": 25,  "address": {    "street": "123 Main St",    "city": "New York"  }}
```


使用嵌套查询即可


```java
db.persons.find({"address.city": "New York"})
```


可以看到，直接被秒杀了

