---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RWZF4JZG%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T040049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF7HaATBXnAhNYhWMiDQM%2BFB%2FFm6vfyr0AlrfUeQLhheAiBIThsv3EVkFRMniwa7oYmkXOe7BNwl9UPl%2BKrARTModSr%2FAwhcEAAaDDYzNzQyMzE4MzgwNSIMn%2BUrRB00d2lLFRkXKtwDoURwnKaUZ0JTj4SG%2B4eHPRhO9j6qBTJvBlQAKRlK3P%2FR4BLDeZDxX9nRta6DTJI1Ab5PudVBBnbyky5P9sb5dkdsjtgenBSpp9OXJ8%2FVAKscfaERqzQS6L1ViFXNtbDi53SBEsPHcpKsRLdCdLv3Euxtia5K3x7xckNG3KvDaGJB%2F1x970N9gaFYH7bWtbH4HXYE1gGEE88yC%2FYr0sLjDijtFy%2BmURxgbjIt%2BaaOMGkO2lE97Xy0%2BsnyWV3GpAVmFN5sZ2ZhPR79z7FQYlJAju3BxDbLCRpUh0xbHSzrpzxZ9Lfv7SWIdoiHNDN34NRfrUUwYVYN1sOo8%2F75mgV5jqJUfOollWm%2BxaahiPfs4n5d5tnFAU7Q4U8Yfiu5eZD%2FsnF7aXxxWvWrZrr1ZSojwygsPhV1m5PZpZswXKMQlJVUJ3nNcukcPSwsHDkc6N5dBwFTTiuwtyoki02EaoifhoCD09KQ%2F6Ef%2Bc%2FhffZ51F6TN%2F8gyUVayJgfAV%2FPG9XM5%2B%2FZPs7ff0gw1gTINJRgz1gKdfZs8JouAOXGUEylVPgrlqpq86py%2FU73W3ZSArWtEnrimR9Kj2eShmJ80FOZWQoLefLElUoeIWR6DO1wvfoX93wR7pFB%2Fa8YlJkwtLrayAY6pgHAKJ36P7k%2BFKK8eXiL5zUodgLwwEOJsmgAPvlUAOcbIGfTMJ1ChHDHRPY5VGZ6P7LRO0MJvOcY%2F%2BC7vh3K3zk1L4odcmGszLOHD4TJurUr%2BgK80PfaP0GP2R6HFc71dbulFvYN2JyBi2kdN%2BLXqshvPXhfO94%2F7%2FbCwWyteEcpk1IVZ9rWWFmhvGvvbaL8M5kodRzkBuUA40x%2BL999u%2FYkLzzp%2B4nk&X-Amz-Signature=5ecf0cad71892e4050943bbff619e96baa1a16ffbcdd89d3d40c73ce665df7dd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

