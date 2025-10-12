---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VF32OY3B%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJIMEYCIQD66ChR81H1HCpFATNveJQUXIwuvOM0Q0imYfgCmAMk9wIhANXbFKM8tHTA4wTTjWWM%2BNGmh50zAfvEMV64nWgrMxMfKv8DCCUQABoMNjM3NDIzMTgzODA1IgwNe32jIFBL0wLmZVkq3AO639MWJfPfVbqw7WBB%2BxRozJuXcq6h%2F0wynlITeU78HhAUG4H02haTCPRNrNWU7Rl%2FksMsIKNjjAuD2bsVW5coy6zhW38ID7ljhRGLkQl%2FzJQy4sRInU0Yz7gvMDHRD09U94MHtBIfwcFexwzeo59bgElnK%2B6G2wOzDP5%2BRqQbqJWveULKuk51ptHOjoFHv%2FR5LLkhXoUcKgE%2BDNQjJYbSc44ikXdeH0z%2BSkBccOokffSxvMzXnhugU8YvpVDWFEksG4TJxCp3khdMjxNSmUQZi0yLk1Fg4V7meANfVIIo%2BbLOqUMzp9Y20dq3ETgsZDXnjvdWeQZTS4a7fDI0lvrB1%2BuHmFSbhaKUr65%2BwFIbzxpJV%2FESKCUMh7CDKzI65Fv%2FVjzyX14MHYtSS2zKnsx5pRX5kBJHBZcYueRM6ecSEwlOAbbMD%2B5i5L0DunKzLV9o0It2avBNPPs3qxfi%2BH7gxFKkzXsGOZ6VRRzQ7CsNPFdajDgLhuApCsa1tHPqEQKVgCy3eECxLWPU3bDgSiAmVareaMutXyXS2yPIJLk7R31olahkdPFS0Fk0JBbNjkG%2FVcVcyL5eCi2vzrQBUlt79q%2Fvw67g4B93DUJmQK6gLtTpEZAlPsllQ21JZTDOy6zHBjqkAcgJH2AP4ZGx2l5X2OQZBsw2vy46x3DA6b5fsWP1%2FnvAx4Od75b2NK2jdxszK7HRY65vQsJg5%2ByBneJv3EwDCJ2yi4HRIc9erduQuXSwUguFY8MIDcUTMEpf1yhSt6rokIWeAIMJeXr5ax2XYwyXO70O99oR44ylZP5QgQx8HlG90NqeMPao4I%2Bo7kgg3LkK1vLlFDJQbK9KqGkQHOjIMlIoxlYF&X-Amz-Signature=21e3fcf4add6c1134b8dddb3e6f29ff5b2e33a9ac3b42f2e4b4eb856a7b97dea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

