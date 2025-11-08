---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVGPEFQB%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T010128Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDWSnm50rBvKvkemt0uoWuGflr0F%2BOzxitSyTFP4J%2F6ZQIhAOtJGrHxcHXeDcTdlT%2BaiQBUJekYa0QMy8rv1IEfF5AkKogECMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwtDgnWsFEaOEPgXK4q3APWhEtIF4r%2B97w7kwjegdneIRr62MULLwGfwIta4wxf8iLyToeNTqqOKqbxZJdJJEwfZUyVXcqdg3rtmodIILBt7JezOf3Ak2hmL2p852p76oTVlIJsQO58%2BrbO0ROJfiO6bBK9NoEo%2BeDJGYKv5CHNPNpEVN7gmgQ90pQntYSccJ7YQfjExwq1jmJkwjJaxaS4QXnnATXJyg2HTi66dXJf6xOtm5%2BxHVU5nNpkkX00RmJHus4KTzfPoxS6l15wt3Cvp%2BhNrm2Xhs6BzjRT%2Fkk%2FxCDH669qfTAqN91jqEdB%2FcxgFHtO31UMiOiKZQlmka1oVlp24HUzPDHIE4FghvRnN8I9myyHwqNwSrm%2BwIdy5%2FYhUsZZzMl5%2BiRbuoBtPPHPH%2F9ML%2F1B6Q16JdCq4mz2XdiqECDjXXMVxaucIR32XS6KriDsSfHg2ZO6%2BtmyKrG4UZfHdpCLNhH53jCWb53Db4SxN%2F6whFo1BbRlAGE3Y44wewIPMTWcflE8cOCpyk4Btru0V8aujOn1nhZ%2B0E0l%2Fs2A3cXCfTmDwGjVgXE6Pv29wRDZZ1GO9lC%2F%2B6fGCVeJQvLVnawZAanc9ZR4lYgaU43d0TUYmeqbPrdXJd1uhFpuuq3Q3hg2ldFWTzCcm7rIBjqkAR%2FSpXWt974c6xS%2F2eTVy9LWmlkPwO5Z%2Fx6%2BBnKXPJjgMLffl3TW1epdjvzjlW2JSCly23aLXst3aRP1qEvq%2BHIdOCx%2BDYOAAPIP5DLWU5RrhM3RtNMgnebnu35FKy4cQbM%2BFAGS86RE3%2FzeLcO1qmJ00oFrLLDTCohKDIPToTUtnWYdVbpGj9NHJLnjgi5fqT6VIruDKPA2jWUJa1tEXtSNySv7&X-Amz-Signature=ab2a18d7cf76e9203980871d4bc36df4d6ffbc4dd1afbc6fc29a6a691b324027&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

