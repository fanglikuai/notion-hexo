---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQXINEFQ%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T090047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIQC1ExpW93gOgl28pcKLXqvZVv3A8zoahByuTZE%2BigMtIwIgdcpkuY0jj57FIZgPRhaPBak5lnncfQ4HOk3sKHZWny8qiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFHNcuB9TGEQTt2%2BNCrcAzGwA23CPLcOKnt9UtDdMDQjo4z6zLdnil5yrH3%2B2u1ndnypjsoTmJXRvlHT7eJ4T24S0Mqn9mjmtHTOlgis%2FALKN%2Ft5dV17GUDUKN07zQxJco%2BHHqSj5aGoJueCmgzBvjTCaD4w36KJJD%2FWl%2B%2Fyj%2F6cxGbvU1BbIBzcxZKgKwcxtgE%2BIg%2BM2I1q2D6KRpG2xWYC5gdTzsC9IdcY7yR2VSktsrHM44MfD2pV8SjCc7lHtMwT9zjMcD9iuSjplapAk0JoNXfx9paV08L5fEODN%2FEj0sKf796UALPdqtBAgNp%2FIZZb91zBjmLQXrCcu9Xb8qRq%2Fp0x8UT0Wox3AcTCjqqDkTYIpITIMMNiS6M2PkrLY2tvIYMlDWtLhuVUKIaw4Tn%2B5SwXztxgqdGlZcIOb79qW4vF8qVIMBWxg25HG3V3mjh72fmSi9tpSao508gZ3UNYOrgcSlUxiK1E67Nw9J8qgPPhjri5sFUpEof7%2F6MRbZ%2FgrsYOobZnmMmsCZe3JGf2%2BIS%2BWv0cgm7PLBk8V0PkxQB0xj9qrx3QxBmtLHSKQf24CUlwOv7MaE5JzKzLKY4MdZAv3xS%2BoDskI4WTcONKu5nBiwn9S6HF9cqmkWj%2FTlHmR0BjsL88w%2FFCMICK0scGOqUBCfj%2FCE5b%2F1NDk7Wrkq%2FWJeVvE1OgEZppM%2Ft4DJi13sixfit91GJCr%2ButkzOlZNKi2uhl%2FCv9s8McsajuSIXUqPxmDqA4EP1eyiHm0fJZ%2BuzJzIJnLU2a3CcSqTny6xNfVKdqOjyp9G25K8WzWRAd%2FaTof1%2BsqYljjIdgcIQd7rl0BciAQyubvavAANdU8F%2B9F8KEDnJVX28BHSikxfhtDpRcOhSR&X-Amz-Signature=59d6c64ae4cab4475102a0650a1a8aa76a5cfdba9de9692c1e35775602e74e92&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

