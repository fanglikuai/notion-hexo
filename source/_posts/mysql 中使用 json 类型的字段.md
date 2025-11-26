---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627SMQV3W%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFurg6QodyscnITOOGB32BHcEd%2FucmiA2V3cG9BeVZ1nAiEAnEYbrcIPJ0fSdoV6nnWc9Ji8AL07w4%2FtObafCe2oanIqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDG%2BfEMvC39vxZcybcSrcA5Rr8%2BqoTIdWkqFz%2F%2F4FfpB06N9QVXOj5dGEFFzTdY2vDz%2Bo4UvRxxy8UANc1lpiNh2Y1veB%2FHdIHuqOvXQLX0EeCOmASNQYA3o6fGK6JuYF5LEhyP0GGtFlKTp90mX7KspnjPr4XeS7ciWZ8SaY%2Fa%2BAf82KzgkbZJJUpOZIpEFu8auk7mav7XRJh%2B1WeryV9qtt%2FGs%2F4zrj%2ButeMNHPk6SdBK6dyaBmQicu9r9f1BqcX%2FrPKoi8Ck%2FLZ%2FkdMoN0WfUdd3%2BdrSonLRFX1Fr%2Fegdp2Ga4xQ%2FZ%2BXC3keGP%2BPV5Jb0Jbkh7yaChDzOUaYWMKmQ%2Fwu5coDtFqogzrf6SPK3%2FGJWuU94dGa03AmtvecZClmXABmsKBpmzsCwyZGAGLw6914kcfk4sIht%2BNesIj2Yzf4yesFZ6Tfzv4s2sYjzO3%2FUXah1COye21jYYg5KWKkJs6qKgviasfaPGyvfZ8iO0AdOBv%2FEjSQq1qDfdUfyZXwI7aLTCZVC9CRz9QB25Sik8A3S0sNR2UbZustrrpGFLmbJi%2BpK2iXsR%2BWzRiDFsagOy9Nu1S1JETtsEJcF8YB9Qwn8com2fk3lN1FuXwLi2BJOeSClcH1KnAEhzpnoV4b1l%2BPAwA5ziSvx3MMPGnMkGOqUBa4zUeV55%2FEWRKrEBGPNWzaIQ26bZmiDTwBXAl5eFKfVKId3O5JjDJLnzAugSkGXpzhD%2FN1mb1K58vE9Qu%2Fu7u4657pnVipgG48IbNyn1DWb42Z0WC6zhVB9Kk7O0iH1R5BD8Ip3AHWoXDv3JC3LIjQxEzv1TwX7Se3GOibZp3Kx0Sr4KZk9I3bEpNxcmyxCWQfyBtnqkuJwiaEP80P7zn8SjFXK2&X-Amz-Signature=86899643207582ef103bd2b86e246a4a932a05147dff5197d28126db7ec8e8ec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

