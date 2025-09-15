---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSV6MYGC%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T085604Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDHEwDi%2BJbHCY8FhpsvcwCfrKkQWVGoDWOJOh9nPRse6AiEAsrNQkx4YBKlz1LDmlCJGtFr27WjpqB%2FfgOJ6vOd66Nwq%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDNlPp9SpLmXHHjoVSyrcAxdqTdVruFbsjRIED8CdHw%2FqTEDxJo7sutGDnEY2Yi3wwvqYelQlGcrOSHg1%2FNCzHgYOl%2FFF8TyazkMjoVaCm4xDXJIqgeicM3hxxw6LQh%2BQH%2F9Qx40Q7%2BVw%2Bs4qdOmrF%2BrKM3%2BI4HW09ROlW8kkve6oy4g4ud%2BDnG8tqxbeReBeICGo984n7hFKH4A3tJJZUFgF3kefh4UgZRMhK1ts%2BAfsHDQ5q6c%2FOPuSjhQf7ySNdb3J9jEL7BCJvGYmkwMvX0vedHZsUVqznk4snfMvhjOU24pAgo0PXZhLGNANgAFsD9VedFzjrU2e0thZFIYoZx4NSohoD%2BrQXVc17xmycd0t16yOCwjb9UQObp6pm15nd48cZq8sgJDsqfYMFXDam6K1bPNQBfvDo%2BWPaTMnsXN9Z4Cu4JoInYMQxM82eAo6SOo5G%2BAE4vu%2BmLFIasqUnf4pJDmvjaiPVvTIWbQfrbr1WQYB69YWtRgdWbmPwf79OClZW8AmP9050a7tpoM%2BF%2FwwM984Y130Sdi4etMuIm69g0QOFiPQvhXihPNnMST0gp0G%2Fl%2FMyGAKwxod8%2B5Yp0NCkzCkkvVzXgyk1%2FnEQ7jsYEtkpWohczyPXwZqn%2BzICmFxW4JsWpCh0rcvMLWTn8YGOqUBpdavTBxjNR%2FohKL6fgbsGGlHilllFXyA35BgP9ce7Jrip6SE69hEb5D1EfVkiJ0Lih8pATFR1X8Rd02XHN%2FKEs78usgh4nQQJIsLVQVECnmJTLqJDewtPAQ8193UFbQt06YIlxos0hPNGY01NkMyyByoQY3QiFGWoPjLKLA3oXGhncDQy6s962vKXnatXkiL7VN0nCPvDznkUF1xvETPAfZ43AV6&X-Amz-Signature=063925e5b1d7e3be34f7f8e119deb472a1eb03c1202115aff8edf8ab1f129115&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

