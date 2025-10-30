---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RKOEURD3%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T140112Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQCXxvqmmLlOdzlbQOQ3iS40qNG25pSEjT7X5FeEvki%2FnQIhAIx293MsNEHbAUMN8%2BbqhPQv07G4Be7u4FGxeuim0ewJKogECO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwnaEc9OKwEEEPb9VEq3ANq%2BJObhNLa7g2TbgyeZmRKHedQl%2BkUfDQODy%2BYuYd%2BqYbCCyJ78SMUKyOy7xhb1y5J29CJIGbXcvWNpYKSRz8L%2Bf4yxG%2BUzuK1J4Lar%2F2GVE0ffODeDCXXLi%2BaKy5xxugj421tbYhpvqjbU22gq0ZVcIiSYTwKGbV10AlJc6q1ggx82qpt6Sj6%2FwzOQKmB3nNubctFaLd%2B25pp6b6Ad8R%2FjdyMZZFvv9wanaxU9D%2Bz9r5%2F1QIGYm2aKYLzoPk7ECi9t%2FMdckNnfJRUmPMHlptBeBRudsutrofRnf%2BxG3ProGdvSwkQI4wMT6yqhCYpnkMNGTcXsYPeibgwvt6%2BDGZd%2FQeI6TyN3OVJhVyuJZWnzHFkb%2BDJ4m836u5DqoiJGeEdtYUGZ0pKHL%2Ff6CrgcT8d2nlXh3JINU84Do7EqPdCi3T2RkG4X8Cp%2F9gLFUWLanab86OQXhqtxb2H6dOkElrbSxmbvQtxbHc1E%2FiyID9cE7foft1gf7CxHqGD2awsi%2Buo%2BvorzcbCD18CxJW4fWo7Wqsld8U3QborwVQ2yNe0zVKYRRQ1tUlBDIKteb5Gpd0loafnhcI921NhvJbZnBeva0nXt4jR3MrQHsjkNen4LjESp5AwtL9tfZekuDCs2Y3IBjqkAY3NWIgIHJbCtjkfreOHM52NRoRMxteYTxPRrseEF1YJXunCzkrFdh5zjFcuWSzyS6%2BIeKM2Uisn2SjcCK2S07bGntrqvdDewGJbOSDF1KafKpdifdldxSeCIs%2FQ%2F%2BRSwyErkGVeifayD3f11MKb4wWsa6LG46IGN0xlMESvTqkC9Qj%2FJhgMo6VDq0tcdl24VFiESfoB1sxRMgfA6s9XXi55STdN&X-Amz-Signature=ebeb89153015f1d6c0ab3a91f2505b68785d80e3a76dd2b56394486652a4e488&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

