---
categories: 业务开发
tags: []
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ITFFWAR%2F20250914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250914T132915Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHkpcQSAPk6LhitzBPutn%2F0nnIF58U0qjORCcFUHi4DvAiAf0XB9CdvQh5f3srDRG40hmcoHGRCdIV%2BetXUTZ7UyOSr%2FAwheEAAaDDYzNzQyMzE4MzgwNSIM6N%2FS437X1sSB8ZhhKtwDw%2BO41Ta4S%2FtccIpLgWv%2BafiL1Fs38WFh99ICyS9vLClLvGo3%2FuxKHUrhtwKHHnS3fmQU%2F%2B3jT%2Fsa9UQf%2BpNbck1HqQ862OUu8i0qignWYndwO8cejkqUrI1b8bDoKd2YgSl6huxCOt4qWvPSsIbYcHq9CgBxBUX4OVLtZSKwqcYkDrmcsYxUivEHEhjJrDkfgZgYOhvAe6swuEksc15oRLb56vlimZTQ%2F%2BsPVsXIcxV1PMlRfOigxfTfir0kvzpfRaHWPKbRx9kCTpTr8WpsHv9WxfpmWcDphnXICimxVS7CQMdJ3zLyhT3a67pm9abfiwCZlCs6TFC%2Fdwv%2FK8O5NIwaJ4M79lhy%2BPwEIEKX58O8Ppl8MRZBcACWrW3ZTmfU%2B8oqiXLfIn4nQLLNrtwUoDLzAVWrEfZ3RhYxOcAQUQcH00RHUuTwWabvvUvTiKLPHeuk4hSkUCl2XTu5hn7en9okqLLTHtrmMmbIg2WRPIgc5RYVEAulAvu1IhrQpw4UlWemhVcBIFjPad7Bi2Cdl%2B15%2FbPsPcgPCWup%2BnqJ%2BZJXFFSWfAaMKo%2F2dBN8eIgZZ26h%2BThpuXlYbgvW5Tvbd55M4DaUL%2FbKJkUBcLPrkV8Vn9NrfLPANzoSxcUw7vCaxgY6pgFL6S2MbXuEVybam7Z78bye%2FlQebglewD9dpNj3KQzt5F3FvDFVSl%2BNxWrxKNZE0pCvGsbs00pLSkSGJFe6Dej4uTNw%2BTw79kyXjXbqMiyqZSZ65H6%2Bm%2B2xdSUra7Y0%2FdEZZZdK4SSb%2FbCcXy2sSBQgHtlsLoHxsEy79X6L92zXmchyHx%2BP6GXJQX1zQKN3YUZbCb6OyfiQ%2BZfOcB%2B1QFHNtgP5VOZU&X-Amz-Signature=58b215e83eab03386d6ea75093346feb07c7b7e33d5113760d2f1faedd8a1226&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 21:24:00'
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

