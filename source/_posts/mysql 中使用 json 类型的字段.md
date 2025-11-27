---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSZ4NVRM%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T160042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIF7MKLQowdzFWKvDEt5v9KrIt3IrbNkzctTRsZ74MWOgAiEA8SMJf4VAbTgbVNIpBOOTjayNLCq%2F4UIv6pJnGgcIsp4qiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPil8G0nqj7lcqaNbircA4AkEiEfrIJf8IcXogxpda2VxTR74ZA08EJeXNijUtP3zTF3t%2Fu6tlTTUF9Lcf4A7Peg0zkwcnEfXIblHmXYw%2B40eD5Gj6u38lUDnxezyM8bEx0%2Bj7qwY7xRTRuqtvdWrqR%2BK1UA%2FxdgIZeObrCXud1oyYNTuWJCtpkkzGGVCmWg6%2FZcrt78JaTVcQ%2F0FKHmiLkxds0lwfdWBDElA7f2HfYaFdmQUV6O7wPAz2wEQcYGddic99EmEwJLPU5gIjOR%2FSHMiT5LMJLw40aLHWiatOFiIa%2BDg60EkoXK0PHjRsN8InwY8drABqjPWzO7rAu7QFxU5%2FALotlYQ7tXO2OMtN7J84FQ75w2MpW7rdbsMxQqlH4xT2B%2FezU9acrBII%2FsiB9nn3dp6SGp2n3Cyf0q0GZ%2BKr7Smi5B%2F8sV0acgBPYeKn29Ws%2FSNKp5u2HQsal8IfGr5ay1fETxBK5COduD2mk6p7n10ru9KYM2JgnVAh8cf7LXUDpIfZ2Bu5qvNJ2Wy7YH%2FdtaRfQ5o2nk6ah0n0lMDvEz48zP1Go%2FbOciViek6%2FbccmPDiBiD7onS4BDpcqGUOhK3k2pgYDRUsx%2BAbe%2FMHPKIQNobiiFYUSvhVX%2B44PTX6fiKGukR8PxkMNihockGOqUB3iWlpKSxMz2ny5NuuHzpL0V4iDjJvBjjM08NW7MAqnUDdTsAslzD%2FB8OyU%2B1u0DY490KvTaPHeT5rxXdiOCc907PD6Ujr%2FoKHZPhfWJTYH2%2Fbe7xkTRg0Rg4mBobxXR7nJB4gtX5%2F0wWp2wXqh0mjd8et%2BUt7%2FFmsrL17RtDcJOXzYqtD62bRrrSEkR1FWnpdEy8rop%2B2EyQx9QVyXA%2FDSlO70vb&X-Amz-Signature=270ccc2478ba17d7cf9b4a1e6e43aacbd8f2c4496befe5d836bd92172176d5a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

