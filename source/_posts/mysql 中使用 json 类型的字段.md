---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZCADVYM%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T080038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQDTgI3%2FBDc6sgkz%2BEjIrTDH2pOD6SJJIUX0A1RvoXQ%2BHAIgaEAc0JpCyarsHSoYebcd2oiezbvXmvWTTgYgqOjPjBgq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDNmkEvtiHjquWOm0XyrcAzVBXqTOUuqsPkWG2URwPCSWxuWArgdn2YpIJaRJ0w4MAlrh02qtO132wu9j2zzS1pQIKOb4LYZBolCkReAZKcMb8unjzkd2MWfwYkO6XjY1bn8pAO0oeFlz36LBMpGLCLIk9P5Go2JuKyb7Aoi%2FhYCiAKyzja5b8HZjFPDPS%2FNdOykja2J400uoE1HERsF6x37Ju%2BL9jtIM2zaqEgXJKDjZmW2rVYXE03dp864oapM8kyYmTTUj72QecQLGb9opYD%2FOtFMGGhdGU%2BqUY8X4j763prWmBCECbzNpQo1xv82gpPUVrkHAE4gGRogYATJfELR%2Fk4O0vK6NBt%2FNwu4E0xHHVDpgQa7NGlrG6iFUV8rQGsf98AaTopxp3m8RQjHBvKqCWhK%2FT25VusRJ6iq87FoyYvuLEkYdJ%2Br4OnIPqGplcG4989NDl16xIw3vGGowEo8IGFf2fJEqI%2B8Olf%2FPukYuJEraQGsaHJemhKVCwrdGrrOaK%2FsejzhMQOAqXQ7rYA%2F3STvjP4rlyJ5xhETa5ULEqWhdQAWQCQLUO%2BmY1yfgpYp145PYqLOKyhSq5YbWkkU9WnzF%2BqwmkcHXnEzjj1rwX8IEULzQrdenecm6wEEGogWxJju%2BGc%2FAGk8HMM3Um8gGOqUB2EIP6cvrVFCRkaH5OPQJvaqRhx5NYwkrNKhmCGvhDCozknT4sDfYIvb6KgM5kEhacwwXfhjcKB6M8dNsJeUk2dBILmbgkIgE5w%2B39ZTfsA2JXxZ8gyTWjEvai%2FrunrbXGmLn6iiPPxVAE2dkNkt%2BAevJDVDgfTWZLir4rNYhww4s3SB%2FzG7cCeL9XcRzRZz1BF96rTap0hC6sGs%2BJMDIkhcl1qSs&X-Amz-Signature=6a978d4d92f3aae6b498bc61ac466e58bb900a56a5c5d1c4b8feeb7d17b4f2fe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

