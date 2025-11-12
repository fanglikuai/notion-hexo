---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHSINTRM%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T210040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJGMEQCIGkWs1tADg7E%2BHJym6UzbvQd%2Bf1q%2B9OmEpem6NEYjNp9AiBkrtp430P99Q6NOVSOl7ATBn%2Fl03wgth8WMDNcBn1UZSr%2FAwg%2BEAAaDDYzNzQyMzE4MzgwNSIMApegjDmo6KuuH%2FcMKtwDc%2B4o7rfLe7Q1VJBzMc65X6gnxPYYCuKeG9u7MGleA09DaCPYTvZMNZMMckqz%2FMAi1O9Wb0qiOtsCUdf7%2BhVWhrN7AYZA1T%2BGJdlF6PUy7HxqCUkvIcGhiVa2pLsBrbifdo9BKL9rP074kiJqFuDDRjqX%2FHPBicysatSE6MAZ6i177gLkkFDxyDxKz79FmDE%2Fxp36EvehuC9RmkQc9gPftXas2YSZ35P0fKn5lCmurWA%2FaL5E%2F7TvKKDl9bzOq8FW15aUkq%2FgEyfULduwB68wjy9HmS%2BMKaNH0AnBWhyODP4SiT1QMYrawkZ%2BeWYKYctKklvVoQo4gTIUdprAVZMJvvj%2F2quroVpfE0JNfOo0D%2FKphhhoZjxwWh%2FyrJeFJL3L5XroJBgj6mVfZRYRliV3%2FV2MR3WfLAk0wlyGOwCJRsf1k%2FOt4fN1ruzgPxtVzSuUYJn2guhwMYxMxHnuTuSh%2FrrAPektnh7s1Q9klK7EHLAGlRQvRWHjvJ40GPLYBxBqDyNTOEL65ccItVuPDq8PBlSUCB3M2dEthd7Yyz6TQzYPZj2%2FZfkKW856%2BgBmEGpbQYyz1EqLv1lfzGFvLatkAUA2eqZ7Y0yzPFgEmUbIR8aLwI7D29mNgBCrd1owu%2BDTyAY6pgEl3BQ6dIm78TCr1yiLNq%2BLrCVps2LKjgmly80RBBzTeC%2FyCzMTe8A2GG9432%2F1rzrG9wv3M2UwOf8okNgiZJl1EEv9iwc1oPYOq2HBabmE9CPtrxnTwYXows2CaMlJBjaiwQQFREjSiaJlgD5u3V1T2mlECI1UU4OOweZQSgCCw5rs3vBL36sMyFeECUxR1dIrpOse8%2FbOQztcHUhzR3G5vqAVu1b9&X-Amz-Signature=4edc0d761944986f25f99bbe35ac7472f0708401a70a68c2540b96353d54e959&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

