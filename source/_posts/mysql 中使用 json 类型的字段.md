---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666VCDJGOU%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDAzLArQ15PfjLFVE2cCZU%2BvnxTb3Xfl%2FLoFOATxiA3ewIgBhykf5wrZef7%2BwPQy341fDXAjK4eNdzHCEFaktG6%2Fkoq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDARmG8AmPJE1mHQXFCrcAzSDURgOLJup5JoixQXkw%2FHi58rMu9bgdvbXAOsMxu1ndE5DukGyYL7ggUfCoIixvMyNooLsLJp9HhSl2xA32vrxcgJBE6xQ8cUJ1lwmJ8BEhUw6rAlqS1gHokO%2BPwiN49O%2BBK89y1fL27VxM%2BhasBgBywrFXscjs1KBtGKDh8kX%2Fu%2F1iRV2yGi9%2BQwTQJOYEDK71CNyDDGra4AoCveQafORkxb8bMB5T1PpmpP1v0UBtXvQmQTq3i9JSJmrvnTqmZqH7V0cQDkSFi9Ayg%2F%2FTwqeTMpyq9WAqn6m7nThFUBCFBspXxw%2BUAtFtvwVowklA1oLSyjDcrjrKIiZpVgbe4adqRYq%2BmM%2FNtjWQZ2Upjzxoz%2B8zJxmqImc%2BwtogX6XkdHskf7Wkc9NfAonpFALaMuaxhU%2F4p2iQD9ueN4P3UymHZZsZW4i1Y7Qujm9IyO%2FRZqPULCPVl%2F0BB98hD1Dq%2BsalXu1Rt0fekvAKeZ70uV4s%2Btk%2FMtUGEtjXKZygHjLIyvOO07DqAfSZZGQ9z8c%2FeJIy6UiD%2FtfPpPNNDWk%2BIOZYD7tBtervFrCbsR5OfCETaIXLWQlMJ0SwAzfloJf4Nas%2FYnvGkfTFfNAohdTkmslPx8OGppYqj8OI6T3MKe7n8gGOqUBshIg%2BRnRVzRsZL7CAbAdx3tHT89H5cA3Y1tsWLOd5789f6JM54IcckRLWB9g3D5viFe47nzye7tarA58fVB7Iyv2po7nmzGXjKwBXdyd8sgXkgS4Set0GOe52EQ4%2BNepm5WUBvNg54F27wCPSIvlxqb1lDHVCPjqZNn0l9SJOYFn9PUDUhRo5XChir1Q09FofJuFyu5cesArbWiXOXrwjgB7WA2I&X-Amz-Signature=0f85c7df40bed8904093bb29957aab65a490a3ae31438ab02d70f68e0f0beaf1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

