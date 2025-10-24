---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7SDWVPG%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDBDWhTGP6Ta9BzPHM6os%2B68hIlicrUzdejbspVEklYFQIhAPFDPORkuv4X4tmb8Os3uaJREtPAzG270JJlyx18bt2vKv8DCGQQABoMNjM3NDIzMTgzODA1Igz3mPa2%2F63lflnBFgMq3ANcK7peP0YphybJZg66uV3fqI6FpuKoaZ9ZQo6dxuyS9oYa4mSe%2FgZz04bwU%2BOECztQUbjCf6OWroyBZBxO30EUYSQFoyM22mucD4sVxmzH7SRzsngI3kZZ0PbTOnfMbTYctuJa6vazP6ccnDvUVeElj%2F74XLTjiZ1QIiuWo7pXj1mEaOokY8b2yss0iSpwAST7QO%2B37Wp%2F%2BN6%2FEYUxPbbgjK8BENnEU3utr2wjGFDpE5kec50VtFPvC82PbAYh8b3XAm9Na2oV%2F0bw9tGq49uxI%2F8yrk15f%2B%2B7bgmlExwINIbUgnzgxJU7zYlTGAiObhLtFBQlfI8n2RDGAAIAmKbDM1Qfhz03kAqMSLouY0aUwBLeXQd8ZqmmF5cRp72zQFPVpUqMwraEfQqbhr97sexqZocVFS%2Fu3d6YLkY%2Fkq0zZLAiVeG6bKHeeekuxTylu4L8ovSIp%2BWosbFemi36g4hHEPUJUp8kT7crsvvPpha1NuDwErRh7IXjTwyfpH9SPU1KWLVRHEtsYs5XCF9jITZBpJHHBwkm4wjswGqbhDKuIUihpUEgnlAa7LQQApzhuaCsWdymlebOizIm7lqkyVNp3o%2BpEYEW5rGI%2Bw%2B5kKYPpCjaLgDildmuC4veVzCEke%2FHBjqkAUOLhLxeRn7t2GSbMvPd7%2FlZYyTTzsLdu73CUoLSDAUvnlvuIhXXp9nR4vFijow3YUJ%2F8iVHONopln44kqF4J3a1FYkfmZBgRR375%2FK2hOaYxy3uYbqcZsAMlrf%2FsrSppRO4ewY2KNhHep6OmqYhduNkU12tOeNto95bRL0ANM7Ppq%2FL0s8fTX4yfudA0KpdAdnyqOwfCpHw3GpdmSX1B6mRpT%2Bi&X-Amz-Signature=d2810a2030c09fa240560359eca99cc8cf0ec2c75610c8fa1d157b9153b1ca4a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

