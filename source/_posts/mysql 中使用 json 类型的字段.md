---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YU7IWCAG%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T080723Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDZh9HLEX2ksSHnFshVtidMn4phb%2FVcVDIDDiWA3WAwcQIhAJYkYmx8KAz%2BBJA0IR53u5NC8XQKdrDaoqL%2BKl%2FhFMzEKv8DCG8QABoMNjM3NDIzMTgzODA1IgwDTwb8PILZ6oPK5foq3AMIBRoqMqWfbq4mfeGeB2iNsZAQ5jTuZAkAD5GZZ%2FJN%2FTBFWgIhgrbbRXIOIb%2BQRzQsabc81wzVTFx%2BOqeGOTwyDcGmQEG9b%2F6FpB3al6K7Yj13fZbRMa311YeAYmcLvWUx3zr4D1HTDCoy%2FUslzrmu5YN7WKZaDySu6q%2BZsyYwneM4SAUk31dKx7TU%2Bx9B3%2FaOQGFq1x73Nat49HQahHdlXlB8iBp0Wzecv%2BXlCCEn2Ra1bFepXTPozBC1S9X0038IG8ZZlzzjfIblvKbebdxlVHVpI3ALDcDo6LaGwe%2Bd4pcRlsbIXOvhBvJl%2B9xQK04ioHeDQx3QUegCgjETBT03m%2Bvs%2FPdMXEPLKfvoXEp4Ez2ZY27bZLY5JCCh%2BP1BYy1YZQJ0CS8XDAXFwTlXDRdhrYx8pUxK8eAQwhUf%2FUj8%2BUNtAS%2By6LHZJX%2F3EFZ1tmMcTXt5T25kmm0v96pVQ5mAew9AwPhUQdOGjQrcOczfH6X%2BTnYn5VE7SH3cQQnTLwTeZvHvOdmJLs8hFybfMBEQpBxB6%2B2x8uToRwELgZ2wvc2vKVn1Q%2F7ymRJOhE2C85aYqz%2B48BiW4dau0ORPMgX%2Fyy5aI0SZ%2FESIS5V6FRVGxPdSJsftkIsSuGFtYTDc357GBjqkAZ3yvqVQyIeguvtSKlWEPiop%2F0p1205qMD4frxbdpiDZNyu2X2Pbar8n1BU5rj7Ag4q8ExrGfiW0IlZVfgqtEDfiCv5osqfYHHzJ2Xrc8XuM1sRKDgrFf1sYbFh6MReTvM3OHt5w9zS6jvWvBxmhXPGCKWiq1QckpLtwN8pcOnVevGM%2BtvgFXz5Mu31yw3p%2FhvGBE7YMQ2FK0%2B4xvMeX6hOW1veK&X-Amz-Signature=b0013f2db8fa8e85ea87a3c2ebd3770daf9dbf7cf561bd2f99f6fbced8674b9a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

