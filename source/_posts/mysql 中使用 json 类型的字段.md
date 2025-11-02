---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RSATOVMT%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDxCbxXONySD9sDGpAiD6c5wU9xn2gKZ3Ys5AMWSukFqAIhAI5CUGOKlWvFixH0CKX3MYv%2FboYMycBMg9IIDTmtA%2BCIKv8DCEwQABoMNjM3NDIzMTgzODA1IgwgkrCzhi%2F4bVV57cMq3AOCzHM%2FXCrfHtH3rGPyRcC0AchHkfOqcnq4P49H8R7XYFJIpMfEb%2Ferrp8CA6tz4GooKCc6dxMwYMuihR2N%2FAbl9HT9DRihy0%2BNwdvjDUrHrbPryR1w9IK16nhBPBnyt3%2BWCBZi2BNGptN5ctyfgixTAk6GVBzMkQZNg7snmbj4OiIrSetUzQRmqt3ssrL36mt%2Bt1dRLQ38EV65sW%2FqdXGd7wDuOoq0%2Baxg4lltEPou6AxHixi%2FAxr6qgILZEwGQ8bG%2FdMnv5G0QgXoHHbqBBc%2FKln%2FJonRv2LEZ9hPEFbqOgapXUtdDz31JDOf0Xjpm0jv9wbBpR2MMQDG%2FDEEizfj4nnpqxFcQGFith0732SNhl2m8tocwEhUBbdFu11TVbvDT8qBrxcVuXz1qRxnRQAJSrQO4qeS1RNZ0ze5C5x3rbPqE83iWOjXmB1Qq1%2BNc8XzOM%2FJIM%2Bm69Fj4RNY3XFkyNaRNsmdjSO6pI3p1hoyNoK3eciPOR7qA7CBVOHtSHSBJSUi6gOsezOx%2BYVakqlKNVH%2FzYD0adG6nrU2C6LZxlXvZwEqJU3hIIQokoN%2FSq0JCruRH9IFAiBFwMi7QeJ6A%2B%2BvnHiTgRgvP0IpwEJ8IZBl8zrxRZFyftxIjjCl3Z7IBjqkAVF5SPTotNK8sxJzMz235mpnS5YNv45Jzv9WN1dPf32AfYVJBeSU2kb%2Fq%2Ff8GY8KlHT9fYdRD7G2C47bCowtz%2BVbja0BartMrjBr%2F1G65vxY2gfYxqzp4XqDOup9p2PmGjbzXTOC8UxbtuW8bmPQDRISpRluixR74QSMsiwcg9w1lhtm9I%2FpDYNKKcBxpuR4yLMqnrblkDgWxh2T%2BIK1zAoMUqnh&X-Amz-Signature=943f66710b954e06aa759419d2ed038475f186bd87b8c8592afa889fd13b0170&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

