---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VM4P4LWZ%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T090819Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFmj41ExhqSr1BgnyUIWRfqxDMG7fWB63XMpF%2BNLyV5GAiEAqnmc%2BpB5cJnbsMfMZPyNjRCT%2Bi90lTnJEwiGhOq7ij4q%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDKyL1rvodNrB9niL2CrcA2uXjnbrsKwqyPKdW07JLM9Wpui3jZ4zUGQYohnIfpOAZsOyA%2F5yc8WrlFzhw8x99oOBcYLCNkYY0b%2FJ0Ycxvxfo1pJAVK3XBAHNX5A9vkEinJf%2BzqPgQbUL5c2CzvPOwRL6g2dNh68H%2BESlUnbKxriqpNo6MwBEecN0Yi55wOcH9kNKCbpkOWkCvB%2FWm1ln21oOy%2BWSO%2FAqmexo6%2BNB0f8p019e4V9aZOV7U1wBLhY%2BizFSxwOd7h0jdCCgE9VVNjunRb26QAIvc9J%2Fh1pNmE5%2FiCi0q1RVceT6LNXjSc9DP1omf0nvQcgUhtjJGHlHauEkODqPeFFcRURfI1LSn5FRUP1XK1CFZmidyUYo9wT6EA0F7vYZTKLvArIdSqsCvFTD5EPQ5g6WKWfynzA3cQ6jJKswYG2AkMGSEK1vXmzXvlKROV%2BAqJNmTSRJXtGJfhrS5V78ywvJisEJq8f23tPa63%2FuHkvPQwYj14FBwoSVMk5IGNcB4V4C8haWYBSdDm6GgsB15%2F4dd9CPnx64b7IKpJCctDQgYGmyiw0nhjDvsu1JY6z73M3Rm7QtmmT%2Fapar4TX%2BzVASLSOlHfAgG7FliXHc8hZR%2FLLLs6wH%2FkodHhqyN%2FSbqtkqtCVkMJGTn8YGOqUBXUm20HEiXdSKLviL6K2Pv4us3zuPuTS8hsu0lJMTnwre4gH9u0f8eQFwrw3o3DxiioXgG6uuLZSw0eiQNUSXoaX45n9l0kRHHI%2BWFWkicYxuQSNBiPjQAAWu%2B65Ml6z8v3WMKiOjBTLwKvuO7l3upok9oBaXyRAqDu863Dy3nKtKd1VRT7OnkTCjsndd3c%2BhwwbjqQ5peOS2PpPdyjoV0E8LkZEp&X-Amz-Signature=1b5ec49bfe9f9ce0dd81e64e8ca0e0a32e7e7b4f3deb4ca04c8a8761f8b5c38e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

