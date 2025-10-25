---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667EMVGAG5%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T150108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHZlL9M7ELKHyqsLCwXXNvbxX%2BGVX%2B%2BwuIG%2Ff5LtXO%2FEAiAm7gVz0uLvwN%2F2webRuiTMVAQi2nZLeKxV8jn%2FSjcvCir%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMg9pQM8CvnZS7s6oYKtwDHr2iVCbXMbCKq30ztH7RZJy%2FOodqgk9jM5gQYpmghhbNKRWcvtVrAK71xVt77IP8whI7BBKtMwGq7gsUjpxIOAkewh7yKEeKiwQ%2BhCXTsv3NoIOrodtsE0sCdOhRtDxC4ivuPg%2BjUUTwiZzG1MzWI637wQ%2B4KFU4CQs66DUAAp95o2agPMPJP%2Bk%2FdiLxPblsk7w3inTVEaK%2BCs3ARSKbc1yQZ0SIGELY7IiP%2F%2Bb5jl1DKlpY75xenE5sAYOgA3LATWQ6BZWSZnTjycQ7%2B7u6vuodf7gfhO0WqN2WRCINJ%2Bfu6IDly0bT33D8hzrpVZSgToQsQTNoS1fLGOXSWmrNxUwI6th6pNGoZyfPNZ9Ir3Jsm3D070P6IsZr7%2B1hg8w3LtbJx%2B%2F2XugKbpaleyP%2FuBEiIXhyF2%2BCZNu9t2wbd8juS8EFfMpTAarxDMQ2KReEIIkZA3eyF7rCCkvsSVOwiCUQ6lhnyLAQdhMB4nxfoaQ9sI5TRYWuNtKZ7%2BEaY3fOJSZzYybpfTWWhtF2ZH1YDrdNm0oxV0Y3txo7IBsPRaJ3i8S6RMk9NvM0SJEDy7A%2FLIISJSzM1JmXtopXgq6%2BUpC5SLp%2F9Hc2PJBaM3h2md7WJodxe%2BLr52Bl%2Bbcwu9fyxwY6pgEkpMq05Ky2H5qzfMWsLOPbJr%2BdpVNPmFrpG%2FlIdAmMkd48W2Z8%2FyqCiyhUo7i4jc5YcwbUV3%2BYJQSA%2FC7fVCv9U4yzdNwJ5Re4lDAh9KHrmePzFaBbhZyB87Qi8Seeee1QsYt%2Fx4vE3%2B%2BEeNAVqjXKxFCLczy%2BnnDz2zR1PurwLVjbx86d9SzCM6%2BaeB1GxZBqXZWfFuD%2F2LdYti1wkMKuYAX1oJyT&X-Amz-Signature=e1b2aea68a5988ce9456f1c5d02bd01ee2ad535ce75b77701662451b35297701&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

