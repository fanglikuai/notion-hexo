---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WY552B3L%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T153712Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCv0Bahu2zKjPQax%2FxWWrRag1ME8DyZzKmTWtKQRRXRPAIgf37O%2Fhgdl70yHHoVBFZfEItZgcf72QE6Jc72Pa4FEt4q%2FwMIeBAAGgw2Mzc0MjMxODM4MDUiDL9VoJ60fMA3H0wZpyrcA%2BQFkO%2B%2F5PEHboGeno%2Fevi3X5Ay4yirBAmsNz97qO8IGbX4tEYG%2B79F6kS%2F9UgCnk5F4F%2BTLcZ1SboH9x0k3qBgjfWljLuRFOqHaEoNu8PsQEtwPsALFn983kCglc3qdsR%2Fq7ZhRY1NYlONJfS3m9sUv9KcSCjpqlSTZIQPES9PI61PSAWKQt7qapcMm%2Bo6mMd04jXxEmBuBqzZwdtNhhiPsAOiBG7qzJHmFY%2BMD0VEsI0o5QQnRa4fajlVybVo0d1gIg0YSpEHVe60wyewnHg0H99dgvFYfJWfl3VjLw0FE1Og7gViP8Jwl2Ih9XmV0Zv1prv8MC8a5xq9LJuHdI0YGhZ1vG2kOWiSiz09bXSYu4Oe2rSSEcobcX1jEe%2FqZ2TzylYt5D3OjQ22vObAnlHqbGKQW9toaEfPS%2FtjC9e9g8WFXKEdA0kBhjP7hFsKjBZFBFEvNZIuAoX0c%2Fdv9fujiu9mUSQRf8i6mUo2PJmrDUBWNR0zhyQ2hIBLoRQBtPHcNXdaXOhVOmfbPCGgSW6bKKje%2Fxlh9nswt8x1TWAErhHXLTAfT%2BrNHhuBsihL8PWQ79Sgk1qnf6z1wTPsDKJgfWX3WDzkcaQ9OLQaesO4CP9FcPJcQu0K%2FelIYMIbboMYGOqUB2SxFBouMrS4VLlsbe7jDWUgreCbOnmcMs22cCD5KCPm%2FFx2ct5tkd4nTcwebUHB8kaoKg%2BsEvHR8l%2BJJdT8zfattjvy%2F8CwUbQpBW9bSXl82%2BZG%2B0Ju8LfnAfbF%2Fi0LiZ%2FyH5xu7Igb6rH9IHT3LABqSRKOJHvvwZrW0iJeA6BlcOR9jzhOcU2v7PhDmrtdR3ZGUiSeT1XgpvQpyA80daP86to9q&X-Amz-Signature=ef8b441df19e2672f96b6c60f16d42b092e1e035ef0f264f59348a0be143e1e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

