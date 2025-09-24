---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SMRTDH6K%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCqZw6KZEmtNkx0xVFzYpm1MCyN8D0DjO%2F%2FmU7JBcUUaAIgeQfmJrC4SKR9raoFWaaQ0SuLg2ELAm7xBtpKWTNMn7kq%2FwMIYBAAGgw2Mzc0MjMxODM4MDUiDPUiASJQp1OXz53w3CrcA38DODqdcPZq1HO3sPmdfjHiUpykWTH5MgWvhvp8qRavTukc7EEcVFn7bClUS7K4Nd42uBMfn%2BqE1ABdht%2FlHnyZ0n%2FJUZggnPU8hcbgK2%2B%2Fmb%2FyPHJMmoZDvbwUF0JVXwm%2F7JrNRmHxkOEyAJ2Bt%2BE58criEbryoeeJrcitewLGjvqiII9f5K1irmX%2FzqzLQzuOHb9NznV8ygjQ2CL%2FCGhQNyqEg1M929vYN37SMkupB6keYDyBCN7DJlTMOPTqdkPRdvwovJAP%2FOYhtNwI7Ayj39UEOc6JuG6FJu%2Fvc7%2FVblcdDbAq6%2Fop3QP9azy7vTOwZAdapBOFJZxj%2FCQU%2BWLl2ah%2BTsXCVF0KfMmI1DtgnVRqSN5h5K6bz516KZhjTi78AYx991mVlSO%2FTkplHaVSksVBnf%2Bqnq%2BSHbFW4lcI4J2beGpTxTeCOdlKHH%2FokJrwBJN93%2FKemhEJuEWxmdzHLFYOjV%2FQ0H8M2sPOyI2RVq9N%2Bpg56KR7Nat4mWEpMLGxFYtCFO7kB%2FcN7PGJpCkbkOc5cQusvOHSoMel4JEkZKEWzrjCAqiTMvT3jHm6Sp6TJLWHOLxYbvbX3%2F%2FQj2AVIDd%2Fn6pKuzQoRCjDzgR%2F9xuinjTcs4R24iVHMPSX0MYGOqUBstakHI1O5htNayXl38vc%2FZwAUrsOeLjFFufwoYo%2FtfzLGa5aIti442ZmVF3gZ7kjC8l0fJyY9cv%2Fg03u9zKY6mv1TJdrcXwN7rSVBAzS6DtC2UAah5do81l7D%2FhhW4hJehTu3SJEnMJ%2FNSpJvt0xYTDukLoy4MMBU2AmAiaxQk57q%2BGWf13y4P75RHCMK%2B2%2FqnzMcKySHGtqUyBCyRb%2FguYuU9Md&X-Amz-Signature=89851ca774e6c2b89bbec4549381fc215a3f2f14b39707d4869225dc6d95cf84&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

