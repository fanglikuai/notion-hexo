---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZJFYMQF%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T230047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQCdKFs%2F2bqvGQl9j%2BBDD8q3wtsEXeb2SPMBR45xvkLgeQIhAIdhV95jnUIO73zZ1urCOn4ICpwWCEkb8f1NoUoz3%2FcCKogECJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwK8LdUBQQi1ncYUVkq3ANWcTUe5sUIwk322n9oM1HGzaL%2FuXl1O0UJsFMM%2Fs10pWGg9D50%2F1OrY6EiFugKVVACa5pxZxYnLoHq0ye5VIwcfDkq%2FY1J9H67Ptw5N%2BNWVLdEnq4ZnO7o0I9YuqURPKNYIrNQ5k1VmW0Ju9Mw4nO7oMZ7%2FZzCnxu4MetmudOkPmyOZnWujINOj6jzkl5XpsDrc2KtYoyq%2BGKc%2B8hLFABK1gZAVl%2Bi0PzOTZBAKatlopcS%2B72g%2BpMFQ6NuBktp3gcovXXz23KMijjfZj8hF%2Ffh7Z2FwwGC3F3OdMJghyicoS2DASKr7ozy%2BVCAIXd%2Fz3v3WtIpukw3U7hGMzT2%2FBH%2BKf54qNoejrMpxDB75Wyn0xJDD%2BSRK7isw62qt3lu0PBMJBYJlyRg%2B9wbIs6X%2BCFZtA%2FtyfTnHcoElebFedezTVeBjZu0HLIjbSkbViKoPqyyGi4kf%2F0VnJ8iBsMlNLs056DShGTZ876BFtMfwSsRpXc1TipGLSa9VXmPNnIwFCIgmtIaNCxpwc5h1P0qh1Lanjwd2n1ihuj8hZsOd9%2FSTDRyaz9inTgC3oeYIz4BsjjnKtL0LPnB0PwI2agtXl7Mg9qjZ22elg%2FmVuldG0GbvrQcOWi4mDHlap8KIzDWstzGBjqkAW6qKCw1d6dHm7udEYB4LoRuDKhX4rveauWNhGviqUF0hGS6wr%2FC6kS52LPSs2s9d7rdQ9Y0yApGEtmEDCZQgy9QQ1yAo%2BdfAuT4tkF9jIRQDEPqD6lhdUY1PbZdNqWC4Iped%2BEvuK4wvfDQw2mGBka64JqMORIizknmINwslnbw%2FGAq7F5HYfVxLAMNBVtjfdkD0%2FVsGObEdUbtqTKO73M8uz3c&X-Amz-Signature=403a047817c06639dab66f34339bf3baf4dbee66f3f2381e96427c67cf5e0165&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

