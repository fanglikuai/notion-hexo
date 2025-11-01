---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVUGGHHR%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJIMEYCIQDVFfJxfuU5UpD8OuGoDlT7rhlgXRpdY0j%2FAaxkFRq%2B%2FAIhAJtAj3p87NTKXbjA4HV3CAH1g72WVICq00h4TY%2F1AlytKv8DCDUQABoMNjM3NDIzMTgzODA1IgyNWCP4oh%2BrNgozHhcq3APt0I0NePVaJn1Nnij2X%2FttqbhrmQK9ltepd0kPc8u5Gh8LgwAKCPoq31%2BqjVGLgiOasIvCUCeEMcmdQ077P453D2DtczR9S6KjhpnBO0dbJKxKi9XGvyBbIPgGXkmQlLirhvW8di6lCZTDo4QGGrVOxZOAu5YQ0gVRrGG3IrlEEhYvOUY5TZSKbFEhoXGjYHuY7kTM3r%2FquGc0Wh7%2BxPUC3fytCcFjV1%2F0%2BosLBADaTCoiqyXiLjkPin75qGu9HNmlS8TKZvi0dMIP3AGSZBjBWgf93cxyC25lgyaOFWfbupXBDoPW0NF18iP2h8%2FvAUmxLA35%2Bb0LP4r2nq%2F%2BNEZofkSvsvVRd0UY%2FjmoDwCAP9zXu5r8GJIplZ%2F6%2B2mZOYOWyRj5KMjUeeRbZ1CF8pKkGLMy3j6bgrkm5HJVnaRTCvIfT1Bs%2BiMz%2F%2BL6JBhORnyCTTickAifNXSLLrviAfQ4eUMZbh6ScPgjYhMp3t3aJJ871G5Ajae%2FPJnhOSDeDfdmROyaYznYFN1KY5Dbx9NZ0bnHHlQ12oOr9jS5ngjFkbeCBUDBhHiM3Uku%2BVAPH3KHrzWo6dc9NsekLrSpjYUktTpx9W8fE9BVymLXrWbwMdSCACECQ6WiBfFE7jCYw5nIBjqkAQYHweVxycOJ1%2B%2BLLPAkB3AeMMR%2B6jOigy%2BkV9R1Nzu8SpcIWY9r3Xx2CLLrwxqd8PnPI7G0ze%2BVd8rX1cL3s5UZJgcHutzF4YvFBxUIzkeAg%2BxqH8mt0RoWQiYjrOqTCK3Pz94n4HFLig55TwuIGFfOAL2S68%2BGLYklBgtqPDBXbCQGvqj2VjO%2Fr8RukFKteb08yMNOnNcN7e%2BWmKJLlEuips9B&X-Amz-Signature=dc9343bab06a36121a48e9333942bf09fcdd7bb44561ad7f6d6e8397ff02bdcf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

