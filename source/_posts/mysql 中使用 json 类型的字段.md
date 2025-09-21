---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ANEEPBB%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T170037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDubH6jOgzMPc9w73HTOqSYMpixrzhtem3iaZsvw6SCNAIgX%2BWBLy5jMT83f4EJGXzFFTjIYtbvT0U%2FVCKFNdJgw60q%2FwMIGRAAGgw2Mzc0MjMxODM4MDUiDNWYWYSE45s3HfXXmSrcA4KWy7E66SFA%2B3LUGs5j3HXtPJmlK%2BBopF3u%2BTHnXJ6KQpYOmS9kHN95o299uSXHImpZexqY%2BBY6iACvv54iSXGrAqMT8xYZLABP6S12YGyVBEGeCS00WCP6iWYmq9DOwsyRCTzx2gOI3lGf9za6kLgbKc7Wr%2FLmIdNcOG21ohEarwp7D%2BTsJcoH7wDfxHxErZEdpNOSWmuYvq%2BEY7UsDP5nAD45vV7a9QA3MQPSWrd4RJEH9%2Fegprd0bJuWA6%2BjqiAVpH4U4rfIruePXwXwwowH1wLy1ttX3tm5rCFJHdz99XROjJWD%2BJE4UsVS1jiQzQ%2BBMZcr3GNl4gQ%2B6vfTFVO0N8KBY3v6wLQNc83vD6ZaXeQDQ3BCZcFBqGHHfgOZGKCKqmdKeWHCfUAUJYBkK8ViW%2BUC%2BxAqdXtHdiEQKn4gx9Xo515o6qTQcNHs%2FkGLQAt2JSto8A58RzqUpzJR17ylDsE%2FJBKAaDAMzstKZmLpze9WS8HGm%2Bxj%2FNaKY%2Bp9tjGU3%2BcVB2p7uJsfL6T7SsIaJwDQsvu7UWslU7SFAFoeHxZmT9i5yAyN0B4RLP0oeFDIVETwloyTdGZEtGaA%2B5yyMdWf3AjHdBOXKgTE62iXcYgz06e3zvSwaoGFMKy%2BwMYGOqUB1wTWgT2mxVFRsza4sjd2BIhhjJMjpu8LiD6fS0t8pZJXnJm5vhCRn92jITmL2XEg%2BEZ4h6FaZR6bAu6FwhyBeQsLj0Evrgtq8rlU0cJimVUaKe2w%2BSIuVT4uNtw8IWtvFnzzZEcGIpmuVOoRLzMy8kj0h0jQE4p8Qncb8InboDOLDvezcJp7xF89tYH9qmFsd6s39wJwAJFzAmuEIhLF50%2FRW5pv&X-Amz-Signature=1184c9ffa6874661f663792a8e9714eaa976b7f0297d03ee20de07c39c54e294&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

