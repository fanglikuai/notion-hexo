---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBZXKWVM%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIQDon5dhACEcJyvCIIZUNJGMZohVqOC%2FUlQ0YTHC84Wf1QIgZc%2FxT0Uxf7ZLM1w9EtN3q7CF%2FShSy7oXA8ABxOo08agq%2FwMIAhAAGgw2Mzc0MjMxODM4MDUiDHqaHsID5EEl8%2B4hxircA3ZJHQ5QVox7maAIKNG%2BxIZqK18NbCwVVxdUvzwpvK6q%2FCLHFbBn6z2llAelddFoN6rLk47YEQGitU1GfMHyxIrnHrHnNAnVqo2SfbFm%2F1O614pgT5g%2FoqwGjSkt2U2drAUWOSAB%2BxeFAQhKCZ%2FU%2F8eUvQyxzM%2Bk5nKqaYiixOZ0ystuqBBivkNZqB698pJM436U0mrIn9OGxhyLQCSvcK4GCT9VZJHEAGbNAKC5SrEEDJfjxYd%2BntTCrasPbCQh7O3oa1B9jPcaxVTsVDB0pmutoTkm2kGJ45mSJkOfNdrhBBN8ZshA8A40vFngpm8woViJpZORUfS2InZ%2FVA6T8ITu1umuhmOKJNukUKL4y1yXmGrlz%2FBFWr9c5iNqgkT5s0DPaytxsH%2ByvTPkzIT%2BApE2vTDebntFL%2Bbr8uUgt7M1qrrz284IqMFTNEelHJG2dFfSbrGKBDKE6UbHq%2F%2BX9XHWSdZthrK3DGYwe8vIeq7YT7nXURU%2BWBRe%2FL5NF9bdbmS%2BR%2FDauFFFtY5WEEDJC86QMO0WDDR8xUbAeiiFGr2NBW2j3PEf5red%2FJb3N1NpmoRNkpYKheaLkgOtXmlKdhqaQe0i5%2FaGETyz0Ah5bYg%2Fft9Zykmnj8SRd1xbMMbVxsgGOqUBxskOxoO8xK7KK8139zhWRrQTsswxmJDfYJjFCwzc0oz60Il1YPI8Dv6ZCa8H9l8xmyyrvN%2FNCT2nprQ3m32wbj7WXP5XeaNh2DSn%2Bjn6ISjbjHlYSETNXSoK0awIGnabgC4Nt3ADzUK%2FV3J7zYFhX17RlZ4oS3Yrt71fwD9kdvvk57NXlTQ098%2FucL%2BRYl5vFnSbK2x%2BREMiZXQFnFWpLj%2BoyB5y&X-Amz-Signature=2a14958b6c1de63a6d66433c99263f67c8a0e15afab57ac83d6db4d471d6cf73&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

