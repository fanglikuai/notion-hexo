---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZGKJEVGH%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T110037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJIMEYCIQCUqIgPrJvcpSG8SXOn9U34giEPGHD8H%2BplKw8H4ACjkwIhAP1FFa3B4CfLSfWxvytChbtNAi%2Fok%2Bexe%2Fz6hOCStwnzKogECKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwUZUYa1YUo1YDyfykq3AOjHmS3w22cr%2FanbLAWNSqqnRYZNwAfx41zC7wDADayNyU35bKkYdGOcD13PW%2BlAiVNKmwTuh6jdm%2FRcbVoVDi%2FJcb0UDn9gEUviC05zxjB8hd02Zi6wgJBSgE3wITNdRkkV%2B4QJwf%2BZEgGz7i%2ByZhFfBwWdUGtp2NydAUxSa8c4XCKsjK8u54AOEl83k58xTWEAW3oTpYuU4vA47Z89COJ3GP2hG8utRXKS%2BPLRed%2F59DBioGlQNhrmWzLzCN%2BUw6igX3%2F9tJg7pmj%2BYrY2UQCH8%2FfNuQctPQs0rnEv%2ByD3ko6LdnGrcPvpyAppA8S%2FgmEO6Pj1Wra0YLs0vDqnuEpMj5ClcMrT2Rq5bwxF8fKzHpvhrno1Z8NU40FopnMKAuE3CHgJbGIBFFZUD%2BGofeELn9QXZxfpoQsqBh50tK4kiiM5F8dqhRi0nnSS6ADG0xV76lIfYpplQR0aWTzjjm%2FkTX6HfWDPPJgPlNkhYgmwUPzJhbI%2FucLwtnyp1OJCMqCEfz7%2BZMNhmkrxMDR7gRYeXlfV5cQMWbo%2BfrxFr1xh9zf8FBj00jhf%2BqfHzwRUShp8vF1WbUbQS%2BIfp0hxfABeGMevXYLMJ39p%2Bmf2Rpwdl7f68mvDm8M2a%2B05TCW3ZPHBjqkAYc1C7gUsfnO%2FzvCuPbk7UTz5PpS4TTNXgobfaUSOKAVUVj9LppK%2FkVdTAsnJ92JVf4pF6oWlNdNam2ViLfYTSNHoRL2IpEghk6o1Qils3%2B035COa5fL5eCBM3yYJKq08Q96JScSmanmt3iDifSXuJTYi7lF%2FQwaRpcHIz4fJlitjqBKUAXqFkM%2FbGo52hRMWA3%2FvA3PvLF2sIYjOvCCGNQ1Iti7&X-Amz-Signature=9c28913ee6b2f2f6b711423ced6b9d3beb93661698bd8639be7cccba76cd4444&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

